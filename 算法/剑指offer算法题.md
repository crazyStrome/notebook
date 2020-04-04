<!-- TOC -->

- [面试题3：数组中重复的数字](#面试题3数组中重复的数字)
    - [题目一：找出数组中重复的数字](#题目一找出数组中重复的数字)
    - [题目二：不修改数组找出重复的数字](#题目二不修改数组找出重复的数字)
- [面试题4：二维数组中的查找](#面试题4二维数组中的查找)
- [面试题5：替换字符串中的空格](#面试题5替换字符串中的空格)
- [面试题6：从头到尾打印链表](#面试题6从头到尾打印链表)
- [面试题7：重建二叉树](#面试题7重建二叉树)
- [面试题8：二叉树的下一个节点](#面试题8二叉树的下一个节点)
- [面试题9：用栈实现队列](#面试题9用栈实现队列)
- [面试题10：斐波那契数列](#面试题10斐波那契数列)
    - [题目一：求斐波那契数列的第n项](#题目一求斐波那契数列的第n项)
    - [题目二：青蛙跳台阶问题](#题目二青蛙跳台阶问题)
    - [面试题11：旋转数组的最小数字](#面试题11旋转数组的最小数字)
    - [面试题12：矩阵中的路径](#面试题12矩阵中的路径)
    - [面试题13：机器人的运动范围](#面试题13机器人的运动范围)
    - [面试题14：剪绳子](#面试题14剪绳子)
- [面试题15：二进制中1的个数](#面试题15二进制中1的个数)
- [面试题16：数值的整数次方](#面试题16数值的整数次方)
- [面试题17：打印从1到最大的n位数](#面试题17打印从1到最大的n位数)
- [面试题19：正则表达式匹配](#面试题19正则表达式匹配)
- [面试题20：表示数值的字符串](#面试题20表示数值的字符串)
- [面试题21：调整数组顺序使奇数位于偶数前面](#面试题21调整数组顺序使奇数位于偶数前面)
- [面试题22：链表中倒数第k个节点](#面试题22链表中倒数第k个节点)
- [面试题23：链表中环的入口节点](#面试题23链表中环的入口节点)
- [面试题24：反转链表](#面试题24反转链表)
- [面试题25：合并两个排序的链表](#面试题25合并两个排序的链表)
- [面试题26：树的子结构](#面试题26树的子结构)
- [面试题27：二叉树的镜像](#面试题27二叉树的镜像)
- [面试题28：对称的二叉树](#面试题28对称的二叉树)
- [面试题29：顺时针打印矩阵](#面试题29顺时针打印矩阵)
- [面试题30：包含min函数的栈](#面试题30包含min函数的栈)
- [面试题31：栈的压入、弹出序列](#面试题31栈的压入弹出序列)
- [面试题32：从上到下打印二叉树](#面试题32从上到下打印二叉树)
    - [题目一：不分行从上到下打印二叉树](#题目一不分行从上到下打印二叉树)
    - [题目二：分行从上到下打印二叉树](#题目二分行从上到下打印二叉树)
    - [题目三：从上到下打印二叉树](#题目三从上到下打印二叉树)
- [面试题33：二叉树的后序遍历序列](#面试题33二叉树的后序遍历序列)
- [面试题34：二叉树中和为某一值的路径](#面试题34二叉树中和为某一值的路径)
- [面试题35：复杂链表的复制](#面试题35复杂链表的复制)
- [面试题36：二叉搜索树与双向链表](#面试题36二叉搜索树与双向链表)
- [面试题37：序列化二叉树](#面试题37序列化二叉树)
- [面试题38：字符串的排列](#面试题38字符串的排列)
- [面试题39：数组中出现次数超过一半的数字](#面试题39数组中出现次数超过一半的数字)

<!-- /TOC -->
#  面试题3：数组中重复的数字

##  题目一：找出数组中重复的数字

[leetcode](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

描述：在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字

示例：

输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 

思路：通过遍历把数字放到对应下标的位置，如果该下标已经有该数字，则重复

代码：

```go
func findRepeatNumber(nums []int) int {
    if len(nums) == 0 {
        return -1
    }
    idx := 0
    for idx < len(nums) {
        if nums[idx] == idx {
            idx ++
        } else {
            if nums[nums[idx]] == nums[idx] {
                return nums[idx]
            } else {
                nums[nums[idx]], nums[idx] = nums[idx], nums[nums[idx]]
            }
        }
    }
    return -1
}
```

##  题目二：不修改数组找出重复的数字

描述：在一个长度为n+1的数组里的所有数字都在1~n范围内，所以数组中至少有一个数字是重复的。找出任意一个重复的数字，但不能修改输入的数组。

示例：

输入：[2,3,5,4,3,2,6,7]

输出：2或3

思路：从1-n取中心点m，判断在1~m之间的数总数是否为m，如果大于m，就说明这个区间有重复数字。

代码：

```go
func getDuplicateNum(nums []int) int {
	if len(nums) == 0 {
		return -1
	}
	n := len(nums) - 1
	start := 1
	end := n

	for start+1 < end {
		mid := start + (end-start)/2
		c := count(nums, start, mid)
		if c > (mid - start + 1) {
			end = mid
		} else {
			start = mid + 1
		}
	}
	if count(nums, start, start) > 1 {
		return start
	}
	if count(nums, end, end) > 1 {
		return end
	}
	return -1
}
func count(nums []int, start, end int) int {
	res := 0
	for i := 0; i < len(nums); i++ {
		if nums[i] >= start && nums[i] <= end {
			res++
		}
	}
	return res
}

```

#  面试题4：二维数组中的查找

[leetcode](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

描述：在一个二维数组中，每一行从左到右递增，每一列从上到下递增，输入一个数组和整数，判断数组中是否有该整数。

示例：

现有矩阵 matrix 如下：

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
给定 target = 5，返回 true。

给定 target = 20，返回 false。

思路：两种思路：

1. 从右上角开始，如果大于目标值，向左走，如果小于目标值，向下走
2. 从左下角开始，如果小于目标值，向右走，否则向上走

代码：

```go
func findNumberIn2DArray(matrix [][]int, target int) bool {
    //从右上角开始
    if len(matrix) == 0 || len(matrix[0]) == 0 {
        return false
    }
    m := len(matrix)
    n := len(matrix[0])
    x := 0
    y := n-1

    for x < m && y >= 0 {
        if matrix[x][y] == target {
            return true
        } else if matrix[x][y] > target {
            y --
        } else {
            x ++
        }
    }
    return false
}
```

#  面试题5：替换字符串中的空格

[leetcode](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

描述：把字符串中的每个空格替换成“%20”

思路：使用空间换时间或者时间换空间

代码：

```go
func replaceSpace(s string) string {
    count := 0
    for _, c := range s {
        if c == ' ' {
            count ++
        }
    }
    res := make([]byte, len(s)+count*3)
    idx := 0
    for _, c := range s {
        if c == ' ' {
            copy(res[idx:idx+3], []byte{'%', '2', '0'})
            idx += 3
            
        } else {
            res[idx] = byte(c)
            idx ++
        }
    }
    return string(res[:idx])
}
```

#  面试题6：从头到尾打印链表

[leetcode](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof)

描述：输入一个链表的头结点，从尾到头反过来打印每个结点的值。

思路：栈，或者递归

代码：

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reversePrint(head *ListNode) []int {
    if head == nil {
        return []int{}
    }
    if head.Next == nil {
        return []int{head.Val}
    }
    return append(reversePrint(head.Next), head.Val)
}
```

#  面试题7：重建二叉树

[leetcode](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof)

描述：给出前序遍历和中序遍历，重建二叉树

思路：前序遍历的第一个数是根节点，然后在中序遍历中找到对应的左右子节点，递归遍历

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
func buildTree(preorder []int, inorder []int) *TreeNode {
    if len(preorder) == 0 {
        return nil
    }
    if len(preorder) == 1 {
        return &TreeNode{Val:preorder[0]}
    }
    rootNum := preorder[0]
    leftInorderArray, rightInorderArray := getChild(inorder, rootNum)
    root := &TreeNode{Val:rootNum}
    if len(leftInorderArray) == 0 {
        root.Right = buildTree(preorder[1:], rightInorderArray)
    } else if len(rightInorderArray) == 0 {
        root.Left = buildTree(preorder[1:], leftInorderArray)
    } else {
        // leftNodeLastIdx := findIndex(preorder, leftInorderArray[len(leftInorderArray)-1])
        root.Left = buildTree(preorder[1:len(leftInorderArray)+1], leftInorderArray)
        root.Right = buildTree(preorder[len(leftInorderArray)+1:], rightInorderArray)
    }
    return root
}
func getChild(inorder []int, root int) ([]int, []int) {
    if len(inorder) == 0 {
        return []int{}, []int{}
    }
    idx := 0
    for idx < len(inorder) {
        if inorder[idx] == root {
            return inorder[:idx], inorder[idx+1:]
        }
        idx ++
    }
    return []int{}, []int{}
}
```

#  面试题8：二叉树的下一个节点

描述：给定一个二叉树和其中一个节点，如何找出中序遍历的下一个节点？树节点除了有指向左右子节点的指针，还有指向父节点的指针。

思路：右子树的最左节点，如果没有子节点分两部分：

* 是父节点的左节点，那么下一个值就是父节点
* 是父节点的右节点，那么一直向上遍历，找到一个节点，且该结点是其父节点的左子节点，那么下一个节点就是他的父节点。

代码：

```go
type TreeNode struct {
	Val   int
	Pre   *TreeNode
	Left  *TreeNode
	Right *TreeNode
}

func getNextInorder(head *TreeNode) *TreeNode {
	if head == nil {
		return nil
	}
	if head.Right != nil {
		tmp := head.Right
		for tmp != nil && tmp.Left != nil {
			tmp = tmp.Left
		}
		return tmp
	} else {
		if head == head.Pre.Left {
			return head.Pre
		} else {
			tmp := head.Pre
			for tmp != tmp.Pre.Left {
				tmp = tmp.Pre
			}
			return tmp.Pre
		}
	}
}
```

#  面试题9：用栈实现队列

[leetcode](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof)

描述：用栈实现队列，实现函数`appendTail`和`deleteHead`，分别完成在队列尾部插入结点和在头部删除结点

思路：使用stack和辅助栈helper

* 插入操作：如果stack为空，直接插入；否则把stack弹出，压到helper中，将新值压入helper，然后把helper的弹出依次压入stack中，这样保证stack栈顶就是队列头
* 删除操作：直接删除stack的栈顶元素

代码：

```go
type CQueue struct {
    s *stack
    h *stack
}


func Constructor() CQueue {
    return CQueue{
        s:NewStack(),
        h:NewStack(),
    }
}


func (this *CQueue) AppendTail(value int)  {
    if this.s.Empty() {
        this.s.Push(value)
        return
    }
    for !this.s.Empty() {
        this.h.Push(this.s.Pop())
    }
    this.h.Push(value)
    for !this.h.Empty() {
        this.s.Push(this.h.Pop())
    }
}


func (this *CQueue) DeleteHead() int {
    if this.s.Empty() {
        return -1
    }
    return this.s.Pop().(int)
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
/**
 * Your CQueue object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AppendTail(value);
 * param_2 := obj.DeleteHead();
 */
```

#  面试题10：斐波那契数列

##  题目一：求斐波那契数列的第n项

[leetcode](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

描述：如题

思路：递归或者循环

代码：

```go
func fib(n int) int {
    if n == 0 {
        return 0
    }
    n1 := 1
    n2 := 1
    for i := 3; i <= n; i ++ {
        sum := (n1+n2)%1000000007
        n1 = n2
        n2 = sum
    }
    // fmt.Println(1134903170%1000000007)
    return n2
}
```

##  题目二：青蛙跳台阶问题

[leetcode](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof)

描述：一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个n级台阶总共有多少种方法。

思路：和斐波那契数列相似，只不过初始条件不同

代码：

```go
func numWays(n int) int {
    if n == 0 {
        return 1
    }
    if n < 3 {
        return n
    }
    n1 := 1
    n2 := 2
    for i := 3; i <= n; i ++ {
        sum := (n1+n2)%1000000007
        n1 = n2
        n2 = sum
    }
    return n2
}
```


##  面试题11：旋转数组的最小数字
描述：把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。

思路：减治的二分算法，使用mid位置的数字和left或者right比较。首先考虑和left比较，[3, 4, 5, 1, 2] 与 [1, 2, 3, 4, 5] ，此时，中间位置的值都比左边大，但最小值一个在后面，一个在前面，因此这种做法不能有效地减治。考虑和right比较，[1, 2, 3, 4, 5]、[3, 4, 5, 1, 2]、[2, 3, 4, 5 ,1]，用右边位置和中间位置的元素比较，可以进一步缩小搜索的范围。

代码：
```go
func minArray(numbers []int) int {
    if len(numbers) == 0 {
        return -1
    }
    start := 0
    end := len(numbers)-1

    for start + 1 < end {
        mid := start + (end-start)/2
        if numbers[mid] == numbers[end] {
            end --
        } else if numbers[mid] > numbers[end] {
            start = mid+1
        } else {
            end = mid
        }
    }
    if numbers[start] < numbers[end] {
        return numbers[start]
    }
    return numbers[end]
}
```

##  面试题12：矩阵中的路径
描述：请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。

[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。
思路：使用另外一个相同大小的二维数组record记录该路径是否走过，如果该路径失败，则将路径中的记录清除。
使用遍历判断字符是否和word的首字母相同，如果相同的话，把该位置的record设置为true，不过在判断之前需要进行边界的判定以及是否该路径走过的判定。之后判定该字符周围的字符是否在剩余word[1:]组成的路径中，如果是的话，就说明该路径成立，否则不成立，取消该字符在record中的标记

代码：
```go
func exist(board [][]byte, word string) bool {
    if len(board) == 0 || len(board[0]) == 0 {
        return word == ""
    }
    if word == "" {
        return true
    }
    m := len(board)
    n := len(board[0])
    record := make([][]bool, m)
    // record中默认false，表示该字符都没有路过
    for i := 0; i < m; i ++ {
        record[i] = make([]bool, n)
    }

    for i := 0; i < m; i ++ {
        for j := 0; j < n; j ++ {
            if helper(board, record, word, i, j, m, n) {
                return true
            }
        }
    }
    return false
}
func helper(board [][]byte, record [][]bool, word string, x, y int, m, n int) bool {
    //  如果word为""，则说明前面的字符已经匹配完了
    if len(word) == 0 {
        return true
    }
    //  检查越界
    if x < 0 || y < 0 || x >= m || y >= n {
        return false
    }
    //  如果该路径走过了，就返回不可达
    if record[x][y] {
        return false
    }
    if board[x][y] == word[0] {
        fmt.Println(word)
        fmt.Println(board[x][y])
        record[x][y] = true
        flag := false
        // 检查上下左右的字符是否匹配剩余的路径
        flag = flag || helper(board, record, word[1:], x+1, y, m, n)
        flag = flag || helper(board, record, word[1:], x, y+1, m, n)
        flag = flag || helper(board, record, word[1:], x-1, y, m, n)
        flag = flag || helper(board, record, word[1:], x, y-1, m, n)
        if flag {
            return true
        } else {
            record[x][y] = false
            return false
        }
    }
    return false
}
```

##  面试题13：机器人的运动范围
描述：地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

思路：使用record记录走过的地点，依次向下遍历

代码：
```go
func movingCount(m int, n int, k int) int {
    record := make([]bool, m*n)
    return helper(0, 0, m, n, k, record)
}
func helper(x, y, m, n, k int, record []bool) int {
    if x < 0 || x >= m || y < 0 || y >= n {
        return 0
    }
    if calculate(x, y) > k {
        return 0
    }
    if record[x*n+y] {
        return 0
    }
    res := 1
    record[x*n+y] = true
    res += helper(x, y+1, m, n, k, record)
    res += helper(x, y-1, m, n, k, record)
    res += helper(x-1, y, m, n, k, record)
    res += helper(x+1, y, m, n, k, record)
    return res
}
func calculate(a, b int) int {
    res := 0
    for a != 0 || b != 0{
        res += a%10
        a /= 10
        res += b%10
        b /= 10
    }
    return res
}
```

##  面试题14：剪绳子
[leetcode](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

描述：给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m] 。请问 k[0]*k[1]*...*k[m] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

思路一：动态规划，当前的最大积是之前的积的最大值。

代码：
```go
func cuttingRope(n int) int {
    if n <= 1 {
        return 0
    }
    if n == 2 {
        return 1
    }
    if n == 3 {
        return 2
    }
    arr := make([]int, n+1)
    arr[0] = 0
    arr[1] = 1
    arr[2] = 2
    arr[3] = 3
    arr[4] = 4
    for i := 5; i <= n; i ++ {
        max := 0
        for j := i-1; j >= i/2; j -- {
            tmp := arr[j]*arr[i-j]
            if max < tmp {
                max = tmp
            }
        }
        arr[i] = max
    }
    return arr[n]
}
```

思路二：贪心算法：当n>=5时，尽可能多剪长度为3的绳子，如果长度为4，则剪成两根长度为2的绳子。

代码：
```go
func cuttingRope(n int) int {
    if n <= 1 {
        return 0
    }
    if n == 2 {
        return 1
    }
    if n == 3 {
        return 2
    }
    timeOf3 := n/3
    if n%3 == 1 {
        timeOf3 --
    }
    timeOf2 := (n-timeOf3*3)/2
    return int(math.Pow(3, float64(timeOf3)))*int(math.Pow(2, float64(timeOf2)))
}
```

#  面试题15：二进制中1的个数

[leetcode](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

描述：请实现一个函数，输入一个整数，输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。

思路：使用1进行与操作，一次向左移

代码：

```go
func hammingWeight(num uint32) int {
    var a uint32 = 1
    count := 0
    for i := 0; i < 32; i ++ {
        if num & a != 0 {
            count ++
        }
        a <<= 1
    }
    return count
}
```

思路：使用n和n-1进行与操作，例如一开始n=5，二进制是0101,那么和4与之后为4，二进制位0100，之后4和3再与就是0了，相当于做了两次与操作，然后就得到1的个数

代码：

```go
func hammingWeight(n uint32) int {
    var count int
    for n != 0 {
        count ++
        n = n &(n-1)
    }
    return count
}
```

#  面试题16：数值的整数次方

描述：实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。

思路：使用二分法，考虑次方为负数的情况

代码：

```go
func myPow(x float64, n int) float64 {
    if n == 0 {
        return 1.0
    }
    if n < 0 {
        return 1.0/pow(x, -n)
    }
    return pow(x, n)
}
func pow(x float64, n int) float64 {
    if n == 1 {
        return x
    }
    tmp := pow(x, n/2)
    if n%2 == 1 {
        return x*tmp*tmp
    }
    return tmp*tmp
}
```

#  面试题17：打印从1到最大的n位数

[leetcode](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof)

描述：输入数字 `n`，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

代码：

```go
func printNumbers(n int) []int {
    num := make([]int, int(math.Pow(10,float64(n))-1))
    for i := 0; i < len(num); i ++ {
        num[i] = i+1
    }
    return num
}
```

#  面试题19：正则表达式匹配

[leetcode](https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/submissions/)

描述：请实现一个函数用来匹配包含'. '和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但与"aa.a"和"ab*a"均不匹配。

思路：

![image-20200329161913939](https://i.loli.net/2020/03/29/41bfzeCwuySrJnZ.png)

代码：

```go
func isMatch(s string, p string) bool {
    return helper(s, p, 0, 0)
}
func helper(s, p string, x, y int) bool {
    if x == len(s) && y == len(p) {
        return true
    }
    if x < len(s) && y == len(p) {
        return false
    }
    if y < len(p)-1 && p[y+1] == '*' {
        if x < len(s) && s[x] == p[y] || (p[y] == '.' && x < len(s)) {
            return helper(s, p, x+1, y+2) || helper(s, p, x, y+2) || helper(s, p, x+1, y)
        } else {
            return helper(s, p, x, y+2)
        }
    }
    if x < len(s) && s[x] == p[y] || (p[y] == '.' && x < len(s)) {
        return helper(s, p, x+1, y+1)
    } 
    return false
}

```

#  面试题20：表示数值的字符串

[leetcode](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

描述：请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100"、"5e2"、"-123"、"3.1416"、"0123"及"-1E-16"都表示数值，但"12e"、"1a3.14"、"1.2.3"、"+-5"及"12e+5.4"都不是。

 

#  面试题21：调整数组顺序使奇数位于偶数前面

[leetcode](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

描述：输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

思路：使用双指针，奇数指针从0开始，偶数指针从数组末尾开始，当奇数指针指向的数为偶数时，同偶数指针指向的数交换，偶数指针减一，进入下一个循环。如果奇数指针指向的数为奇数时，奇数指针加一。

代码：

```go
func exchange(nums []int) []int {
    if len(nums) == 0 || len(nums) == 1 {
        return nums
    }
    left := 0
    right := len(nums)-1

    for left + 1< right {
        if nums[left] % 2 != 0 {
            left ++
        } else {
            nums[left], nums[right] = nums[right], nums[left]
            right --
        }
    }
    if nums[left] % 2 == 0 {
        nums[left], nums[right] = nums[right], nums[left]
    }
    return nums
}
```

#  面试题22：链表中倒数第k个节点

[leetcode](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

描述：输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。

思路：两个指针，快指针先走k-1步，这样当快指针走到尾，慢指针就走到第k个了，在一开始要注意链表长度小于k的情况。

代码：

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func getKthFromEnd(head *ListNode, k int) *ListNode {
    var f, s = head, head
    for i := 0; i < k; i ++ {
        if f == nil {
            return head
        }
        f = f.Next
    }
    for f != nil {
        s = s.Next
        f = f.Next
    }
    return s
}
```

#  面试题23：链表中环的入口节点

描述：给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

说明：不允许修改给定的链表。

思路：首先快慢指针走，一个走两步一个走一步，当出现相遇时表明有环，记录该节点。然后使用两个指针，一个指向头结点，一个指向相遇结点，同时向后走，如果相遇，则为环的入口点。

代码：

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func detectCycle(head *ListNode) *ListNode {
    if head == nil || head.Next == nil {
        return nil
    }
    intersect := getInter(head)
    if intersect == nil {
        return nil
    }
    l1 := head
    l2 := intersect
    for l1 != l2 {
        l1 = l1.Next
        l2 = l2.Next
    }
    return l1
}
func getInter(root *ListNode) *ListNode {
    if root == nil || root.Next == nil {
        return nil
    }
    slow := root
    fase := root
    for fase != nil && fase.Next != nil {
        fase = fase.Next.Next
        slow = slow.Next
        if slow == fase {
            return slow
        }
    }
    return nil
}
```

#  面试题24：反转链表

[leetcode](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

描述：定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

思路：循环或者递归

循环代码：

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseList(head *ListNode) *ListNode {
    var pre, next *ListNode
    for head != nil {
        next = head.Next
        head.Next = pre
        pre = head
        head = next
    }
    return pre
}
```

递归代码：

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseList(head *ListNode) *ListNode {
    if head == nil {
        return head
    }
    if head.Next == nil {
        return head
    }
    // 反转之后，n变为尾结点
    n := head.Next
    t := reverseList(n)
    n.Next = head
    head.Next = nil
    return t
}
```

#  面试题25：合并两个排序的链表

[leetcode](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

描述：输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

思路：使用递归实现，首先判断是否为nil，如果有一个为nil的话就返回另一个；如果两个指针都不为nil的话就进行比较，找出较小者作为头结点，其next结点为递归该函数使用前者的next结点和较大节点产生的新节点。

代码：

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    if l1 == nil {
        return l2
    }
    if l2 == nil {
        return l1
    }
    if l1.Val < l2.Val {
        l1.Next = mergeTwoLists(l1.Next, l2)
        return l1 
    } else {
        l2.Next = mergeTwoLists(l2.Next, l1)
        return l2
    }
    return nil
}
```

#  面试题26：树的子结构

[leetcode](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

描述：输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

思路：首先对A树的每一个节点进行遍历，如果该结点和B相等时，调用helper函数，否则的话，递归调用A的子节点和B的函数。

在helper函数中，如果B为nil则返回true，如果A为nil而B不为nil时，说明不存在字数，返回false；如果A和B不相同的话，返回false，相同的话，使用helper递归调用判断A、B的左右子树是否相同。

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
func isSubStructure(A *TreeNode, B *TreeNode) bool {
    return (A != nil && B != nil) && (isBofA(A, B) || isSubStructure(A.Left, B) || isSubStructure(A.Right, B))
}
func isBofA(A *TreeNode, B *TreeNode) bool {
    if B == nil {
        return true
    }
    if A == nil {
        return false
    }
    if A.Val != B.Val {
        return false
    }
    return isBofA(A.Left, B.Left) && isBofA(A.Right, B.Right)
}

```

#  面试题27：二叉树的镜像

[leetcode](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

描述：请完成一个函数，输入一个二叉树，该函数输出它的镜像。

思路：首先镜像子树，然后交换子树。使用递归完成

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
func mirrorTree(root *TreeNode) *TreeNode {
    if root == nil {
        return nil
    }
    if root.Left == nil && root.Right == nil {
        return root
    }
    root.Left, root.Right = mirrorTree(root.Right), mirrorTree(root.Left)
    return root
}
```

#  面试题28：对称的二叉树

[leetcode](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

描述：请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

思路：使用helper函数，输入两个子树，递归调用，如果节点相同的话，递归判断树1的左子树和树2的右子树，以及树1的右子树和树2的左子树。

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
func isSymmetric(root *TreeNode) bool {
    if root == nil {
        return true
    }
    return helper(root, root)
}
func helper(t1, t2 *TreeNode) bool {
    if t1 == nil && t2 == nil {
        return true
    }
    if t1 != nil && t2 == nil {
        return false
    }
    if t1 == nil && t2 != nil {
        return false
    }
    if t1.Val == t2.Val {
        return helper(t1.Left, t2.Right) && helper(t1.Right, t2.Left)
    }
    return false
}
```

#  面试题29：顺时针打印矩阵

[leetcode](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

描述：输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

思路：一层一层的打印。

代码：

```go
func spiralOrder(matrix [][]int) []int {
    if len(matrix) == 0 || len(matrix[0]) == 0 {
        return []int{}
    }
    m := len(matrix)
    n := len(matrix[0])

    layers := m 
    if layers < n {
        layers = n
    }
    if layers % 2 == 1 {
        layers = layers/2 + 1
    } else {
        layers = layers/2
    }
    res := make([]int, m*n)
    idx := 0
    for i := 0; i < layers; i ++ {
        for j := i; j < n-i; j ++ {
            if idx == m*n {
                return res
            }
            res[idx] = matrix[i][j]
            idx ++
        }
        for j := i+1; j < m-i; j ++ {
            if idx == m*n {
                return res
            }
            res[idx] = matrix[j][n-i-1]
            idx ++
        }
        for j := n-i-2; j >= i; j -- {
            if idx == m*n {
                return res
            }
            res[idx] = matrix[m-i-1][j]
            idx ++
        }
        for j := m-i-2; j >= i +1; j -- {
            if idx == m*n {
                return res
            }
            res[idx] = matrix[j][i]
            idx ++
        }
    }
    return res
}
```

#  面试题30：包含min函数的栈

[leetcode](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

描述：定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

思路：维持一个最小值min字段，每次压栈时如果x比min小，就把min和x都压入栈，然后更新min。出栈时，如果Pop的值和min相等的话，就把下一个pop的值附给min。

代码：

```go
type MinStack struct {
    s *stack
    min int
}


/** initialize your data structure here. */
func Constructor() MinStack {
    return MinStack{NewStack(), math.MaxInt32}
}


func (this *MinStack) Push(x int)  {
    if x <= this.min {
        this.s.Push(this.min)
        this.min = x
    }
    this.s.Push(x)
}


func (this *MinStack) Pop()  {
    if this.s.Pop().(int) == this.min {
        this.min = this.s.Pop().(int)
    }
}


func (this *MinStack) Top() int {
    return this.s.Top().(int)
}


func (this *MinStack) Min() int {
    return this.min
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
/**
 * Your MinStack object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(x);
 * obj.Pop();
 * param_3 := obj.Top();
 * param_4 := obj.Min();
 */
```

#  面试题31：栈的压入、弹出序列

[leetcode](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

描述：输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

思路：如果下一个弹出的数字是栈顶数字，那么直接弹出，否则的话就一次压入数字直到出现该弹出数字，如果压完了都没有相同的，则说明该序列不是弹出序列

代码：

```go
func validateStackSequences(pushed []int, popped []int) bool {
    if len(pushed) == 0 && len(popped) == 0 {
        return true
    }
    s := NewStack()
    idx := 0
    for _, n := range popped {
        for idx < len(pushed) && (s.Empty() || s.Top().(int) != n) {
            s.Push(pushed[idx])
            idx ++
        }
        if s.Top().(int) == n {
            s.Pop()
        } else {
            return false
        }
    }
    return true
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
```

#  面试题32：从上到下打印二叉树

##  题目一：不分行从上到下打印二叉树

[leetcode](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

描述：从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

思路：使用队列，每一层遍历时首先取出队列中的元素，加入结果数组中，然后把该结点的右子树和左子树分别压进去，进行下一个循环。

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
func levelOrder(root *TreeNode) []int {
    if root == nil {
        return []int{}
    }
    queue := []*TreeNode{root}
    res := []int{}
    for len(queue) != 0 {
        t := queue[0]
        res = append(res, t.Val)
        queue = queue[1:]
        if t.Left != nil {
            queue = append(queue, t.Left)
        }
        if t.Right != nil {
            queue = append(queue, t.Right)
        }
    }
    return res
}
```

##  题目二：分行从上到下打印二叉树

[leetcode](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

描述：从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

思路：使用两个队列，公有队列和临时队列，每一次循环中从公有队列取结点，添加到结果集后，把左右子节点添加到临时队列中，一个循环结束，让公有队列指向临时队列。

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
func levelOrder(root *TreeNode) [][]int {
    res := [][]int{}
    if root == nil {
        return res
    }
    queue := []*TreeNode{root}
    for len(queue) != 0 {
        t := []*TreeNode{}
        nums := []int{}
        for _, n := range queue {
            nums = append(nums, n.Val)
            if n.Left != nil {
                t = append(t, n.Left)
            }
            if n.Right != nil {
                t = append(t, n.Right)
            }
        }
        res = append(res, nums)
        queue = t
    }
    return res
}
```

##  题目三：从上到下打印二叉树

[leetcode](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

描述：请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

思路：和分层打印类似，只不过使用一个flag判断是否该层的结果需要reverse以下

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
func levelOrder(root *TreeNode) [][]int {
    res := [][]int{}
    if root == nil {
        return res
    }
    queue := []*TreeNode{root}
    flag := false
    for len(queue) != 0 {
        t := []*TreeNode{}
        nums := []int{}
        for _, n := range queue {
            nums = append(nums, n.Val)
            if n.Left != nil {
                t = append(t, n.Left)
            }
            if n.Right != nil {
                t = append(t, n.Right)
            }
        }
        if flag {
            i, j := 0, len(nums)-1
            for i < j {
                nums[i], nums[j] = nums[j], nums[i]
                i ++
                j --
            }
            flag = false
        } else {
            flag = true
        }
        res = append(res, nums)
        queue = t
    }
    return res
}
```

#  面试题33：二叉树的后序遍历序列

[leetcode](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

描述：输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 `true`，否则返回 `false`。假设输入的数组的任意两个数字都互不相同。

思路：使用递归实现，后序遍历的最后一个数肯定是根节点，那么就在前面找到左子树和右子树，然后递归判断，如果前面的右子树种存在小于根节点的数，则该二叉树不存在，返回false

代码：

```go
func verifyPostorder(postorder []int) bool {
    return helper(postorder, 0, len(postorder)-1)
}
func helper(nums []int, start, end int) bool {
    if end < start {
        return true
    }
    if end == start {
        return true
    }
    rN := nums[end]
    i := start
    for i < end {
        if nums[i] > rN {
            break
        }
        i ++
    }
    j := i
    for j < end {
        if nums[j] < rN {
            return false
        }
        j ++
    }
    return helper(nums, start, i-1) && helper(nums, i, end-1)
}
```

#  面试题34：二叉树中和为某一值的路径

[leetcode](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

描述：输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。

思路：使用递归前序遍历，遍历子节点的时候把sum减去当前节点的值；如果某个节点是叶结点而且和sum相等，则返回，负责返回空集。

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
func pathSum(root *TreeNode, sum int) [][]int {
    if root == nil {
        return [][]int{}
    }
    if root.Val == sum && root.Left == nil && root.Right == nil{
        return [][]int{{root.Val}}
    }
    if root.Val != sum && root.Left == nil && root.Right == nil{
        return [][]int{}
    }
    // if root.Val > sum {
    //     return [][]int{}
    // }
    l := pathSum(root.Left, sum-root.Val)
    r := pathSum(root.Right, sum-root.Val)
    res := [][]int{}
    for _, nums := range l {
        res = append(res, append([]int{root.Val}, nums...))
    }
    for _, nums := range r {
        res = append(res, append([]int{root.Val}, nums...))
    }
    return res
}
```

#  面试题35：复杂链表的复制

[leetcode](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

描述：请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。

思路：根据链表的每个结点N复制一个节点N'，然后把N'挂在N后面，之后复制其任意指针的指向关系。

代码：

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
class Solution {
    public Node copyRandomList(Node head) {
        dumplateList(head);
        setRandom(head);
        return getNewList(head);
    }
    public void dumplateList(Node head) {
        Node tmp = head;
        while (tmp != null) {
            Node n = new Node(tmp.val);
            n.next = tmp.next;
            tmp.next = n;
            tmp = n.next;
        }
    }
    public void setRandom(Node head) {
        Node tmp = head;
        while (tmp != null) {
            Node n = tmp.next;
            if (tmp.random == null) {
                n.random = null;
            } else {
                n.random = tmp.random.next;
            }
            
            tmp = n.next;
        }
    }
    public Node getNewList(Node head) {
        Node tmp = head;
        Node res = new Node(-1);
        Node preserve = res;
        while (tmp != null) {
            Node n = tmp.next;
            tmp.next = n.next;
            res.next = n;
            tmp = tmp.next;
            res = res.next;
        }
        return preserve.next;
    }
}
```

#  面试题36：二叉搜索树与双向链表

[leetcode](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

描述：输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

思路：使用中序遍历，一个pre结点，一个cur结点，pre的right指向cur，cur的left指向pre。

代码：

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val,Node _left,Node _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
    public Node treeToDoublyList(Node root) {
        if (root == null) {
            return root;
        }
        Stack<Node> stack = new Stack<>();
        // stack.Push(root);
        Node cur, pre, tail, head;
        pre = null;
        tail = null;
        head = null;
        cur = root;
        while(!stack.isEmpty() || cur != null) {
            while (cur != null) {
                stack.push(cur);
                cur = cur.left;
            }
            tail = stack.pop();
            cur = tail;
            if (pre == null) {
                head = cur;
            } else {
                pre.right = cur;
                cur.left = pre;
            }
            pre = cur;
            cur = cur.right;
        }
        tail.right = head;
        head.left = tail;
        return head;
    }
}
```

#  面试题37：序列化二叉树

[leetcode](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)

描述：请实现两个函数，分别用来序列化和反序列化二叉树。

思路：使用堆在数组中储存的概念进行序列化和反序列话，假设一个结点的位置为i，那么左子树和右子树结点的位置分别为2i+1和2i+2.依次类推，如果某一个子树不存在的话，使用null代替。

代码：[lemon-132的解答](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/solution/java-kong-jian-100-xu-lie-hua-er-cha-shu-by-lemon-/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {
    private String results; // 定义一个成员变量存储序列化结果

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null) {
            return "[]";
        } // root为null直接返回空字符串即可

        // 创建一个队列存储每一个非null节点
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root); // 将root先放进去
        results = "[" + root.val;
        while(!queue.isEmpty()) {
            // 每次从队列中取出一个节点，根据其左右子节点是否为null进行字符串拼接
            TreeNode tmp = queue.poll();
            if (tmp.left != null) {
                queue.offer(tmp.left);
                results += "," + tmp.left.val;
            } else {
                results += ",null";
            }
            // 上面处理left，下面处理right
            if (tmp.right != null) {
                queue.offer(tmp.right);
                results += "," + tmp.right.val;
            } else {
                results += ",null";
            }
        }
        // 处理完成之后添加上结束符
        results += "]";
        return results;
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data.length() == 2) {
            return null;
        }

        // 去除收尾的中括号字符
        data = data.substring(1, data.length() - 1);
        // 利用逗号分隔符获取每一个节点的值
        String[] vals = data.split(",");
        // 定义队列存储每一个有效节点，为了构建其左右子节点
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        TreeNode root = new TreeNode(Integer.valueOf(vals[0]));
        queue.offer(root); // 第一个元素为root节点
        int i = 1; // 标记后续处理的节点值（包含null）
        while (!queue.isEmpty()) {
            TreeNode tmp = queue.poll();
            // 从队列中取出节点，然后根据是否为null，依次设置left和right
            // 如果不是null，则需要加入队列，后续需要处理此有效节点的左右节点
            if (vals[i].equals("null")) {
                tmp.left = null;
            } else {
                tmp.left = new TreeNode(Integer.valueOf(vals[i]));
                queue.offer(tmp.left);
            }
            i++;
            if (vals[i].equals("null")) {
                tmp.right = null;
            } else {
                tmp.right = new TreeNode(Integer.valueOf(vals[i]));
                queue.offer(tmp.right);
            }
            i++;
        }

        return root;
    }
}
```

#  面试题38：字符串的排列

[leetcode](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

描述：输入一个字符串，打印出该字符串中字符的所有排列

思路：深度优先，递归

代码：

```go
func permutation(s string) []string {
    return helper([]byte(s))
}
func helper(bs []byte) []string {
    if len(bs) == 0 {
        return []string{}
    }
    if len(bs) == 1 {
        return []string{string(bs)}
    }
    res := []string{}
    m := make(map[string]bool)
    for i, b := range bs {
        tmp := make([]byte, len(bs)-1)
        copy(tmp[:i], bs[:i])
        copy(tmp[i:], bs[i+1:])
        t := helper(tmp)
        for _, ts := range t {
            if m[ts] {
                continue
            }
            m[ts] = true
            res = append(res, string(b)+ts)
        }
    }
    return res
}
```

#  面试题39：数组中出现次数超过一半的数字

[leetcode](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

描述：数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

思路：排序之后取中间位置值

代码：

```go
func majorityElement(nums []int) int {
    sort.Ints(nums)
    return nums[len(nums)/2]
}
```

