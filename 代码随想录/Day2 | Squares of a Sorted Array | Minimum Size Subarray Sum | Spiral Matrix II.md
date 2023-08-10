## #977 Squares of a Sorted Array
(https://leetcode.com/problems/squares-of-a-sorted-array/)

`题目：`
Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

`思路：`第一个想法就是两个指针往中间并，不断比较绝对值，取绝对值大的数往新数组里放，然后移动指针，直到两个指针重合；最后需要把重合点处理一下；

`关注点：`看了视频之后发现自己花了太多code处理边际情况了，其实完全可以把两者相等看成else，这样随便只挪一边的pointer，下一轮另外一个一定会被pickup；而且while加上等于号，就不用单独处理最后重合的情况了

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        left = 0
        right = len(nums)-1

        answer = [0 for _ in range(len(nums))]
        i = len(nums)-1

        while left < right:
            if (abs(nums[left]) < abs(nums[right])):
                answer[i] = nums[right] ** 2
                right -= 1
            elif(abs(nums[left]) > abs(nums[right])):
                answer[i] = nums[left] ** 2
                left += 1
            else:
                answer[i] = nums[left] ** 2
                i -= 1
                answer[i] = nums[left] ** 2
                left += 1
                right -= 1

            i -= 1

       # 其实这里是错的。如果最后left right指向[-1, 1], 执行完上面的代码后出循环。
       # 理论上是不用再多写这么一行的，但我写了也没问题，因为把同样的数字重新又给answer[0]赋值了一遍。但写的code逻辑性问题很大。

        answer [0] =  nums[right] ** 2  
        
        return answer
```

看视频后更新后的代码：

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        left = 0
        right = len(nums)-1

        answer = [0 for _ in range(len(nums))]
        i = len(nums)-1

        while left <= right:    # 这里直接考虑相等的情况，两个pointer指向同一个数据就会被else给handle了
            if (abs(nums[left]) < abs(nums[right])):
                answer[i] = nums[right] ** 2
                right -= 1
            else:
                answer[i] = nums[left] ** 2
                left += 1
            i -= 1
        
        return answer
```

## #209 Minimum Size Subarray Sum
(https://leetcode.com/problems/minimum-size-subarray-sum/)

`题目：`
Given an array of positive integers nums and a positive integer target, return the minimal length of a 
subarray whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.

`思路：`还是和第一题一样，滑动窗口的思路一下子就有了，写code写了很久；

`关注点：`其实听了视频之后，发现滑动窗口的implementation应该是固定住快的一点， 然后不停往前试探滑动后面的一点，而我写的是在滑动快的时候分析两种情况。虽然code差不多，但是感觉上改过的比较容易顺畅的写出来。

`拓展： ` **#904**， **#76**

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        slow = 0
        fast = 0
        length = len(nums)

        min_len = length + 1
        sum_value = nums[0]

        while fast <= length -1:
            if sum_value < target:
                fast += 1
                if fast <= length -1:
                    sum_value += nums[fast]
            else:
                min_len = min(min_len, fast - slow + 1) 
                sum_value -= nums[slow]
                slow += 1

        return 0 if min_len > length else min_len
```
看视频后更新后的代码：

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:

        slow = 0
        min_len = float('inf')  # 我自己用了len+1, 感觉都差不多；
        sum_value = 0

        for fast in range(len(nums)):
            sum_value += nums[fast]     # 不用事先assign first value; 这样也就不用像我还要处理溢出的情况
            while sum_value >= target:   # 定住fast然后不停试探的滑动slow
                min_len = min(min_len, fast - slow + 1)
                sum_value -= nums[slow]
                slow += 1

        return min_len if min_len != float('inf') else 0
```

## #59 Spiral Matrix II
(https://leetcode.com/problems/spiral-matrix-ii/)

`题目：`
Given a positive integer n, generate an n x n matrix filled with elements from 1 to n2 in spiral order.

`思路：`这个没有思路，早晨的车上和队友讨论了一下他认为很简单，用2-dimension array就可以了。感觉说起来很简单。

`关注点：` 这里直接复制一下文字解说的一部分内容：

和二分法一样， 求解本题要坚持**循环不变量原则**。

画矩阵的过程: 上行从左到右； 右列从上到下； 下行从右到左； 左列从下到上。 

由外向内一圈一圈这么画下去。这里一圈下来，我们要画每四条边，这四条边怎么画，每画一条边都要坚持一致的**左闭右开**，或者左开右闭的原则，这样这一圈才能按照统一的规则画下来。

![image](https://github.com/mandelu222/algorithm/assets/141774447/2b6a99b5-334d-4c71-88b8-31eb5a853ad4)

感觉不是很想做这道题，没有什么特别难或者漂亮的思路、算法。就是按上面的原则一圈一圈的遍历。遍历n//2圈（这个需要想到才行）

`拓展： ` **#54**， **#剑指offer29** (https://leetcode.cn/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

先贴个黏贴过来的代码。周末或者有时间再自己尝试写：

```python

class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        nums = [[0] * n for _ in range(n)]
        startx, starty = 0, 0               # 起始点
        loop, mid = n // 2, n // 2          # 迭代次数、n为奇数时，矩阵的中心点
        count = 1                           # 计数

        for offset in range(1, loop + 1) :      # 每循环一层偏移量加1，偏移量从1开始
            for i in range(starty, n - offset) :    # 从左至右，左闭右开
                nums[startx][i] = count
                count += 1
            for i in range(startx, n - offset) :    # 从上至下
                nums[i][n - offset] = count
                count += 1
            for i in range(n - offset, starty, -1) : # 从右至左
                nums[n - offset][i] = count
                count += 1
            for i in range(n - offset, startx, -1) : # 从下至上
                nums[i][starty] = count
                count += 1                
            startx += 1         # 更新起始点
            starty += 1

        if n % 2 != 0 :			# n为奇数时，填充中心点
            nums[mid][mid] = count 
        return nums


```




