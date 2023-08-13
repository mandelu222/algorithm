## #24. Swap Nodes in Pairs
(https://leetcode.com/problems/swap-nodes-in-pairs/)

`题目：`
Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

`思路：`
就是先用curr locate到一个node，然后做swap，然后移动curr到下一个为止，继续swap。

`关注点：`
两个temp的使用。如果想不清楚就先画图，确定哪些node会被断开，然后用temp保存地址。while的condition要想清楚。

```python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # 因为可能需要对头指针进行操作，需要从head前一个node操作。所以使用dummyhead
        dummyNode = ListNode(float('inf'))
        dummyNode.next = head
        curr = dummyNode

        # 如果LL为偶数个，curr.next为空则结束；否则，curr.next.next为空结束
        # 同样适用于空LL
        while curr.next and curr.next.next:

            # 在接下来的过程中， 会有2个node被断开，先保存这个两个node
            temp1 = curr.next
            temp2 = curr.next.next.next
            # 然后进行swap，重新链接各个node
            curr.next = curr.next.next
            curr.next.next = temp1
            temp1.next = temp2
            # 往后移两位
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

`题目：`
Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return null.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to (0-indexed). It is -1 if there is no cycle. Note that pos is not passed as a parameter.

Do not modify the linked list.

`思路：`
联想到之前做过的一道找数组中两数和为target的题，觉得可以用dict来做。遍历LL一路存节点的地址到dict，只要某个节点的地址被存过，那这个就是被两次point到的要找的Node。

`关注点：`
发现视频给出的解答并不是我的方法，而是双指针法，感觉有点复杂。就先略过了。

```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:

        dummyNode = ListNode(float('inf'))
        dummyNode.next = head
        curr = dummyNode

        linked_list = {}

        while curr:
            if curr.next in linked_list.keys():
                return curr.next
            linked_list[curr.next] = 0
            curr = curr.next

        return None
```
然后发现上面写的挺傻逼的。为什么需要dict，明明用一个set就可以了，也不用dummy。。。或者next

```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        curr = head
        linked_list = set()

        while curr:
            if curr in linked_list:
                return curr
            linked_list.add(curr)
            curr = curr.next

        return None
```

这个是一个数学方法解出来的做法：详细解释见（https://www.bilibili.com/video/BV1if4y1d7ob/?spm_id_from=333.788&vd_source=5d163d78c46b415677c71a21dcf4977b)

总结一下就是：快慢指针，一个2步一个1步，如果有环终于会相遇(fast == slow)。如果相遇了，从相遇地点，和从head，2个指针一起移动，终将会在环入口相遇。

```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = fast = head

        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next

            if fast == slow: 
                index1 = fast
                index2 = head
                while index1 != index2:
                    index1 = index1.next
                    index2 = index2.next

                return index1

        return None
```
