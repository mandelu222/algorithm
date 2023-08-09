## #704 Binary Search 
(https://leetcode.com/problems/binary-search/)

`题目：`
Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1.
You must write an algorithm with O(log n) runtime complexity.

`思路：`暂时觉得一看见 O(log n)就往binary上想。

`关注点：`这题已经看过一次视频，听过一次讲座，还是写不对。在=、+1和-1里纠结。 

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums)-1  
        # 一： 既然上面对于right的取值，已经可以取到[left, right]双闭双包含， 这里就应该加等号。可以想到特殊情况如只有1个number，两个index必然重合
        while left <= right:
            # 二：一开始用了"/"而不是"//", 直接报错产生floating; 然后换成int(), 最后换成答案里的//。这个两个应该都是向下取整，结果是一样的。
            mid = left + (right - left)//2
            if nums[mid] == target:
                return mid
            elif nums[mid] > target:
                # 三：此时target在左区间，双包含[left, middle-1]，因为middle已经看过了，直接看到middle左边一个数就可以了。 
                right = mid - 1 
            else:
                # 四：此时target在右区间，双包含[middle+ 1, right]，同样因为middle已经看过了，直接看到middle右边一个数就可以了。
                left = mid + 1
        # 五：第一次忘记return -1了
        return -1
```


## #27 Remove Element 
(https://leetcode.com/problems/remove-element/)

`题目：`
_Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val._

_Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:_

_Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.
Return k._

`思路：` 移除的题目就想到pop，因为反正留着也没用。但重要的是如何一边pop还能一边知道自己在哪里，然后遍历到arry的最后一位。这个不算滑动窗口，但直觉需要两个pointer，一个track当前位置，一个指向最后一位，告诉当前位置可以停止了。虽然理论上不停地算len(nums)也可以，但是不停调用len速度就没那么快了。

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        count = 0
        left = 0
        right = len(nums) -1 

        while left <= right:
            if nums[left] == val:
                right -= 1
                nums.pop(left)
            else:
                left += 1

        return len(nums)
```
