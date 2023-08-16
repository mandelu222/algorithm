## #344. Reverse String

(https://leetcode.com/problems/reverse-string/)

`题目`

Write a function that reverses a string. The input string is given as an array of characters s.

You must do this by modifying the input array in-place with O(1) extra memory.

`思路`

第一次试着用了s[::-1], 打印id(s)和id(s[::-1])会发现这个生成了新的字符串。不符合要求。

后来发现其实可以这样：

```python
 s[:] = s[::-1]
```

中规中矩的解答：
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left, right = 0, len(s) - 1

        while left < right:
            s[left], s[right] = s[right], s[left]
            left += 1
            right -= 1

        return s
```

## #541. Reverse String II

(https://leetcode.com/problems/reverse-string-ii/)

`题目`

Given a string s and an integer k, reverse the first k characters for every 2k characters counting from the start of the string.

If there are fewer than k characters left, reverse all of them. If there are less than 2k but greater than or equal to k characters, then reverse the first k characters and leave the other as original.

`思路`


## #151. Reverse Words in a String


(https://leetcode.com/problems/reverse-words-in-a-string/)

`题目`

Given an input string s, reverse the order of the words.

A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.

Return a string of the words in reverse order concatenated by a single space.

Note that s may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

`思路`
网站上的做法是 1）用双指针去除多余空格；2）翻转整个字符串；3）翻转各个单词。因为是用c++来讲的，这种方法可以实现空间复杂度O(n)。但是python的字符串是不变的，所以一定会生成新的字符串。空间复杂度不可能是O(n)。

双指针做法参考(https://leetcode.com/problems/remove-element)

```python
class Solution:
    def reverseWords(self, s: str) -> str:

        s = s.strip()
        worlds = []
        while s:
            space = s.find(" ")
            print(space)
            if space > 0:
                worlds.append(s[:space])
                s = s[space:].strip()
            else:
                worlds.append(s[:])
                s = ""

        world = worlds[::-1]
        return " ".join(world)
```
网站上的做法。对比一下发现我就是忘记了.split。所以写在分隔单词上的代码很麻烦。

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        # 删除前后空白
        s = s.strip()
        # 反转整个字符串
        s = s[::-1]
        # 将字符串拆分为单词，并反转每个单词
        s = ' '.join(word[::-1] for word in s.split())
        return s
```

## 剑指 Offer 05. 替换空格 LCOF

(https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)

`题目`

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

`思路`

```python
class Solution:
    def replaceSpace(self, s: str) -> str:

        s = s.replace(" ", "%20")

        return s
```


## 剑指 Offer 58 - II. 左旋转字符串

(https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

`题目`

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

`思路`

```python
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        s = s[n:]+ s[:n]
        return s
```
