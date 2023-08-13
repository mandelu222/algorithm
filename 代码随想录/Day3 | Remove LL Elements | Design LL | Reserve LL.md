
## #203 Remove Linked List Elements
(https://leetcode.com/problems/remove-linked-list-elements/)

`题目：`
Given the head of a linked list and an integer val, remove all the nodes of the linked list that has Node.val == val, and return the new head.

`思路：` 这个没什么好说的。常规操作。唯一特殊的地方是第一次写dummy头。

```python
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:

        dummyNode = ListNode(float('inf'))
        dummyNode.next = head
        curr = dummyNode

        while curr.next:
            if curr.next.val == val:
                curr.next = curr.next.next
            else:
                curr = curr.next

        return dummyNode.next

```

## #707 Design Linked List
(https://leetcode.com/problems/design-linked-list/)

`题目：`
Design your implementation of the linked list. You can choose to use a singly or doubly linked list. Implement the MyLinkedList class:

`MyLinkedList()` Initializes the MyLinkedList object.

`int get(int index)` Get the value of the indexth node in the linked list. If the index is invalid, return -1.

`void addAtHead(int val)` Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.

`void addAtTail(int val)` Append a node of value val as the last element of the linked list.

`void addAtIndex(int index, int val)` Add a node of value val before the indexth node in the linked list. If index equals the length of the linked list, the node will be appended to the end of the linked list. If index is greater than the length, the node will not be inserted.

`void deleteAtIndex(int index)` Delete the indexth node in the linked list, if the index is valid.

`思路：` 
这个题目真的是写的五味陈咋。各种崩溃。因为一定不想看答案，结果就是不停有edge case崩掉。每个method都define一个dummy node也是很崩溃。结果队友让我直接在initiate LL class的时候用dummy Node。这样这个LL object就总是会有一个dummy Node。因为永远不会操作这个Node本身，而是从dummy.next开始，所以也不用一直维护一个real head。
然后看了老师的答案，发现最重要的一个difference就是**maintain一个size的featrue**。

`关注点：`
1. 一直维护一个LL的长度信息； 2. 总是指向要操作节点的前一个节点, or always work on curr.next

```python

class Node:
    def __init__(self, value, next=None):
        self.value = value
        self.next = next

class MyLinkedList:

    def __init__(self):
        self.dummy = Node(float("inf"))
        self.size = 0          # <------这个是最最最最重要的。解决了几乎所有判断 index invalid的问题


    def get(self, index: int) -> int:
        if index < 0 or index > self.size - 1: 
            return -1
        
        curr = self.dummy.next     # 如果index为0就直接是head。所以可以直接从dummy.next开始。如果index为0就不用进循环
        
        for i in range(index):
            curr = curr.next

        return curr.value

    def addAtHead(self, val: int) -> None:

        newNode = Node(val)
        newNode.next = self.dummy.next
        self.dummy.next = newNode

        self.size += 1


    def addAtTail(self, val: int) -> None:

        curr = self.dummy

        newNode = Node(val)

        while curr.next:
            curr = curr.next

        curr.next = newNode

        self.size += 1


    def addAtIndex(self, index: int, val: int) -> None:

        if index < 0 or index > self.size:     # 这里index判断有一点点不同，允许index的值为LL长额度
            return

        newNode = Node(val)
        curr = self.dummy      # 总是从dummy表头开始，这样就一直操作curr.next。curr总是指向需操作节点的前一个位置

        for i in range(index):
            curr = curr.next
        
        newNode.next = curr.next
        curr.next = newNode

        self.size += 1


    def deleteAtIndex(self, index: int) -> None:
        
        if index < 0 or index > self.size - 1:
            return

        curr = self.dummy

        for i in range(index):
            curr = curr.next

        curr.next = curr.next.next

        self.size -= 1
```

## #206 Reverse Linked List
(https://leetcode.com/problems/reverse-linked-list/)

`题目：`
Given the head of a singly linked list, reverse the list, and return the reversed list.

`思路：` 第二个LL从结尾None开始，然后不断把第一个LL的Node连去第二个LL。

`关注点：`操作2个LL或者有需要断开一个node的情况时，用temp暂时存住Node的信息。

```python

class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None:
            return None

        # 不存在插入、删除元素，没有必要用虚拟节点，直接从头结点开始
        curr = head   # 1. 用于track第一个LL
        pre = None    # 2. 用于track第二个LL，从后向前，None 即为最后的tail

        while curr:  # 只要原LL还有值就继续操作
            temp = curr.next    # 如果直接把curr.next指向pre(连去LL2), 会丢失LL1的track。所以要先存一个temp;    
            curr.next = pre     # LL1当前Node指向LL2，与LL1断开；
            pre = curr          # LL2向后移动一位；
            curr = temp         # LL1向后移动一位；

        return pre   # 必须返回新LL头节点
```
坑爹的S(n)空间复杂度的递归法：完全根据iteration的方法写出来。其实完全是把遍历用递归写了一遍。

```python

class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None:
            return None
        return self.reserve(head, None)   # curr 和 pre 的初始值

    def reserve(self, curr: Optional[ListNode], pre: Optional[ListNode]):
        if curr is None:     # 终止条件，直接return pre
            return pre

        temp = curr.next   # 和iteration法一毛一样的两行
        curr.next = pre

        return self.reserve(temp, curr)   # 递归，move curr 和 pre to next, and reverse curr.next (which is stored in temp) 和 pre (which is curr)
```
