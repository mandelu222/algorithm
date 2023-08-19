## #232. Implement Queue using Stacks

(https://leetcode.com/problems/implement-queue-using-stacks/)

`题目`

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (push, peek, pop, and empty).

Implement the MyQueue class:

`void push(int x)` Pushes element x to the back of the queue.

`int pop()` Removes the element from the front of the queue and returns it.

`int peek()` Returns the element at the front of the queue.

`boolean empty()` Returns true if the queue is empty, false otherwise.


`思路`
我的思路就是用两个stack。如果要pop，就先全部压入第二个stack，拿出第一个值，再把其他压回stack1。


```python
class MyQueue:

    def __init__(self):
        self.stack1 = []
        self.stack2 = []

    def push(self, x: int) -> None:
        self.stack1.append(x)

    def pop(self) -> int:

        # 自己写的时候把这个给忘记了
        if self.empty():
            return None

        while self.stack1:
            self.stack2.append(self.stack1.pop())

        pop_value = self.stack2[-1]

        self.stack2.pop()

        while self.stack2:
            self.stack1.append(self.stack2.pop())
            
        return pop_value

    def peek(self) -> int:
        return self.stack1[0]

    def empty(self) -> bool:
        return len(self.stack1) == 0

```


## #225. Implement Stack using Queues


(https://leetcode.com/problems/implement-stack-using-queues/)

`题目`

Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (push, top, pop, and empty).

Implement the MyStack class:

`void push(int x)` Pushes element x to the top of the stack.

`int pop()` Removes the element on the top of the stack and returns it.

`int top()` Returns the element on the top of the stack.

`boolean empty()` Returns true if the stack is empty, false otherwise.


```python
from queue import Queue

class MyStack:

    def __init__(self):
        self.que1 = Queue()
        self.que2 = Queue()

    def push(self, x: int) -> None:
        self.que1.put(x)

    def pop(self) -> int:

        if self.que1.qsize() == 0:
            return None

        for i in range(self.que1.qsize() - 1):
            x = self.que1.get()
            self.que2.put(x)

        value = self.que1.get()

        for i in range(self.que2.qsize()):         
            x = self.que2.get()
            self.que1.put(x)

        return value   

    def top(self) -> int:
        
        if self.que1.qsize() == 0:
            return None

        for i in range(self.que1.qsize()):
            x = self.que1.get()
            self.que2.put(x)

        value = x

        for i in range(self.que2.qsize()):
            x = self.que2.get()
            self.que1.put(x)
            
        return value

    def empty(self) -> bool:
        return self.que1.empty()
```