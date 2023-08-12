## #24. Swap Nodes in Pairs
(https://leetcode.com/problems/swap-nodes-in-pairs/)

`题目：`
Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

`思路：`
就是先用curr locate到一个node，然后做swap，然后移动curr到下一个为止，继续swap。

`关注点：`
刚才用双指针很顺利，就老想用。其实这个情况用一个curr确定下一次循环时停下来的位置就好了。比较tricky的是用temp给两个node换位置的操作。预先知道需要curr，temp1和temp2，我写出来了。但如果自己去decide需要哪些辅助variable还是做不出。

另外就是while的条件和特殊情况的处理。如剩下一个node。其实都可以generalize。我之前专门写一句就多余了。

```python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:

        dummyNode = ListNode(float('inf'))
        dummyNode.next = head

        # if dummyNode.next is None or dummyNode.next.next is None:
        #     return dummyNode.next    多余

        curr = dummyNode

        while curr.next and curr.next.next:
            temp1 = curr.next
            temp2 = curr.next.next

            temp1.next = temp2.next
            temp2.next = temp1
            curr.next = temp2

            curr = curr.next.next

        return dummyNode.next

```



## #19. Remove Nth Node From End of List
(https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

`题目：`
Given the head of a linked list, remove the nth node from the end of the list and return its head.

`思路：`
这个很显而易见啊，当然是要两个指针隔n步了。自己很快就写出来了。

`关注点：`
关于while的条件还是要继续练习，这次唯一的问题就出在while的判断和移指针的先后次序。还有一点就是，delete node指针前后一定要多隔一位，才能用next = next.next

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:

        dummyNode = ListNode(float('inf'))
        dummyNode.next = head

        slow = fast = dummyNode

        while n > 0:
            fast = fast.next
            n -= 1

        while fast.next: 
            fast = fast.next
            slow = slow.next

        slow.next = slow.next.next

        return dummyNode.next
```

## #160. Intersection of Two Linked Lists
(https://leetcode.com/problems/intersection-of-two-linked-lists/)

`题目：`
Given the heads of two singly linked-lists headA and headB, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null.

`思路：`
没思路，一点思路都没有。。。自己写了个暴力破解的，不出意外的系统蹦了。后来看老师的思路才写出来。

`关注点：`
其实看到思路之后很快就写出来了。感触比较大的是refactor的过程：1. 没有写self.; 2. leetcode需要pass parameter的type，这个第一次知道；3. move_n_step这个忘记返回的curr position要assign给对应的pointer。都来回debug了几次。不过算是第一次在leetcode里写function了。**It is much cleaner after refactor!**

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        a = headA
        b = headB

        lenA = lenB = 0
        while a:
            lenA +=1 
            a = a.next
        while b:
            lenB +=1 
            b = b.next

        a = headA
        b = headB
        
        diff = lenA - lenB

        if diff > 0:
            for i in range(diff):
                a = a.next           
        else:
            for i in range(-diff):
                b = b.next
        while a:
            if a == b:
                return a
            a = a.next
            b = b.next
        return None
```
Refactor the code to put 2 repeated code into function:

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        # get length of both LL; 
        lenA = self.get_length(headA)    
        lenB = self.get_length(headB)

        a = headA
        b = headB
    
        diff = lenA - lenB
        # move the pointer of the longer LL,
        # so that both pointers points at the nodes where the remaining of the LLs have the same length
        # if there is intersaction, of coz 2 LLs are 右对齐
        if diff > 0:
            a = self.move_n_step(a, diff)
        else:
            b = self.move_n_step(b, -diff)

        # move both pointers together, return the node where both have the same reference (not value)
        while a:
            if a == b:
                return a
            a = a.next
            b = b.next
        return None

    def get_length(self, head: ListNode):
        length = 0
        curr = head
        while curr:
            length += 1
            curr = curr.next
        return length

    def move_n_step(self, curr: ListNode, n: int):
        for i in range(n):
            curr = curr.next
        return curr
```


## #142. Linked List Cycle II
(https://leetcode.com/problems/linked-list-cycle-ii/)

Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return null.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to (0-indexed). It is -1 if there is no cycle. Note that pos is not passed as a parameter.

Do not modify the linked list.
