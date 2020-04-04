<!-- TOC -->

- [1.  unsafe包](#1-unsafe%e5%8c%85)
	- [1.1.  ArbitraryType](#11-arbitrarytype)
	- [1.2.  Pointer](#12-pointer)
		- [1.2.1.  使用Pointer作为中间者将\*T1转换为\*T2](#121-%e4%bd%bf%e7%94%a8pointer%e4%bd%9c%e4%b8%ba%e4%b8%ad%e9%97%b4%e8%80%85%e5%b0%86t1%e8%bd%ac%e6%8d%a2%e4%b8%bat2)
		- [1.2.2.  把Pointer转换为uintptr](#122-%e6%8a%8apointer%e8%bd%ac%e6%8d%a2%e4%b8%bauintptr)
		- [1.2.3.  把Pointer转为uintptr再转换回Pointer，其中带有uintptr数值运算](#123-%e6%8a%8apointer%e8%bd%ac%e4%b8%bauintptr%e5%86%8d%e8%bd%ac%e6%8d%a2%e5%9b%9epointer%e5%85%b6%e4%b8%ad%e5%b8%a6%e6%9c%89uintptr%e6%95%b0%e5%80%bc%e8%bf%90%e7%ae%97)
		- [1.2.4.  当调用syscall.Syscall时，需要把Poiner转换为uintptr](#124-%e5%bd%93%e8%b0%83%e7%94%a8syscallsyscall%e6%97%b6%e9%9c%80%e8%a6%81%e6%8a%8apoiner%e8%bd%ac%e6%8d%a2%e4%b8%bauintptr)
		- [1.2.5.  将reflect.Value.Pointer或者reflect.Value.UnsafeAddr的结果从uintptr转换为Pointer](#125-%e5%b0%86reflectvaluepointer%e6%88%96%e8%80%85reflectvalueunsafeaddr%e7%9a%84%e7%bb%93%e6%9e%9c%e4%bb%8euintptr%e8%bd%ac%e6%8d%a2%e4%b8%bapointer)
		- [1.2.6.  reflect.SliceHeader或者reflect.StringHeader的Data字段同Pointer的相互转换](#126-reflectsliceheader%e6%88%96%e8%80%85reflectstringheader%e7%9a%84data%e5%ad%97%e6%ae%b5%e5%90%8cpointer%e7%9a%84%e7%9b%b8%e4%ba%92%e8%bd%ac%e6%8d%a2)
	- [1.3.  Sizeof函数](#13-sizeof%e5%87%bd%e6%95%b0)
	- [1.4.  Alignof](#14-alignof)
	- [1.5.  Offsetof](#15-offsetof)
	- [总结](#%e6%80%bb%e7%bb%93)
	- [参考](#%e5%8f%82%e8%80%83)

<!-- /TOC -->
#  1.  unsafe包

##  1.1.  ArbitraryType

unsafe包下定义了一个ArbitratyType类型，代表了任意的Go表达式。

```go
type ArbitraryType int
```

## 1.2.  Pointer

Pointer定义：

```go
type Pointer *ArbitraryType
```

Pointer代表了一个指向任意类型的指针，有四种只适用对Pointer而不适用于其他类型的操作。

*  任意类型的指针值可以被转换为一个Pointer
* 一个Pointer可以被转换为任意类型的指针值
* 一个uintptr可以被转换为一个Pointer
* 一个Pointer也可以被转换为一个uintptr

因此，Pointer可以跳过类型系统而直接指向任意类型。所以需要十分小心的使用。

关于使用Pointer的规则，不使用这些规则的代码是不可用的，或者在未来是不可用的。

###  1.2.1.  使用Pointer作为中间者将\*T1转换为\*T2

前提是T2的大小不超过T1，而且两者的内存分布相同。

```go
func Float64bits(f float64) uint64 {
	return *(*uint64)(unsafe.Pointer(&f))
}
```

###  1.2.2.  把Pointer转换为uintptr

把Pointer转换为uintptr将产生一个指向类型值的int变量。常用来打印一个uintptr。

将uintptr转换为Pointer是不可用的。

因为uintptr是一个整数值，而不是引用。就是说uintptr和指针没有任何关系。可以说是将Pointer指向的地址的值返回给uintptr，即使uintptr中的值对应的地址的对象更新了或者删除了，uintptr也不会改变。

###  1.2.3.  把Pointer转为uintptr再转换回Pointer，其中带有uintptr数值运算

如果Pointer指向一个分配的对象，那么如下转换可以把Pointer指针向后移动。

```go
p = unsafe.Pointer(uintptr(p) + offset)
```

---

最常用的是指向结构体中不同字段或者数组中的元素

```go
// equivalent to f := unsafe.Pointer(&s.f)
f := unsafe.Pointer(uintptr(unsafe.Pointer(&s)) + unsafe.Offsetof(s.f))

// equivalent to e := unsafe.Pointer(&x[i])
e := unsafe.Pointer(uintptr(unsafe.Pointer(&x[0])) + i*unsafe.Sizeof(x[0]))
```

这可以用来向前或向后移动指针，通过加或者减offset。指针移动之后，也应该指向该内存范围中。

---

将Pointer移动超过其对象的原始内存分配范围是不可用的，如：

```go
// INVALID: end points outside allocated space.
var s thing
end = unsafe.Pointer(uintptr(unsafe.Pointer(&s)) + unsafe.Sizeof(s))

// INVALID: end points outside allocated space.
b := make([]byte, n)
end = unsafe.Pointer(uintptr(unsafe.Pointer(&b[0])) + uintptr(n))
```

---

当然如下代码也是错误的，因为uintptr不可以储存在变量中：

```go
// INVALID: uintptr cannot be stored in variable
// before conversion back to Pointer.
u := uintptr(p)
p = unsafe.Pointer(u + offset)
```

---

Pointer必须指向一个已经分配好的对象，而不能是nil

```go
// INVALID: conversion of nil pointer
u := unsafe.Pointer(nil)
p := unsafe.Pointer(uintptr(u) + offset)
```

###  1.2.4.  当调用syscall.Syscall时，需要把Poiner转换为uintptr

syscall包下的Syscall函数把uintptr参数传递给操作系统，然后根据调用的相关信息，把相应的uintptr再转换为指针。

如果一个指针参数必须被转换为uintptr作为参数的话，这个转换只能在调用函数中的参数表达式完成，因为uintptr是不能储存在变量中的。

```go
syscall.Syscall(SYS_READ, uintptr(fd), uintptr(unsafe.Pointer(p)), uintptr(n))
```

编译器处理函数调用中的指针时，该指针所指向的对象会被保留到函数调用结束，即使该对象在函数调用时并不使用。

如下是错误的代码，因为**uintptr不能保存在变量中**

```go
// INVALID: uintptr cannot be stored in variable
// before implicit conversion back to Pointer during system call.
u := uintptr(unsafe.Pointer(p))
syscall.Syscall(SYS_READ, uintptr(fd), u, uintptr(n))
```

###  1.2.5.  将reflect.Value.Pointer或者reflect.Value.UnsafeAddr的结果从uintptr转换为Pointer

包reflect下Value的Pointer方法和UnsafeAddr方法返回的是uintptr而不是Pointer类型，以便于调用者不使用usafe包就可以转换为任意类型。这也意味着，这两个方法的返回值必须使用Pointer进行转换才可以使用：

```go
p := (*int)(unsafe.Pointer(reflect.ValueOf(new(int)).Pointer()))
```

因为这两个函数调用的返回值是uintptr，所以也是不可以变量储存的。

###  1.2.6.  reflect.SliceHeader或者reflect.StringHeader的Data字段同Pointer的相互转换

前面说过，返回uintptr是为了调用者可以直接进行不同类型的转换，而不用导入unsafe包。这意味着，只有当指针解析为切片或者字符串时SliceHeader和StringHeader才可以被使用。

```go
var s string
hdr := (*reflect.StringHeader)(unsafe.Pointer(&s)) // case 1
hdr.Data = uintptr(unsafe.Pointer(p))              // case 6 (this case)
hdr.Len = n
```

通常情况下，SliceHeader和StringHeader只能作为\*SliceHeader和\*StringHeader使用，而不可以使用其结构体形式。

```go
// INVALID: a directly-declared header will not hold Data as a reference.
var hdr reflect.StringHeader
hdr.Data = uintptr(unsafe.Pointer(p))
hdr.Len = n
s := *(*string)(unsafe.Pointer(&hdr)) // p possibly already lost
```

##  1.3.  Sizeof函数

定义：

```go
func Sizeof(x ArbitraryType) uintptr
```

直接复制标准文档中的内容，下同。

> Sizeof返回类型v本身数据所占用的字节数。返回值是“顶层”的数据占有的字节数。例如，若v是一个切片，它会返回该切片描述符的大小，而非该切片底层引用的内存的大小。

##  1.4.  Alignof

定义：

```go
func Alignof(v ArbitraryType) uintptr
```

> Alignof返回类型v的对齐方式（即类型v在内存中占用的字节数）；若是结构体类型的字段的形式，它会返回字段f在该结构体中的对齐方式。

##  1.5.  Offsetof

定义：

```go
func Offsetof(v ArbitraryType) uintptr
```

> Offsetof返回类型v所代表的结构体字段在结构体中的偏移量，它必须为结构体类型的字段的形式。换句话说，它返回该结构起始处与该字段起始处之间的字节数。

##  总结

1.2中的Pointer和uintptr的区别：

假设在内存中有一个变量`a := 1`

那么`p := Pointer(&a)`中，p包含的就是a的实际地址，假设为`1000`，当a在内存中移动时，p中的地址值也会实时更新。

而`uintprt(p)`只是`1000`，就是a的地址值，但是当a在内存中移动时，原来获取的`uintptr`值并不会发生变化，一直都是1000。



也是因为这个原因，syscall.Syscall传入的uintptr如果代表一个对象的指针，那么该对象在内存中是一直被保留的，而且不能移动，否则的话uintptr指向的就不是原来的对象了，容易内存泄漏。



还有一个就是uintptr不能保存在变量中，只能使用Pointer进行转换然后才能保存。

##  参考

[题图](https://github.com/rfyiamcool/golang_logo)
[Go语言标准文档](https://studygolang.com/pkgdoc)

