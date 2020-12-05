---
title: 'Summary of bucket sort application'
date: 2020-09-08
permalink: /posts/2020/09/leetcode-bucket-sort/
tags:
  - LeetCode
  - Algorithms
  - code interview
  - bucket sort
---

to be updated

<!--more-->

[Maximum Gap](https://leetcode.com/problems/maximum-gap/)

[Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/solution/)
```java
public int[] topKFrequent(int[] nums, int k) {
    int N = nums.length;
    int[] result = new int[k];

    List<Integer>[] bucket = new ArrayList[N + 1];
    HashMap<Integer, Integer> frequencyMap = new HashMap<>();
    for (int i = 0; i <= N; i++) {
        bucket[i] = new ArrayList();
    }

    for (int i = 0; i < N; i++) {
        frequencyMap.putIfAbsent(nums[i], 0);
        frequencyMap.put(nums[i], frequencyMap.get(nums[i]) + 1);
    }

    for (int i : frequencyMap.keySet()) {
        bucket[frequencyMap.get(i)].add(i);
    }

    int index = k - 1;
    for (int i = N; i >= 0 && index >= 0; i--) {
        if (bucket[i] == null || bucket[i].size() == 0) continue;
        for (int j = 0; j < bucket[i].size() && index >= 0; j++) {
            result[index--] = bucket[i].get(j);
        }
    }

    return result;
}
```

[Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)
```java
public String frequencySort(String s) {
    HashSet<Character>[] bucket = new HashSet[s.length() + 1];
    for(int i = 0; i < bucket.length; i++) bucket[i] = new HashSet<Character>();
    
    int[] table = new int[128];
    for(int i = 0; i < s.length(); i++) table[s.charAt(i)]++;
    
    for(int i = 0; i < table.length; i++) bucket[table[i]].add((char) i);
    
    StringBuilder sb = new StringBuilder();
    for(int i = bucket.length - 1; i >= 1; i--) {
        for(char c : bucket[i]) 
            for(int k = 0; k < i; k++)
                sb.append(c);
    }
    
    return sb.toString();
     
}
```