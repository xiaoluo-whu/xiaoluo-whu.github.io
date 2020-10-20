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

[Pow(x, n)](https://leetcode.com/problems/powx-n/)
```java
double myPow(double x, int n) { 
    if(n == 0) return 1;
    long num = 0L;
    if(n < 0) {
        num = -(long) n;
        x = 1 / x;
    } else {
        num = (long) n;
    }
    
    double ans = 1;
    while(num > 0){
        if((num & 1) == 1) ans *= x;
        x *= x;
        num >>= 1;
    }
    
    return ans;
}
```

[Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/) & [Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)
```java
public boolean searchMatrix(int[][] matrix, int target) {
    if(matrix == null || matrix.length == 0 || matrix[0].length == 0) return false;
    
    int M = matrix.length, N = matrix[0].length, i = 0, j = N - 1;
    
    while(i < M && j >= 0) {
        if(matrix[i][j] == target) return true;
        else if(matrix[i][j] < target) i++;
        else j--;
    }
    
    return false;
}
```

[Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
```java
public int findMin(int[] nums) {
    int low = 0, high = nums.length - 1;
    
    while(low < high) {
        int mid = (low + high) / 2;
        if(nums[mid] > nums[high]) low = mid + 1;
        else high = mid;
    }
    
    return nums[low];
}
```

[Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)
```java
public int findMin(int[] nums) {
    int low = 0, high = nums.length - 1;
    
    while(low < high) {
        int mid = (low + high) / 2;
        if(nums[mid] < nums[high]) high = mid;
        else if(nums[mid] > nums[high]) low = mid + 1;
        else {
            if(nums[mid] > nums[low]) {
                return nums[low];
            } else if(nums[mid] < nums[low]) {
                high = mid;
            } else {
                while(low < high && nums[low] == nums[mid]) low++;
                while(low < high && nums[high] == nums[mid]) high--;
                return Math.min(Math.min(nums[low], nums[high]), nums[mid]);
            }
        }
    }
    
    return nums[low];
}
```