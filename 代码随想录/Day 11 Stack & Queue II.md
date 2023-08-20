## #20. Valid Parentheses


(https://leetcode.com/problems/valid-parentheses/)

`题目`

Given a string s containing just the characters `'(', ')', '{', '}', '[' and ']'`, determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.


```python
class Solution:
    def isValid(self, s: str) -> bool:

        stack = []

        for ch in s:
            if ch in ["(", "{", "["]:
                stack.append(ch)
            # 总是要考虑空stack的问题
            elif (len(stack) > 0 and ((ch == ")" and stack[-1] == "(") or (ch == "}" and stack[-1] == "{") or (ch == "]" and stack[-1] == "["))):
                stack.pop()
            else:
                return False
        
        # if not stack:
        #     return True
        return not stack   # 看上去code更紧凑
```


## #1047. Remove All Adjacent Duplicates In String

(https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

`题目`

You are given a string s consisting of lowercase English letters. A duplicate removal consists of choosing two adjacent and equal letters and removing them.

We repeatedly make duplicate removals on s until we no longer can.

Return the final string after all such duplicate removals have been made. It can be proven that the answer is unique.

```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        stack = []
        for ch in s:
            if stack and stack[-1] == ch:
                stack.pop()
            else:
                stack.append(ch)

        string = ""
        if not stack:
            return ""
        else:
            while stack:
                string = stack.pop() + string

        return string
```
后面整个部分都可以被省掉！

```python

class Solution:
    def removeDuplicates(self, s: str) -> str:
        stack = []
        for ch in s:
            if stack and stack[-1] == ch:
                stack.pop()
            else:
                stack.append(ch)

        return "".join(stack)

```



## #150. Evaluate Reverse Polish Notation


(https://leetcode.com/problems/evaluate-reverse-polish-notation/)

`题目`

You are given an array of strings tokens that represents an arithmetic expression in a Reverse Polish Notation.

Evaluate the expression. Return an integer that represents the value of the expression.

Note that:

The valid operators are '+', '-', '*', and '/'.
Each operand may be an integer or another expression.
The division between two integers always truncates toward zero.
There will not be any division by zero.
The input represents a valid arithmetic expression in a reverse polish notation.
The answer and all the intermediate calculations can be represented in a 32-bit integer.

```python

class Solution:
    def evalRPN(self, tokens: List[str]) -> int:

        stack = []

        for token in tokens:
            if token in ("+", "-", "*", "/"):
                num1 = stack.pop()
                num2 = stack.pop()
                num = self.apply_operation(num2, num1, token)
                stack.append(num)
            else:
                # 第一次用了isinstance, 然后发现数字都是str
                stack.append(int(token))

        return stack.pop()


    def apply_operation(self, num1, num2, token):
        if token == "+":
            return num1 + num2
        if token == "-":
            return num1 - num2
        if token == "*":
            return num1 * num2
        if token == "/":
            # 主要时间花在这里。需要让 6/-132 = 0， 发现只有这个写法。
            return int (num1 / num2)
```

看了网站上的做法有两个替代了`apply_operation`这个挺丑的function：第一个用了lambda和operator library; 第二个用了超级慢的eval。

```python
from operator import add, sub, mul

class Solution:

    op_map = {'+': add, '-': sub, '*': mul, '/': lambda x, y: int(x / y)}
    
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for token in tokens:
            if token not in {'+', '-', '*', '/'}:
                stack.append(int(token))
            else:
                op2 = stack.pop()
                op1 = stack.pop()
                stack.append(self.op_map[token](op1, op2))  # 第一个出来的在运算符后面
        return stack.pop()

```

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for item in tokens:
            if item not in {"+", "-", "*", "/"}:
                stack.append(item)
            else:
                first_num, second_num = stack.pop(), stack.pop()
                stack.append(
                    int(eval(f'{second_num} {item} {first_num}'))   # 第一个出来的在运算符后面
                )
        return int(stack.pop()) # 如果一开始只有一个数，那么会是字符串形式的
```