## #454. 4Sum II
(https://leetcode.com/problems/4sum-ii/)

`题目`

Given four integer arrays nums1, nums2, nums3, and nums4 all of length n, return the number of tuples (i, j, k, l) such that:

0 <= i, j, k, l < n; 
nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0

`思路`

我自己的思路是对的。当时觉得N^2的时间复杂度总好过N^4。本来想的是建立两个dict让后对照。后来发现可以使用两数和的思路。省略了对照的过程。

另外一个写的时候发现需要注意的东西是counter需要加第一个dict里找到的tuple的个数，而不是+1。

总而言之这个写的思路上比较顺利，一点小问题如下：

```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:

        result = {}
        
        for i in range(len(nums1)):
            for j in range(len(nums2)):
                # result[nums1[i] + nums2[j]] = result.get(nums1[i] + nums2[j], []).append((i, j))
                if nums1[i] + nums2[j] in result:
                    result[nums1[i] + nums2[j]].append((i, j))
                else:
                    result[nums1[i] + nums2[j]] = [(i, j)]
                    
        counter = 0

        for i in range(len(nums3)):
            for j in range(len(nums4)):
                if - (nums3[i] + nums4[j]) in result:
                    counter += len(result[- (nums3[i] + nums4[j])])

        return counter
```
感觉自己在细节方面有点傻。。。如果只是计数，完全没必要记录具体的index tuple。只用计数就可以了。

而且之前用result.get(), 如果运行result.get(0, [])，结果是None。

```python

class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:

        result = {}
        
        for i in range(len(nums1)):
            for j in range(len(nums2)):
                result[nums1[i] + nums2[j]] = result.get(nums1[i] + nums2[j], 0) + 1
        
        counter = 0

        for i in range(len(nums3)):
            for j in range(len(nums4)):
                if - (nums3[i] + nums4[j]) in result:
                    counter += result[- (nums3[i] + nums4[j])]

        return counter
```



## #383. Ransom Note
(https://leetcode.com/problems/ransom-note/)

`题目`

Given two strings ransomNote and magazine, return true if ransomNote can be constructed by using the letters from magazine and false otherwise.

Each letter in magazine can only be used once in ransomNote.

`思路`

据说只要说全部是小写字母就是浓浓的使用list暗示？

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        collection = {}
        for letter in ransomNote:
            if letter not in collection:
                collection[letter] = 1
            else:
                collection[letter] += 1
        
        for char in magazine:
            if char in collection:
                collection[char] -= 1

        for value in collection.values():
            if value > 0:
                return False

        return True
```
其实看了随想录网站上的解法，发现完全可以调整一下顺序，先把available的（在杂志里的）全部存起来，然后遍历ransomNote做-1看够不够用。

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        counts = {}
        for c in magazine:
            counts[c] = counts.get(c, 0) + 1
        for c in ransomNote:
            if c not in counts or counts[c] == 0:
                return False
            counts[c] -= 1
        return True
```

另外一个我很喜欢的，虽然很秀但是很清楚

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        return all(ransomNote.count(c) <= magazine.count(c) 
        for c in set(ransomNote))
```


## #15. 3Sum
(https://leetcode.com/problems/3sum/)

`题目`

Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.

Notice that the solution set must not contain duplicate triplets.

`思路`
1. 先排序；
2. nums[i] 要先去重；
3. 使用左右指针，左右指针都要去重

```python

class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        
        result = []
        nums.sort()    # 排序，才可以使用左右指针

        for i in range(len(nums)):

            if nums[i] > 0:
                return result

            if i > 0 and nums[i] == nums[i - 1]:  # 去重
                continue

            left = i + 1
            right = len(nums) - 1

            while left < right:

                if nums[i] + nums[left] + nums[right] > 0:
                    right -= 1
                
                elif nums[i] + nums[left] + nums[right] < 0:
                    left += 1

                else:
                    result.append([nums[i], nums[left], nums[right]])

                    # 左右指针去重，如果同样的数就继续移动
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1

                    # 去重完毕，指向新的非重复数字，对比nums[i]以及新的left, right指向的值
                    left += 1
                    right -= 1

        return result
```

## #18. 4Sum
(https://leetcode.com/problems/4sum/)

`题目`

Given an array nums of n integers, return an array of all the unique quadruplets [nums[a], nums[b], nums[c], nums[d]] such that:

0 <= a, b, c, d < n

a, b, c, and d are distinct.

nums[a] + nums[b] + nums[c] + nums[d] == target

You may return the answer in any order.

`思路`

和3sum一毛一样的思路。只是要再加一个k的循环。见comment for details。

```python
class Solution:

    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        
        result = []
        nums.sort()


        for k in range(len(nums)):
            # 只有当连个数都大于0 的时候可以返回，如果targe和nums是负数，此处剪枝可能不成立；
            
            if nums[k] > target and target > 0:
                break
            # 如果数字重复，往前一步；
            if k > 0 and nums[k] == nums[k - 1]:
                continue
            #注意此处i的起始位置；
            for i in range(k + 1, len(nums)):
                # 这里我debug了很久。因为出现了一个-2， 7，两数和大于target，所以直接返回没有继续遍历了。所以需要两个数都大于0；
                # 另外此处应该break而不是返回，break是可以出这个循环，但上一层循环还会继续；
                if nums[k] + nums[i] > target and target > 0 and nums[k] > 0 and nums[i] > 0:
                    break
                if i > k + 1 and nums[i] == nums[i - 1]:
                    continue    

                left = min(i + 1, len(nums) - 1)
                right = len(nums) - 1

                while left < right:

                    if nums[k] + nums[i] + nums[left] + nums[right] > target:
                        right -= 1
                    elif  nums[k] + nums[i] + nums[left] + nums[right] < target:
                        left += 1
                    else:
                        result.append([nums[k], nums[i], nums[left], nums[right]])

                        while left < right and nums[left] == nums[left + 1]:
                            left += 1
                        while left < right and nums[right] == nums[right - 1]:
                            right -= 1                                
                        
                        #这两步在哪一层indentation一定要想清楚。
                        left += 1
                        right -= 1
        return result
```
