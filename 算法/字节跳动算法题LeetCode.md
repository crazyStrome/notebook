<!-- TOC -->

- [无重复字符的最长子串](#无重复字符的最长子串)
- [整数反转](#整数反转)
- [最长回文子串](#最长回文子串)
- [接雨水](#接雨水)
- [合并两个有序链表](#合并两个有序链表)
- [最大自序和](#最大自序和)
- [三数之和](#三数之和)
- [K个一组反转链表](#k个一组反转链表)
- [合并区间](#合并区间)
- [全排列](#全排列)

<!-- /TOC -->
#  无重复字符的最长子串

描述：给定一个字符串，找出不含有重复字符的最长子串

例如："abcabcbb" 3

思路：使用滑动窗口，双指针移动，判断right字符是否在窗口，没有就继续，同时更新最大值；有的话就修改left值。

代码：

```go
func lengthOfLongestSubstring(s string) int {
    var res int
    var left, right int

    s1 := s[left:right]
    for ; right < len(s); right ++ {
        if index := strings.IndexByte(s1, s[right]); index != -1 {
            left += index+1
        }
        s1 = s[left: right+1]
        if res < len(s1) {
            res = len(s1)
        }
    }
    return res
}
```

#  整数反转

给出一个32位整数，将其每一位反转，考虑溢出。

```go
func reverse(a int) int {
    y := 0
    for a != 0 {
        y = y * 10 + a % 10
        if !(math.MinInt32<= y && y <= math.MaxInt32) {
            return 0
        }
        a /= 10
    }
    return y
}
```

#  最长回文子串

描述：给定一个字符串，找出最长回文子串。

```go
func longestPalindrome(s string) string {
	if len(s) < 1  {
		return ""
	}
	start, end := 0, 0
	for i := 0; i < len(s); i++ {
		len1 := expandAroundCenter(s, i, i)
		len2 := expandAroundCenter(s, i, i + 1)
		lenn := Max(len1, len2)
		if lenn > end - start {
			start = i - (lenn - 1) / 2
			end = i + lenn / 2
		}
	}
	return s[start:end + 1]
}

func Max(x, y int) int {
    if x > y {
        return x
    }
    return y
}


func expandAroundCenter(s string, l int, r int) int {
	for {
		if l >= 0 && r < len(s) && s[l] == s[r] {
			l--
			r++
		} else {
			break;
		}
	}
	return r - l - 1
}

```

#  接雨水

描述： 给定 *n* 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。 

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。

代码：

```go
func trap(height []int) int {
    leftMax := make([]int, len(height))
    rightMax := make([]int, len(height))
    left, right := 0, len(height)-1
    for left < len(height) && right >= 0 {
        if right == len(height)-1 {
            rightMax[right] = 0
        } else {
            rightMax[right] = max(rightMax[right+1], height[right+1])
        }
        if left == 0 {
            leftMax[left] = 0
        } else {
            leftMax[left] = max(leftMax[left-1], height[left-1])
        }
        left ++
        right --
    }
    fmt.Println(leftMax)
    fmt.Println(rightMax)
    sum := 0
    for i := 0; i < len(height); i ++ {
        top := min(leftMax[i], rightMax[i])
        if top >= height[i] {
            sum = sum + top - height[i]
        }
    } 
    return sum
}
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
func min(a, b int) int {
    return a + b - max(a, b)
}
```

#  合并两个有序链表

将两个有序链表合并为一个新的链表

代码：

```go
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    if l1 == nil {
        return l2
    }
    if l2 == nil {
        return l1
    }
    var n *ListNode
    if l1.Val > l2.Val {
        n = l2.Next
        l2.Next = mergeTwoLists(l1, n)
        return l2
    } else {
        n = l1.Next
        l1.Next = mergeTwoLists(n, l2)
        return l1
    }
}
```

#  最大自序和

描述：给定一个数组nums，找到具有最大子序和的连续子数组，返回最大和。

思路：使用动态规划，一样大的dp数组，dp[i]表示截止到当前位置且包括的连续最大子序和，`dp[i]=max(dp[i-1]+nums[i], nums[i])`

代码：

```go
func maxSubArray(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    dp := make([]int, len(nums))
    dp[0] = nums[0]
    res := nums[0]
    for i := 1; i < len(nums); i ++ {
        dp[i] = max(nums[i], nums[i]+dp[i-1])
        res = max(dp[i], res)
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

#  三数之和

描述：给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。

代码：

```go
func threeSum(nums []int) [][]int {
	answer := [][]int{}
	if len(nums) < 3{
		return answer
	}
	sort.Ints(nums)
	for index, elem := range nums{
		if elem > 0 {
			return answer
		}

		if index > 0{
			if elem == nums[index - 1]{
				continue
			}
		}

		L := index + 1
		R := len(nums) - 1
		for L < R {
			if elem + nums[L] + nums[R] == 0{
				answer = append(answer, append([]int{}, elem, nums[L], nums[R]))
				for  L < R && nums[L] == nums[L+1]{
					L += 1
				}
				for L < R && nums[R] == nums[R-1]{
					R -= 1
				}
				L += 1
				R -= 1
			} else if elem + nums[L] + nums[R] > 0{
				R -= 1
			} else if elem + nums[L] + nums[R] < 0{
				L += 1
			}
		}
	}
	return answer
}
```

#  K个一组反转链表

描述：给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。k 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

示例：

给你这个链表：1->2->3->4->5

当 k = 2 时，应当返回: 2->1->4->3->5

当 k = 3 时，应当返回: 3->2->1->4->5

思路：使用递归的方式，每次只找到K的链表，把他和后序链表分割开，然后反转，原来的头结点变成尾结点，这样原来的head后面直接挂递归后序结点返回的值。

代码：

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseKGroup(head *ListNode, k int) *ListNode {
    j := k
    tmp := head
    for k > 1 && tmp != nil {
        tmp = tmp.Next
        k --
    }
    if tmp == nil {
        return head
    }
    n := tmp.Next
    tmp.Next = nil
    child := reverseKGroup(n, j)
    reverse(head)
    head.Next = child
    return tmp
}
func reverse(root *ListNode) *ListNode {
    var pre, next *ListNode

    for root != nil {
        next = root.Next
        root.Next = pre
        pre = root
        root = next
    }
    return pre
}
```

#  合并区间

描述：给出一个区间的集合，请合并所有重叠的区间。

例如：

输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].

思路：先按照左区间进行排序，然后依次判断区间重叠，如果重叠，根据重叠类型进行合并。

```go
func merge(intervals [][]int) [][]int {
    if len(intervals) <= 1 {
        return intervals
    }
    sort.Sort(Inters(intervals))
    res := [][]int{}
    for _, in := range intervals {
        if len(res) == 0 {
            res = append(res, in)
            continue
        }
        l := len(res)
        if in[0] <= res[l-1][1] {
            if in[1] <= res[l-1][1] {
            } else {
                res = append(res[:l-1], []int{res[l-1][0], in[1]})
            }
        } else {
            res = append(res, in)
        }
    }
    return res
}
type Inters [][]int
func (is Inters) Swap(i, j int) {
    is[i], is[j] = is[j], is[i]
}
func (is Inters) Less(i, j int) bool {
    return is[i][0] < is[j][0]
}
func (is Inters) Len() int {
    return len(is)
}
```

#  全排列

给定一个 没有重复 数字的序列，返回其所有可能的全排列。

示例:

输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

思路：使用广度优先算法

```go
func permute(nums []int) [][]int {
    if len(nums) == 1 {
        return [][]int{{nums[0]}}
    }
    result := [][]int{}
    for index, value := range nums {
        var numsCopy = make([]int, len(nums))
        copy(numsCopy, nums)
        numsSubOne := append(numsCopy[:index],numsCopy[index+1:]...)
        valueSlice := []int{value}
        newSubSlice := permute(numsSubOne)
        for _, newValue := range newSubSlice {
            result = append(result, append(valueSlice, newValue...))
        }
    }
    return result
}
```

