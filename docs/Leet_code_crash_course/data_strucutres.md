---
layout: default
title: Leetcode Data Structures
parent: Leetcode Data Structures Crash Course
nav_order: 1
---

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

# Arrays

## 1. Maximum Subarray

![](/images/53.png)

### Scrappy Solution
The solution timed out on the submission test cases
```python
class Solution:
    def __init__(self):
        self.max_sum = -10000

    def maxSubArray(self, nums: List[int]) -> int:
        sliding_window = []
        for i in nums:
            if not sliding_window:
                sliding_window.append(i)
                curr_sum = self.find_sum(sliding_window)
                self.save_sum(curr_sum)
            
            elif self.find_sum(sliding_window) < 0:
                sliding_window = []
                sliding_window.append(i)
                curr_sum = self.find_sum(sliding_window)
                self.save_sum(curr_sum)
            
            else:
                sliding_window.append(i)
                curr_sum = self.find_sum(sliding_window)
                self.save_sum(curr_sum)
        
        return self.max_sum
    
    def find_sum(self, window):
        sum = 0
        for i in window:
            sum += i
        
        return sum

    def save_sum(self, sums):
        if sums > self.max_sum:
            self.max_sum = sums
```

### Good Solution in Python

[Reference](https://www.youtube.com/watch?v=5WZl3MMT0Eg&ab_channel=NeetCode)

```python
class Solution:
    def __init__(self):
        self.max_sum = None
        self.cur_sum = 0

    def maxSubArray(self, nums: List[int]) -> int:
        self.max_sum = nums[0]
        for i in nums:
            if self.cur_sum < 0:
                self.cur_sum = 0
            
            self.cur_sum += i
            self.max_sum = max(self.max_sum, self.cur_sum)

        return self.max_sum
```

### Good Solution in C++

## 2. Contains Duplicate

![](/images/217.png)

### Good Solution in Python

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        flag = False
        hashset = set()

        for i in nums:
            if i in hashset:
                flag = True
                break
            hashset.add(i)
        
        return flag
```

### Good Solution in C++

## 3. Two Sum

[](/images/1.png)

### Solution in Python

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # we'll build the ref dict as we iterate over nums
        # after this hashmap is built, we'll then query it over time
        # ref_dict format is {value: index, value: index...}
        ref_dict = {nums[0]:0}

        # create a temp value which when added to another element gives target
        target_complement = 0
        
        for i in range(1,len(nums)):
            target_complement = nums[i]
            if (target - target_complement) in ref_dict.keys():
                return [ref_dict[(target-target_complement)], i]
            
            else:
                ref_dict[target_complement] = i
```

### Solution in C++

## 4. Merge Sorted Array

![](/images/88.png)

[Reference](https://www.youtube.com/watch?v=P1Ic85RarKY&ab_channel=NeetCode)

### Scrappy Python Solution (didn't even work)

```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        # let's find the largest value in the m array and see if there's anything larger
        # than that in our n array. If there is something larger, then let's not consider
        # that for computation
        m = m + n

        # find largest in m
        largest = nums1[0]
        for i in range(m-n):
            if nums1[i] > largest:
                largest = i
        
        # find all numbers in n array greater than largest
        for j in range(n):
            if nums2[j] > largest:
                # then save this and all following elements (as they are sorted)
                # in the below specified position
                nums1[(m - n + j):] = nums2[j:]
            
            else:
                # now if it isn't larger than all values in nums1, we'll have
                # to fit it in somewhere

                # lets make use of the fact that it's ordered and start at the
                # middle of the nums1 array (essentially do binary search)

                target_index = None
                low = 0
                high = m-n
                while low <= high:
                    mid = int((m-n)/2)

                    if nums2[j] > nums1[mid]:
                        low = mid + 1
                    elif nums2[j] < nums1[mid]:
                        high = mid - 1
                    else:
                        target_index = mid
                
                # if we haven't found it yet then just see check the low and high
                if target_index is None:
                    if nums2[j] > mid:
                        nums1.insert(nums2[j], mid)
                    else:
                        nums1.insert(nums2[j], mid-1)
```

### Good Solution in Python

