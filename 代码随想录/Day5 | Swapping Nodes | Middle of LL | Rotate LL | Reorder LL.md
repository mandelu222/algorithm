
## #1721. Swapping Nodes in a Linked List
(https://leetcode.com/problems/swapping-nodes-in-a-linked-list/)

`题目：`
You are given the head of a linked list, and an integer k.

Return the head of the linked list after swapping the values of the kth node from the beginning and the kth node from the end (the list is 1-indexed).


## #876. Middle of the Linked List
(https://leetcode.com/problems/middle-of-the-linked-list/)

`题目：`
Given the head of a singly linked list, return the middle node of the linked list.

If there are two middle nodes, return the second middle node.

`思路：`一半的问题一般都想到快慢指针，快比慢多走一步。

`关注点：`see comment below。一个比较好的体会是先拿走特别的情况(空，1 node，2 node），然后发现加会来程序也可以work的例子。

```python
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:

 # 不需要，下面的code可以handle.
        # if head is None:
            # return None

        fast = slow = head

 # 一开始的确想到fast可以会move到最后的None的位置，但是忘记这里会需要再判断一次。所以必须是fast和fast.next都不为空的两个条件。
 # 之前光写了fast.next, 结果fast本身挪到空的位置的时候这里就会是在操作空指针，crash
        while fast and fast.next:  
            fast = fast.next.next
            slow = slow.next

        return slow
```


## #61. Rotate List
(https://leetcode.com/problems/rotate-list/)

`题目：`
Given the head of a linked list, rotate the list to the right by k places.

## #143. Reorder List
(https://leetcode.com/problems/reorder-list/)

`题目：`
You are given the head of a singly linked-list. The list can be represented as:

L0 → L1 → … → Ln - 1 → Ln
Reorder the list to be on the following form:

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
You may not modify the values in the list's nodes. Only nodes themselves may be changed.
