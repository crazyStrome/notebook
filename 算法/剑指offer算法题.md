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