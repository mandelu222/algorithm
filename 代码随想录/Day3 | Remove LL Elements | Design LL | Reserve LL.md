
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

`关注点：`

```python


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
