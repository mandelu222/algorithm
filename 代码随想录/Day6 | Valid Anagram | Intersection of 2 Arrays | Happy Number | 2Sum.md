## #242. Valid Anagram
(https://leetcode.com/problems/valid-anagram/)

`题目`

Given two strings s and t, return true if t is an anagram of s, and false otherwise.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

`Constraints:`

1 <= s.length, t.length <= 5 * 104; 

s and t consist of lowercase English letters.

`思路`

用dict和list都可以，但是用list更巧妙且节省空间。

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        
        collection = {}
        for letter in s:
            if letter not in collection:
                collection[letter] = 1
            else:
                collection[letter] += 1

        for char in t:
            if char not in collection:
                return False
            else:
                collection[char] -= 1
                # 本来想最后iterate一遍dict的，然后灵机一动直接扣成0了就删掉
                if collection[char] == 0:
                    del collection[char]
            
        if not collection:
            return True
```
改用list的方式写。因为只有26个字母。不用使用dict。

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        
        # only lowercase English letters
        collection = [ 0 for _ in range(26)]

        for letter in s:
            collection[ord(letter) - ord('a')] += 1

        for letter in t:
            collection[ord(letter) - ord('a')] -= 1

        for i in collection: 
            if i != 0:
                return False
        
        return True
```


## #349. Intersection of Two Arrays
(https://leetcode.com/problems/intersection-of-two-arrays/)

`题目`

Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must be unique and you may return the result in any order.

`Constraints:`

1 <= nums1.length, nums2.length <= 1000

0 <= nums1[i], nums2[i] <= 1000


先自己随便写一个。速度很慢，原因大概是O(n^2)。

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        
        intersection = set()
        for i in nums1:
            if i in nums2:     # 这个有问题，in list 操作是O(n)
                intersection.add(i)

        return intersection
```


上面那个解法的速度挺慢的，改用dict存第一个数组，然后遍历第二个数组看是否出现过。速度快多了。

```python

class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        
        collection = {}
        for num in nums1:
            # if num in collection:
            #     collection[num] += 1
            # else: 
            #     collection[num] = 0
            collection[num] = collection.get(num, 0) + 1

        result = set()
        
        for num in nums2:
            if num in collection:
                result.add(num)

        return list(result)
```
因为题目写了只有1000个元素，所以完全可以使用list。重要的是如下所有的操作都是只做index查询，是O(1)的操作。所以整个还是O(n)。

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        
        count1 = [0]*1001
        count2 = [0]*1001
        result = []

        for i in range(len(nums1)):
            count1[nums1[i]] += 1

        for j in range(len(nums2)):
            count2[nums2[j]] += 1

        for k in range(1001):
            if count1[k] * count2[k] > 0:
                result.append(k)
                
        return result
```

然后看到一个更秀的做法。竟然也很快，空间也用的非常少！

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        
        return list(set(nums1) & set(nums2)) 
```

## #202. Happy Number
(https://leetcode.com/problems/happy-number/)

`题目`

Write an algorithm to determine if a number n is happy.

A happy number is a number defined by the following process:

Starting with any positive integer, replace the number by the sum of the squares of its digits.
Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
Those numbers for which this process ends in 1 are happy.
Return true if n is a happy number, and false if not.

`思路`

题目中说了会 无限循环，那么也就是说求和的过程中，sum会重复出现，这对解题很重要！


以下是在LC上看了个解答后自己写的：compute_sum的函数现在看来有点笨。

```python
class Solution:
    def isHappy(self, n: int) -> bool:

        check = set()

        # 以下是我自己写的

        # while n:
        #     sum = self.compute_sum(n)
        #     n = sum
        #     if n == 1:
        #         return True

        #     if n not in check:
        #         check.add(n)
        #     else:
        #         break

        # return False

        # 以下是看了网站解法后重新写的

        while True:                  # 无脑循环
            n = self.compute_sum(n)  # 直接前后都是n就可以
            if n == 1:
                return True

            if n in check:
                return False

            check.add(n)


    def compute_sum(self, number):
        
        sum = 0
        while number > 0:
            digit = number % 10
            sum += digit ** 2 
            number = number // 10
       
        return sum
```
在随想录上看到的更炫的解法。我觉得以后多位数的各位的操作可以用这个：

```python

class Solution:
   def isHappy(self, n: int) -> bool:

       seen = set()

       while n != 1:

           n = sum(int(i) ** 2 for i in str(n))   # 就是这里

           if n in seen:
               return False

           seen.add(n)

       return True

```




## #1. Two Sum
(https://leetcode.com/problems/two-sum/)

`题目`

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.


`笔记`

第一次写的时候把两个if写反了。结果就fail了[3, 2, 4] target = 6的情况，因为先存了个3进dict，然后就马上找3。然后就返回了[0, 0]。应该先找，没有再存。这个样才能保证没有这种 “我自己配上我自己” 的情况。

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:

        record = {}
        
        for i in range(len(nums)):
            
            if target - nums[i] in record:
                return [i, record[target - nums[i]]]

            record[nums[i]] = i
            
        return []
```

同一个做法改写一下，用了enumerate，看上去更清晰：

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        
        records = dict()

        for index, value in enumerate(nums):  

            if target - value in records:   
                return [records[target- value], index]

            records[value] = index   

        return []
```


### 补充题目

## #49. Group Anagrams 
(https://leetcode.cn/problems/group-anagrams/)

## #438. Find All Anagrams in a String
(https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

## #350. Intersection of Two Arrays II
(https://leetcode.cn/problems/intersection-of-two-arrays-ii/)
