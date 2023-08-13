## 心得体会

1. Dummy Head. 只要有可能对head进行操作，最好使用dummy head, 这样就不用对head专门处理;
2. 只要是需要对node进行增、删、改link之类的操作，都要把curr指向需修改node的前一个node;
3. 对于rotate、swap、merge之类的操作，画图以确定是否会断开某些node，提前使用temp指针保存这些node的位置；
4. 对于loop的condition，总是要考虑是非会对空节点进行操作; 特别是对于快慢指针等需要有.next.next之类的情况；
5. 对LL进行完修改后需要返回head(dummyNode.next);


## 题型总结

### insert node
https://leetcode.com/problems/design-linked-list/
### delete node
https://leetcode.com/problems/remove-linked-list-elements/

https://leetcode.com/problems/remove-nth-node-from-end-of-list/
### swap nodes
https://leetcode.com/problems/swap-nodes-in-pairs/

https://leetcode.com/problems/swapping-nodes-in-a-linked-list/
### find the middle of the list
https://leetcode.com/problems/middle-of-the-linked-list/
### rotate list
https://leetcode.com/problems/rotate-list/
### reverse list
https://leetcode.com/problems/reverse-linked-list/
### merge list
https://leetcode.com/problems/reorder-list/
### cyclic list 
https://leetcode.com/problems/linked-list-cycle-ii/

https://leetcode.com/problems/intersection-of-two-linked-lists/


