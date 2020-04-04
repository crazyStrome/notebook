<!-- TOC -->

- [sync包解析](#sync%e5%8c%85%e8%a7%a3%e6%9e%90)
	- [Mutex](#mutex)

<!-- /TOC -->

#  sync包解析

##  Mutex

Mutex是一个互斥锁，当Mutex是零值的时候，它是没有上锁的。

Mutex不可复制。

以下是Mutex的定义：

```go
type Mutex struct {
	state int32
	sema  uint32
}
```

文件下还定义了一个Locker接口：

```go
// A Locker represents an object that can be locked and unlocked.
type Locker interface {
	Lock()
	Unlock()
}
```

以及一组描述Mutex状态的变量：

```go
const (
	mutexLocked = 1 << iota // mutex is locked
	mutexWoken
	mutexStarving
	mutexWaiterShift = iota

	starvationThresholdNs = 1e6
)
```

