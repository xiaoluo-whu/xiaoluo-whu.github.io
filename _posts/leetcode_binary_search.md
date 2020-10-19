---
title: 'Summary of binary search'
date: 2020-09-10
permalink: /posts/2020/09/leetcode-binary-search/
tags:
  - Leetcode
  - Algorithms
  - code interview
  - binary search
---

to be updated
[Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
```java
public int search(int[] nums, int target) {
    int low = 0, high = nums.length - 1;

    while(low < high) {
        int mid = low + (high - low) / 2;
        if(nums[mid] == target) return mid;
        if(nums[mid] < nums[high]) {
            if(nums[mid] < target && target <= nums[high]) low = mid + 1;
            else high = mid - 1;
        } else {
            //nums[mid] > nums[high]
            if(nums[mid] > target && nums[low] <= target) high = mid - 1;
            else low = mid + 1;
        } 
        
    }
    
    return low <= nums.length - 1 && nums[low] == target ? low : -1;
}
```

[Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)
```java
public boolean search(int[] nums, int target) {
    int low = 0, high = nums.length - 1;
    
    while(low < high) {
        int mid = low + (high - low) / 2;
        if(nums[mid] == target) return true;
        if(nums[mid] < nums[high]) {
            if(nums[mid] < target && target <= nums[high]) low = mid + 1;
            else high = mid - 1;
        } else if(nums[mid] > nums[high]) {
            if(nums[mid] > target && nums[low] <= target) high = mid - 1;
            else low = mid + 1;
        } else {
            if(nums[mid] > nums[low]) high = mid - 1;
            else if(nums[mid] < nums[low]) high = mid - 1;
            else {
                high--;
            }
        }
        
    }
    
    return low <= nums.length - 1 && nums[low] == target ? true : false;
}
```