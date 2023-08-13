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
## #35. Search Insert Position
(https://leetcode.com/problems/search-insert-position/description/)

`题目：`
和上面一样的题目，唯一的不同是如果找不到，返回suppose应该插入的index。

`关注点：`和上面的题目代码基本就差最后的如果找不到的返回值。**我觉得就记得返回left就可以了**。

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:

        left = 0
        right = len(nums) - 1
        
        # interestingly even these edge cases can be handled by the logic below which returns left
        # if target < nums[left]:
        #     return 0

        # if target > nums[right]:
        #     return len(nums)

        while left <= right: 
            mid = left + (right - left) // 2
            if nums[mid] == target: 
                return mid
            elif nums[mid] > target:
                right = mid - 1
            else:
                left = mid + 1

        return left
```
## #34. Find First and Last Position of Element in Sorted Array
(https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

`题目：`
Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value.

If target is not found in the array, return [-1, -1]. You must write an algorithm with O(log n) runtime complexity.

`思路：`如果忘记了可以去看一下下面的youtube。这个从理解上比其他地方的解法更加的直观一点。

```python
# explanations @ https://www.youtube.com/watch?v=4sQL7R5ySUU

class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        left_index = self.getBoundary(nums, target, True)
        right_index = self.getBoundary(nums, target, False)

        return [left_index, right_index]
    

    # add isleft Boolean to decide if we are to find left-most boundary or right
    def getBoundary(self, nums: List[int], target: int, isleft: bool):

        left, right = 0, len(nums) - 1
        index = -1    # if index is not updated below, return -1 as required

        while left <= right:
            mid = left + (right - left) // 2
            if nums[mid]  > target:
                right = mid - 1
            elif nums[mid]  < target:
                left = mid + 1
            else:
                # Ordinary BS will return mid; however here we will assign mid value to index, and keep looking from here
                index = mid
                # if we are to work on left-most bondary, we try to move the right towards left, and get into loop again. 
                # if we get another mid that equals to target, we update index above; otherwise keep the previous index value;
                # Until the loop condition met; 
                if isleft:
                    right = mid - 1
                # To look for right-most boundary, we move left towards right
                else:
                    left = mid + 1
        
        return index 
```

## #27 Remove Element 
(https://leetcode.com/problems/remove-element/)

`题目：`
_Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val._

_Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:_

_Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.
Return k._

`思路：` 移除的题目就想到pop，因为反正留着也没用。但重要的是如何一边pop还能一边知道自己在哪里，然后遍历到arry的最后一位。这个不算滑动窗口，但直觉需要两个pointer，一个track当前位置，一个指向最后一位，告诉当前位置可以停止了。虽然理论上不停地算len(nums)也可以，但是不停调用len速度就没那么快了。但问题是pop(n)是O(n), 加上loop就是n^2的time complexity. 

然后视频给的是另外一种快慢双指针的写法，只用loop一边。O(n), 但不知道为什么lc提交反而第一种方法比较快(32ms vs. 41ms)。。。以下是两种做法的code


`关注点：`第二种方法必须画图才能行。建议看视频https://www.bilibili.com/video/BV12A4y1Z7LP/?vd_source=5d163d78c46b415677c71a21dcf4977b
简而言之就是：fast总是往前移，但只有fast指向的数和val不一样时，才把fast的数给到slow, 然后slow前移。这样就把所有不一样的值copy到slow指向的一个个位置里。
感觉这也是题目为什么强调`The remaining elements of nums are not important as well as the size of nums`

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:

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

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:

        slow = 0
        length = len(nums)

        for fast in range(length):
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
        
        return slow
```
