---
title: 'Summary of multiple pointers application'
date: 2020-09-30
permalink: /posts/2020/09/leetcode-multiple-pointers/
tags:
  - Leetcode
  - Algorithms
  - code interview
  - multiple pointers
---

to be updated

<!--more-->

[Sort Colors](https://leetcode.com/problems/sort-colors/)
```java
public void sortColors(int[] nums) {
    int count_0 = 0, count_1 = 0, count_2 = 0;
    
    for (int i = 0; i < nums.length; i++) {
        if(nums[i] == 0) {
            nums[count_2] = 2;
            count_2++;
            nums[count_1] = 1;
            count_1++;
            nums[count_0] = 0;
            count_0++;
        } else if(nums[i] == 1) {
            nums[count_2] = 2;
            count_2++;
            nums[count_1] = 1;
            count_1++;
        } else {
            nums[count_2] = 2;
            count_2++;
        }
    }
}
```

[Ugly Number II](https://leetcode.com/problems/ugly-number-ii/)
```java
public int nthUglyNumber(int n) {
    int[] ugly = new int[n];
    ugly[0] = 1;
    int index2 = 0, index3 = 0, index5 = 0;
    int factor2 = 2, factor3 = 3, factor5 = 5;
    for(int i=1;i<n;i++){
        int min = Math.min(Math.min(factor2,factor3),factor5);
        ugly[i] = min;
        if(factor2 == min)
            factor2 = 2*ugly[++index2];
        if(factor3 == min)
            factor3 = 3*ugly[++index3];
        if(factor5 == min)
            factor5 = 5*ugly[++index5];
    }
    return ugly[n-1];
}
```