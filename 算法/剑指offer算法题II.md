<!-- TOC -->

- [面试题40：最小的k个数](#面试题40最小的k个数)
- [面试题41：数据流中的中位数](#面试题41数据流中的中位数)
- [面试题42：连续子数组的最大和](#面试题42连续子数组的最大和)
- [面试题43：1~n整数中1出现的次数](#面试题431n整数中1出现的次数)
- [面试题44：数字序列中某一位的数字](#面试题44数字序列中某一位的数字)
- [面试题45：把数组排成最小的数](#面试题45把数组排成最小的数)
- [面试题46：把数字翻译成字符串](#面试题46把数字翻译成字符串)
- [面试题47：礼物的最大价值](#面试题47礼物的最大价值)
- [面试题48：最长不含重复字符的子字符串](#面试题48最长不含重复字符的子字符串)
- [面试题49：丑数](#面试题49丑数)
- [面试题50：第一个只出现一次的字符](#面试题50第一个只出现一次的字符)
    - [题目一：字符串中第一个只出现一次的字符](#题目一字符串中第一个只出现一次的字符)
- [面试题51：数组中的逆序对](#面试题51数组中的逆序对)
- [面试题52：两个链表的第一个公共节点](#面试题52两个链表的第一个公共节点)
- [面试题53：在排序数组中查找数字](#面试题53在排序数组中查找数字)
    - [题目一：数字在排序数组中出现的次数](#题目一数字在排序数组中出现的次数)
    - [题目二：0~n-1中缺失的数字](#题目二0n-1中缺失的数字)
- [面试题54：二叉搜索树的第k大个节点](#面试题54二叉搜索树的第k大个节点)
- [面试题55：二叉树的深度](#面试题55二叉树的深度)
    - [题目一：二叉树的深度](#题目一二叉树的深度)
    - [题目二：平衡二叉树](#题目二平衡二叉树)
- [面试题56：数组中数字出现的次数](#面试题56数组中数字出现的次数)
    - [题目一：数组中只出现一次的两个数字](#题目一数组中只出现一次的两个数字)

<!-- /TOC -->
#  面试题40：最小的k个数

[leetcode](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

描述：输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

思路：可以使用最大堆，或者用快排的思想，每次排一下，判断当前base的位置是不是k，如果是的话，就直接返回，负责调整区间

代码：

```go
func getLeastNumbers(arr []int, k int) []int {
    if len(arr) == 0 {
        return arr
    }
    if k == 0 {
        return arr[:0]
    }
    if k == len(arr) {
        return arr
    }
    start := 0
    end := len(arr)-1

    for {
        //循环终止条件
        if start >= end {
            break
        }
        left := start
        right := end
        base := arr[left]
        for left < right {
            for right > left && arr[right] > base {
                right --
            }
            arr[left] = arr[right]
            for left < right && arr[left] <= base {
                left ++
            }
            arr[right] = arr[left]
        }
        arr[left] = base
        fmt.Println(arr,left)
        if left == k-1 {
            //循环终止条件
            return arr[:k]
        } else if left < k-1 {
            start = left+1
        } else {
            end = left-1
        }
    }
    return arr[:k]
}
```

#  面试题41：数据流中的中位数

[leetcode](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

描述：如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

思路：将流中的元素中，中位数字左边使用最大堆储存，中位数字右边使用最小堆储存。取中位数时，当总个数为偶数时，就返回最大堆的堆顶和最小堆的堆顶之和的二分之一，当总个数为奇数时，就返回最小堆的堆顶元素。插入元素时，如果当前总个数为偶数个，则先插入最大堆，然后弹出插入最小堆；度过为奇数个，则先插入最小堆，然后弹出插入最大堆。

代码：

```go
type MedianFinder struct {
    min *MinHeap
    max *MaxHeap
}


/** initialize your data structure here. */
func Constructor() MedianFinder {
    return MedianFinder{
        min:NewMinHeap(),
        max:NewMaxHeap(),
    }
}


func (this *MedianFinder) AddNum(num int)  {
    if this.min.Size() != this.max.Size() {
        this.min.PushHeap(num)
        this.max.PushHeap(this.min.PopHeap())
    } else {
        this.max.PushHeap(num)
        this.min.PushHeap(this.max.PopHeap())
    }
}


func (this *MedianFinder) FindMedian() float64 {
    if this.min.Size() != this.max.Size() {
        return float64(this.min.TopHeap())
    } else {
        return (float64(this.max.TopHeap())+float64(this.min.TopHeap()))/2
    }
}
// MaxHeap 最大堆结构体
type MaxHeap struct {
	data []int
	size int
}

// NewMaxHeap 建立一个最大堆
func NewMaxHeap() *MaxHeap {
	return &MaxHeap{
		data: make([]int, 16)[:0],
		size: 0,
	}
}

// PushHeap 向最大堆压入一个数字
func (mh *MaxHeap) PushHeap(val int) {
	if mh.size == 0 {
		mh.data = append(mh.data, val)
		mh.size++
		return
	}
	mh.data = append(mh.data, val)
	mh.size++
	mh.shiftUp()
}

// PopHeap 用来弹出堆顶元素
func (mh *MaxHeap) PopHeap() int {
	if mh.size == 0 {
		return -1
	}
	res := mh.data[0]
	mh.size--
	mh.data[0] = mh.data[mh.size]
	mh.data = mh.data[:mh.size]
	if mh.size <= 1 {
		return res
	}
	mh.shiftDown()
	return res
}

// 把最后一个数字依次向父节点挪动
// 维持最大堆的特性
func (mh *MaxHeap) shiftUp() {
	idx := len(mh.data) - 1
	for idx >= 0 {
		fidx := (idx - 1) / 2
		if mh.data[fidx] < mh.data[idx] {
			mh.data[fidx], mh.data[idx] = mh.data[idx], mh.data[fidx]
			idx = fidx
		} else {
			break
		}
	}
}

// 把第一个数字向下移动
// 维持最大堆
func (mh *MaxHeap) shiftDown() {
	idx := 0
	for idx < len(mh.data) {
		lidx := idx*2 + 1
		ridx := idx*2 + 2
		largeidx := idx
		if lidx < mh.size && mh.data[lidx] > mh.data[largeidx] {
			largeidx = lidx
		}
		if ridx < mh.size && mh.data[ridx] > mh.data[largeidx] {
			largeidx = ridx
		}
		if largeidx == idx {
			break
		}
		mh.data[largeidx], mh.data[idx] = mh.data[idx], mh.data[largeidx]
		idx = largeidx
	}
}

// TopHeap 返回堆顶元素但不弹出
func (mh *MaxHeap) TopHeap() int {
	if mh.size == 0 {
		return -1
	}
	return mh.data[0]
}

// Size 返回堆的大小
func (mh *MaxHeap) Size() int {
	return mh.size
}

// MinHeap 最小堆结构体
type MinHeap struct {
	data []int
	size int
}

// NewMinHeap 建立一个最小堆
func NewMinHeap() *MinHeap {
	return &MinHeap{
		data: make([]int, 16)[:0],
		size: 0,
	}
}

// PushHeap 向最小堆压入一个数字
func (mh *MinHeap) PushHeap(val int) {
	if mh.size == 0 {
		mh.data = append(mh.data, val)
		mh.size++
		return
	}
	mh.data = append(mh.data, val)
	mh.size++
	mh.shiftUp()
}

// PopHeap 用来弹出堆顶元素
func (mh *MinHeap) PopHeap() int {
	if mh.size == 0 {
		return -1
	}
	res := mh.data[0]
	mh.size--
	mh.data[0] = mh.data[mh.size]
	mh.data = mh.data[:mh.size]
	if mh.size <= 1 {
		return res
	}
	mh.shiftDown()
	return res
}

// 把最后一个数字依次向父节点挪动
// 维持最小堆的特性
func (mh *MinHeap) shiftUp() {
	idx := len(mh.data) - 1
	for idx >= 0 {
		fidx := (idx - 1) / 2
		if mh.data[fidx] > mh.data[idx] {
			mh.data[fidx], mh.data[idx] = mh.data[idx], mh.data[fidx]
			idx = fidx
		} else {
			break
		}
	}
}

// 把第一个数字向下移动
// 维持最小堆
func (mh *MinHeap) shiftDown() {
	idx := 0
	for idx < len(mh.data) {
		lidx := idx*2 + 1
		ridx := idx*2 + 2
		smallidx := idx
		if lidx < mh.size && mh.data[lidx] < mh.data[smallidx] {
			smallidx = lidx
		}
		if ridx < mh.size && mh.data[ridx] < mh.data[smallidx] {
			smallidx = ridx
		}
		if smallidx == idx {
			break
		}
		mh.data[smallidx], mh.data[idx] = mh.data[idx], mh.data[smallidx]
		idx = smallidx
	}
}

// TopHeap 返回堆顶元素但不弹出
func (mh *MinHeap) TopHeap() int {
	if mh.size == 0 {
		return -1
	}
	return mh.data[0]
}

// Size 返回堆的大小
func (mh *MinHeap) Size() int {
	return mh.size
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AddNum(num);
 * param_2 := obj.FindMedian();
 */
```

#  面试题42：连续子数组的最大和

[leetcode](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

描述：输入一个整型数组，数组里有正数也有负数。数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

思路：使用动态规划，实现一个dp数组和globalmax变量，其中dp[i]=max(nums[i], dp[i-1]+nums[i])，每次循环时如果dp[i]大于globalmax，则更新globalmax字段。

进一步优化可以把dp数组变为pre1和pre2变量，因为dp[i]只和dp[i-1]有关，即在一次循环中只需要其中的两个变量即可。

代码：

```go
func maxSubArray(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    pre1 := nums[0]
    pre2 := 0
    globalmax := nums[0]
    for i := 1; i < len(nums); i ++ {
        pre2 = max(0, pre1) + nums[i]
        globalmax = max(globalmax, pre2)
        pre1 = pre2
    }
    return globalmax
}
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

#  面试题43：1~n整数中1出现的次数

[leetcode](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/)

描述：输入一个整数 n ，求1～n这n个整数的十进制表示中1出现的次数。

例如，输入12，1～12这些整数中包含1 的数字有1、10、11和12，1一共出现了5次。

代码：

```go
func countDigitOne(n int) int {
    count, pow := 0, 1
    for n >= pow {
	count += n / (pow * 10) * pow
	if incr := n%(pow*10) - pow + 1; incr > pow {
	    count += pow
	} else if incr > 0 {
	    count += incr
	}
	pow *= 10
    }
    return count
}

作者：cmatrix
链接：https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/solution/zhao-gui-lu-tong-su-yi-dong-by-cmatrix/

```

#  面试题44：数字序列中某一位的数字

[leetcode](https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/)

描述：数字以0123456789101112131415…的格式序列化到一个字符序列中。在这个序列中，第5位（从下标0开始计数）是5，第13位是1，第19位是4，等等。

请写一个函数，求任意第n位对应的数字。

思路：一位数有10个，但是从1开始的话有九个，两位数的话有90个，三位数有900个，根据个数乘位数依次循环检查n所在的区间，然后取余操作。

代码：

```go
func findNthDigit(n int) int {
    if n <= 9 {
        return n
    }
    i := 1
    for {
        x := 9 * int(math.Pow(10, float64(i-1))) * i
        if n < x {
            break
        }
        n -= x
        i ++
    }
    n--
	number := int(math.Pow10(i-1)) + n/i
	index := n % i
	s := strconv.Itoa(number)
	return int(s[index] - '0')
}


```

#  面试题45：把数组排成最小的数

[leetcode](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

描述：输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

思路：自定义排序规则，实现sort的接口

代码：

```go
func minNumber(nums []int) string {
    if len(nums) == 0 {
        return ""
    }
    as := make([]string, len(nums))[:0]
    for _, n := range nums {
        as = append(as, strconv.Itoa(n))
    }
    sort.Sort(Arrs(as))
    res := ""
    for _, a := range as {
        res += a
    }
    return res
}
type Arrs []string

func (as Arrs) Len() int {
    return len(as)
}
func (as Arrs) Swap(i, j int) {
    as[i], as[j] = as[j], as[i]
}
func (as Arrs) Less(i, j int) bool {
    a := as[i]+as[j]
    b := as[j]+as[i]
    return a < b
}

```

#  面试题46：把数字翻译成字符串

[leetcode](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

描述：给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

思路：使用动态规划，从个位一次向上查询，dp[i]为第i位可以构成的序列的个数，当第i位和第i-1位构成的数字在26以内而且第i位大于0时，dp[i]=dp[i-1]+dp[i-2]，否则的话dp[i] = dp[i-1]，可以知道dp只需要三个变量就可以了，所以设为pre1、pre2和curtimes，而且在循环中需要记录上一位的数字，所以使用preNum记录。

代码：

```go
func translateNum(num int) int {
    if num / 10 == 0 {
        return 1
    }
    preNum := num%10
    pre1 := 1
    pre2 := 1

    num /= 10
    for num != 0 {
        curNum := num%10
        curTimes := 0
        if curNum == 1 || (curNum == 2 && preNum < 6){
            curTimes = pre1+pre2
        } else {
            curTimes = pre2
        }
        pre1 = pre2
        pre2= curTimes
        fmt.Println(pre2)
        num /= 10
        preNum = curNum
    }
    return pre2
}
```

#  面试题47：礼物的最大价值

[leetcode](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)

描述：在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

思路：使用一个相同的二维数组，从右下角开始往左和向上走，每个位置i,j的值为grid\[i\]\[j\] +max(dp\[i+1\]\[j\],dp\[i\]\[j+1\])，在边缘的话分别讨论。

例如输入[[1,3,1],[1,5,1],[4,2,1]]，dp=[[12,11,3],[9,8,2],[7,3,1]]

进阶的话只需要一个一维数组就可以实现。

代码：

```go
func maxValue(grid [][]int) int {
    if len(grid) == 0 || len(grid[0]) == 0 {
        return 0
    }
    m := len(grid)
    n := len(grid[0])

    dp := make([]int, n)
    for i := m-1; i >= 0; i -- {
        dp[n-1] = dp[n-1] + grid[i][n-1]
        for j := n-2; j >= 0; j -- {
            dp[j] = max(dp[j+1], dp[j])+grid[i][j]
        }
    }
    return dp[0]
}
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

#  面试题48：最长不含重复字符的子字符串

[leetcode](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

描述：请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

思路：使用滑动窗口

代码：

```go
func lengthOfLongestSubstring(s string) int {
    if len(s) <= 1 {
        return len(s)
    }
    left, right := 0,1
    res := 1
    for right < len(s) {
        idx := right-1
        for idx >= left {
            if s[idx] == s[right] {
                break
            }
            idx --
        }
        if idx < left {
            left = left
        } else {
            left = idx+1
        }
        res = max(res, right-left+1)
        right++
    }
    return res
}
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

#  面试题49：丑数

[leetcode](https://leetcode-cn.com/problems/chou-shu-lcof/)

描述：我们把只包含因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

思路：把一批数字乘2、3、5，然后再，选最小的数，再乘2、3、5

代码：

```go
func nthUglyNumber(n int) int {
    res := []int{}
    res = append(res,1)
    i2,i3,i5:= 0,0,0
    index := 1
    for index<n{
        res = append(res,int(math.Min(float64(2*res[i2]),math.Min(float64(3*res[i3]),float64(5*res[i5])))))
        if res[index]==2*res[i2]{
            i2++
        }
         if res[index]==3*res[i3]{
            i3++
        }
         if res[index]==5*res[i5]{
            i5++
        }
        index++
    }
    return res[index-1]
}

```

#  面试题50：第一个只出现一次的字符

##  题目一：字符串中第一个只出现一次的字符

[leetcode](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

描述：在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格

思路：两次循环，第一次循环把字符添加到map中，第二次循环判断map中字符出现的次数，如果为1则返回

代码：

```go
func firstUniqChar(s string) byte {
    m := make(map[rune]int)
    for _, c := range s {
        m[c] ++
    }
    for _, c := range s {
        if m[c] == 1 {
            return byte(c)
        }
    }
    return ' '
}
```

#  面试题51：数组中的逆序对

[leetcode](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

描述：在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数

思路：使用归并排序，在merge的过程中计算逆序对。比如使用cnt字段计数，初始数组为[7,5,6,4]，那么归完之后为l=[5,7]、r=[4,6]，每个子数组从头开始递归，i=0,j=0;如果l[i]>r[j]的话，那么l剩下的数字都可以和r[j]构成逆序对。

代码：

```go
func reversePairs(nums []int) int {
	// TODO: 分治的思想
	cnt := 0
	var merge func([]int, []int) []int
	var mergeSort func([]int) []int

	mergeSort = func(nums []int) []int {
		if len(nums) < 2 {
			return nums
		}
		n := len(nums)
		mid := n >> 1
		left := mergeSort(nums[:mid])
		right := mergeSort(nums[mid:])
		return merge(left, right)
	}

	merge = func(ar1, ar2 []int) []int {
		var res = make([]int, 0)
		sz1, sz2 := len(ar1), len(ar2)
		i, j  := 0, 0
		for i < sz1 && j < sz2 {
			if ar1[i] <= ar2[j] {
				res = append(res, ar1[i])
				i++
			} else {
				res = append(res, ar2[j])
				cnt = cnt + sz1 - i
				j++
			}
		}
		for i < sz1 {
			res = append(res, ar1[i])
			i++
		}
		for j < sz2 {
			res = append(res, ar2[j])
			j++
		}
        return res
	}

	mergeSort(nums)
	return cnt
}

```

#   面试题52：两个链表的第一个公共节点

[leetcode](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

描述：输入两个链表，找出他们的第一个公共节点。

思路：因为链表是单向的，所以在公共节点之后的结点都是相同的。可以把两个链表的长度都求出来，然后长的链表首先走几步，之后两个链表一起走，直到遍历到的结点相同，则为第一个公共节点。

代码：

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    la := 0
    lb := 0
    t := headA
    for t != nil {
        la ++
        t = t.Next
    }
    t = headB
    for t != nil {
        lb ++
        t = t.Next
    }
    if la < lb {
        headA, headB = headB, headA
        la, lb = lb, la
    }
    for i := 0; i < la-lb; i ++ {
        headA = headA.Next
    }
    for headA != nil && headB != nil {
        if headB==headA {
            return headA
        }
        headA = headA.Next
        headB = headB.Next
    }
    return nil
}
```

#  面试题53：在排序数组中查找数字

##  题目一：数字在排序数组中出现的次数

[leetcode](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

描述：统计一个数字在排序数组中出现的次数。

思路：使用二分查找该数字，然后向左向右扩散求出大小。

代码：

```go
func search(nums []int, target int) int {
    var l, h = 0, len(nums)-1
    var count int
    for l <= h {
        mid := l + (h-l)/2
        if nums[mid] == target {
            i := mid
            for i < len(nums) && nums[i] == target {
                i ++
                count ++
            }
            i = mid-1
            for i >= 0 && nums[i] == target {
                count ++
                i --
            }
            return count
        } else if nums[mid] < target {
            l = mid + 1
        } else {
            h = mid - 1
        }
    }
    return count
}

```

##  题目二：0~n-1中缺失的数字

[leetcode](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

描述：一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

思路：因为数组使递增的，那么正常情况下，idx对应的数字和idx是相等的，如果不想等的话，就把nums[idx]和nums[nums[idx]]进行交换，如果nums[idx]超过了len(nums)的话，交换会导致越界，那么直接返回该idx

代码：

```go
func missingNumber(nums []int) int {
    if len(nums) == 0 {
        return -1
    }
    idx := 0
    for idx < len(nums) {
        if nums[idx] == idx {
            idx ++
            continue
        } else if nums[idx] >= len(nums) {
            return idx
        } else {
            nums[nums[idx]], nums[idx] = nums[idx], nums[nums[idx]]
        }
    }
    return len(nums)
}
```

#  面试题54：二叉搜索树的第k大个节点

[leetcode](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

描述：给定一个二叉搜索树，请找出第k大的结点

思路：使用循环遍历二叉树，先遍历右节点，在父结点，然后左节点，这样最开始的是最大的，遍历到一个结点时k--，直到k==1时返回当前节点的val

代码：

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
// 中序遍历,不过先遍历右子树
func kthLargest(root *TreeNode, k int) int {
    head := root
    s := NewStack()
    for head != nil || !s.Empty(){
        for head != nil {
            s.Push(head)
            head = head.Right
        }
        head = s.Pop().(*TreeNode)
        if k == 1 {
            return head.Val
        }
        head = head.Left
        k --
    }
    return -1
}
type stack struct {
	root *node
	size int
}
type node struct {
	val  interface{}
	next *node
}

func NewStack() *stack {
	return &stack{
		root: new(node),
		size: 0,
	}
}
func (s *stack) Push(value interface{}) {
	tmp := new(node)
	tmp.val = value
	tmp.next = s.root
	s.root = tmp
	s.size++
}
func (s *stack) Pop() interface{} {
	if s.root != nil {
		v := s.root.val
		s.root = s.root.next
		s.size--
		return v
	}
	return nil
}
func (s *stack) Top() interface{} {
	if s.root == nil {
		return nil
	}
	return s.root.val
}
func (s *stack) Size() int {
	return s.size
}
func (s *stack) Empty() bool {
	return s.size == 0
}

// 1.3.7 为stack添加一个peek() 方法
func (s *stack) Peek() interface{} {
	return s.Top()
}
```

#  面试题55：二叉树的深度

##  题目一：二叉树的深度

[leetcode](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

描述：输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

思路：递归，当到叶结点时返回1，否则返回最大的子节点深度加一。

代码：

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func maxDepth(root *TreeNode) int {
    if root == nil {
        return 0
    }
    if root.Left == nil && root.Right == nil {
        return 1
    }
    l := maxDepth(root.Left)
    r := maxDepth(root.Right)
    return max(l, r)+1
}
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

##  题目二：平衡二叉树

[leetcode](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)

描述：输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

思路：使用一个辅助函数，从叶结点向上走，返回值表示该结点是否为平衡的以及该结点的深度。

代码：

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func isBalanced(root *TreeNode) bool {
    ok, _ := helper(root)
    return ok
}
func helper(root *TreeNode) (bool, int) {
    if root == nil {
        return true, 0
    }
    if root.Left == nil && root.Right == nil {
        return true, 1
    }
    ok, l := helper(root.Left)
    if !ok {
        return ok,l
    }
    ok, r := helper(root.Right)
    if !ok {
        return ok,r
    }
    if abs(l-r) <= 1 {
        return true,max(l,r)+1
    }
    return false, max(l,r)
}
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
func abs(a int) int {
    if a < 0 {
        return -a
    }
    return a
}
```

# 面试题56：数组中数字出现的次数

##  题目一：数组中只出现一次的两个数字

