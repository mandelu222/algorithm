
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

`关注点：`
空LL和index invalid是最崩溃的情况，为了handle add/delete at index， 专门写了一个get_index_node的function，返回LL index**前**一个node。
`晚一点可以考虑用双节点再写一次。`

```python

class Node:
    def __init__(self, value, next=None):
        self.value = value
        self.next = next

class MyLinkedList:

    def __init__(self):
        self.dummy = Node(float("inf"))

    def get(self, index: int) -> int:

        curr = self.dummy

        for i in range(index):
            if curr.next:
                curr = curr.next
            else:
                return -1

        return curr.next.value if curr.next else -1

    def addAtHead(self, val: int) -> None:

        newNode = Node(val)
        newNode.next = self.dummy.next
        self.dummy.next = newNode

    def addAtTail(self, val: int) -> None:

        curr = self.dummy

        newNode = Node(val)

        while curr.next:
            curr = curr.next

        curr.next = newNode

    def get_index_node(self, index: int):

        curr = self.dummy

        if curr.next is None:
            return None

        for i in range(index):
            if not curr.next:
                return None
            else:
                curr = curr.next
                i += 1
        
        return curr

    def addAtIndex(self, index: int, val: int) -> None:

        newNode = Node(val)  

        curr = self.get_index_node(index)

        if curr:
            newNode.next = curr.next
            curr.next = newNode

        elif curr is None and self.dummy.next is None and index == 0:   # handle empty LL and index is 0
            self.addAtHead(val) 


    def deleteAtIndex(self, index: int) -> None:
        
        curr = self.get_index_node(index)    
        if curr and curr.next:
            curr.next = curr.next.next
```

## #206 Reverse Linked List
(https://leetcode.com/problems/reverse-linked-list/)

`题目：`
Given the head of a singly linked list, reverse the list, and return the reversed list.

`思路：` 完全是从别人的视频里看来的。。。

`关注点：`

```python

class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None:
            return head

        dummyNode = ListNode(float('inf'))

        curr = head

        while curr:   #只要现在这个node有值就要处理这个node
            new_curr = curr                  # 用一个新的pointer先保存住node A的地址
            curr = curr.next                 # tracking pointer可以move到下一个node B了
            new_curr.next = dummyNode.next   # 现在A可以跟B断开了，用dummy Node作为新LL的虚拟头，不断把跟之前LL断开的Node插入dummy和dummy.next之间
            dummyNode.next = new_curr        
            
        return dummyNode.next
```
