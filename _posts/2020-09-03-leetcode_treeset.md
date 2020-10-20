---
title: 'Summary of treeset application'
date: 2020-09-03
permalink: /posts/2020/09/leetcode-treeset/
tags:
  - Leetcode
  - Algorithms
  - code interview
  - TreeSet
---

to be updated

[Contains Duplicate III](https://leetcode.com/problems/contains-duplicate-iii/)
```java
public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
    if (nums.length < 2 || k == 0) {
        return false;
    }
    TreeSet<Long> set = new TreeSet<Long>();
    for(int i = 0; i < nums.length; i++) {
        //remove elements whose index differences with i are bigger than k 
        if (i > k) set.remove((long) nums[i - k - 1]);
        
        //find two neighbor elements in the set
        Long floor = set.floor((long) nums[i]);
        Long ceil = set.ceiling((long) nums[i]);

        //meet requirements
        if ((floor != null && nums[i] - floor <= t) || (ceil != null && ceil - nums[i] <= t)) return true;
        set.add((long) nums[i]);
        
    }

    return false;
}
```