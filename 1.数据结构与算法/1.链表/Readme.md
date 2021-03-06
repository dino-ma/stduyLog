# 链表

- 链表是因为优化数组的插入和删除时间而产生的数据结构

- 链表的插入和删除的平均复杂度为O(1)

- 链表的查找时间平均复杂度为O(n)



##  206. 链表反转 reverse-linked-list

```go
反转一个单链表。
示例:
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL

进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-linked-list/ 
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

### 迭代解法

```go
package main

import "fmt"

type ListNode struct {
	Val  int
	Next *ListNode
}

func reversLinkList(head *ListNode) *ListNode {
	var pre *ListNode = nil

	for head != nil {
		next := head.Next // step 1 先存下下一个节点，不然一会就没了
		head.Next = pre   // 当前节点指向上一个节点
		pre = head        // 上一个节点为当前节点
		head = next       // step 2 将head做移动
	}

	return pre
}

func showLinkList(head *ListNode) {
	for head != nil {
		fmt.Println(head.Val)
		head = head.Next
	}
}

func main() {
	head := &ListNode{Val: 0}
	tail := head
	for i := 1; i < 10; i++ {
		tail.Next = &ListNode{Val: i}
		tail = tail.Next
	}

	showLinkList(head)
	resversList := reversLinkList(head)
	fmt.Println("revers:")
	showLinkList(resversList)
}
```

### 24. 两两交换链表中的节点

```go
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

 

示例:

给定 1->2->3->4, 你应该返回 2->1->4->3.

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/swap-nodes-in-pairs
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

### 解法：原地交换

```go
package main

import (
	"fmt"
)

type ListNode struct{
	Next *ListNode
	Val int
}


func makeListNode(nums []int) *ListNode {
	if len(nums) == 0{
		return nil
	}

	res := &ListNode{
		Val:nums[0],
	}

	temp := res

	for i := 1; i < len(nums); i++ {
		temp.Next = &ListNode{Val:nums[i],}
		temp = temp.Next
	}

	return  res
}

/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func swapPairs(head *ListNode) *ListNode {
	if head == nil || head.Next == nil  {
		return head
	}
	p := &ListNode{Next:head}
	h := p
	for p!=nil && p.Next != nil  {
		a := p.Next // step 1 first 
		b := a.Next // step 2 second
		a.Next = b.Next //step 4 拿到换位置之后正确的next指针
		p.Next = b  //step 4 p正确的指向应为b
		b.Next = a  //step 6 b正确的指向应为a
		p = a   //step 3 p节点移动到A的位置进行下一次的两两交换
	}


	return h
}

func main()  {

	a := makeListNode([]int{1,2,3,4})
	//for a != nil {
	//	fmt.Println(a.Val)
	//	a = a.Next
	//}
	result := swapPairs(a)
	//
	for result != nil {
		fmt.Println(result.Val)
		result = result.Next
	}
}
```





### 114.判断链表是否有环

```
给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

示例 1：

输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。


示例 2：

输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。

示例 3：

输入：head = [1], pos = -1
输出：false
解释：链表中没有环。

进阶：

你能用 O(1)（即，常量）内存解决此问题吗？

通过次数208,497提交次数425,414

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/linked-list-cycle
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```



### 解法：快慢指针

```go
package main

import "fmt"

type ListNode struct {
	Next *ListNode
	Val  int
}

func hasCycle(head *ListNode) bool {
	if head == nil {
		return false
	}

	fast := head.Next
	for fast !=nil && fast.Next !=nil  {
		if fast == head {
			return true
		}
		head = head.Next
		fast = fast.Next.Next
	}

	return false
}

func main() {

	//make cycle
	head := &ListNode{Val: 0}
	next := &ListNode{Val: 1}
	head.Next = next
	second := &ListNode{Val: 2}
	next.Next = second
	second.Next = head

	fmt.Println(hasCycle(head))

}
```

### 142. 环形链表 II

```go
给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

说明：不允许修改给定的链表。

 

示例 1：

输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。


示例 2：

输入：head = [1,2], pos = 0
输出：tail connects to node index 0
解释：链表中有一个环，其尾部连接到第一个节点。


示例 3：

输入：head = [1], pos = -1
输出：no cycle
解释：链表中没有环。


 

进阶：
你是否可以不用额外空间解决此题？



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/linked-list-cycle-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

### 解法

```
package main

import "fmt"

type ListNode struct {
	Val int
	Next *ListNode
}

/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func detectCycle(head *ListNode) *ListNode {
	//如果head为空或者head的指针为空则当前链表无环
	if head == nil || head.Next == nil {
		return nil
	}
	//初始化快慢指针，
	slow ,fast := head, head
	//以快指针为主（减少循环次数）
	for fast != nil &&fast.Next != nil && fast.Next.Next !=nil {
		slow = slow.Next//每次走一步
		fast = fast.Next.Next//每次走两步
		//如果快慢指针相遇
		if slow == fast {
			//寻找相交的头节点，并且将快指针改变为每次只走一步
			for head != fast {
				head = head.Next
				fast = fast.Next
			}
			return head
		}

	}

	return nil
}


func main(){
	//make cycle ii
	head := &ListNode{Val:3}
	next := &ListNode{Val:2}
	head.Next = next
	second := &ListNode{Val:0}
	next.Next = second

	third := &ListNode{Val:4}
	second.Next = third
	third.Next = next


	result := detectCycle(head)
	fmt.Println(result.Val)

}
```



### 21. 合并两个有序链表

```
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

 

示例：

输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/merge-two-sorted-lists
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

### 解法：迭代

```
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
   if l1 == nil && l2 == nil {
		return nil

	}
	p := &ListNode{}
	result := p
	for l1 != nil && l2 != nil {
		if l1.Val < l2.Val {
			p.Next = l1
			l1 = l1.Next
		} else {
			p.Next = l2
			l2 = l2.Next
		}

		p = p.Next
	}

	if l1 == nil {
		p.Next = l2
	}

	if l2 == nil {
		p.Next = l1
	}

	return result.Next
}
```





## 双链表



## 跳表

