---
title: 'Summary of treeset and treemap application'
date: 2020-09-03
permalink: /posts/2020/09/leetcode-treeset/
tags:
  - Leetcode
  - Algorithms
  - code interview
  - TreeSet
  - TreeMap
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

[Data Stream as Disjoint Intervals](https://leetcode.com/problems/data-stream-as-disjoint-intervals/)
```java
class SummaryRanges {

    //initialize the data structure
    TreeMap<Integer, Integer> treeMap;
    public SummaryRanges() {
        treeMap = new TreeMap<>();
    }

    public void addNum(int val) {
        //find the next bigger and previous smaller keys int the treemap
        Integer lower = treeMap.lowerKey(val), higher = treeMap.higherKey(val);
        
        //val lies in the interval, just skip this val
        if(treeMap.containsKey(val) || (lower != null && val >= lower && val <= treeMap.get(lower))) return;
        
        //val could bridge the gap between `lower` and `treeMap.get(higher)`
        else if(lower != null && higher != null && val == treeMap.get(lower) + 1 && val == higher - 1) {
            treeMap.put(lower, treeMap.get(higher));
            treeMap.remove(higher);
        } 
        
        //val could be the right side of `lower`
        else if (lower != null && val == treeMap.get(lower) + 1) {
            treeMap.put(lower, val);
        }
        
        //val could be the left side of `treeMap.get(higher)`
        else if (higher != null && val == higher - 1){
            treeMap.put(val, treeMap.get(higher));
            treeMap.remove(higher);
        } 
        
        //[val,val] is a independent interval
        else {
            treeMap.put(val, val);
        }
    }

    public int[][] getIntervals() {
        int[][] result = new int[treeMap.size()][2];
        int index = 0;
        for (int key : treeMap.keySet()) {
            result[index][0] = key;
            result[index][1] = treeMap.get(key);
            index++;
        }

        return result;
    }
}
```

[Max Sum of Rectangle No Larger Than K](https://leetcode.com/problems/max-sum-of-rectangle-no-larger-than-k/)
```java
public int maxSumSubmatrix(int[][] matrix, int k) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0)
        return 0;
    int rows = matrix.length, cols = matrix[0].length;
    int[][] areas = new int[rows][cols];
    for (int r = 0; r < rows; r++) {
        for (int c = 0; c < cols; c++) {
            int area = matrix[r][c];
            if (r-1 >= 0)
                area += areas[r-1][c];
            if (c-1 >= 0)
                area += areas[r][c-1];
            if (r-1 >= 0 && c-1 >= 0)
                area -= areas[r-1][c-1];
            areas[r][c] = area;
        }
    }
    int max = Integer.MIN_VALUE;
    
    //brute force
//    for (int r1 = 0; r1 < rows; r1++) {
//        for (int c1 = 0; c1 < cols; c1++) {
//            for (int r2 = r1; r2 < rows; r2++) {
//                for (int c2 = c1; c2 < cols; c2++) {
//                    int area = areas[r2][c2];
//                    if (r1-1 >= 0)
//                        area -= areas[r1-1][c2];
//                    if (c1-1 >= 0)
//                        area -= areas[r2][c1-1];
//                    if (r1-1 >= 0 && c1 -1 >= 0)
//                        area += areas[r1-1][c1-1];
//                    if (area <= k)
//                        max = Math.max(max, area);
//                }
//            }
//        }
//    }
    
    //tree set & binary search
    for (int r1 = 0; r1 < rows; r1++) {
        for (int r2 = r1; r2 < rows; r2++) {
            TreeSet<Integer> tree = new TreeSet<>();
            tree.add(0);    // padding
            for (int c = 0; c < cols; c++) {
                int area = areas[r2][c];
                if (r1-1 >= 0)
                    area -= areas[r1-1][c];
                Integer ceiling = tree.ceiling(area - k);
                if (ceiling != null)
                    max = Math.max(max, area - ceiling);
                tree.add(area);
            }
        }
    }
    return max;
}
```