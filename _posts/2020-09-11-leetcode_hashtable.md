---
title: 'Summary of hash table'
date: 2020-09-11
permalink: /posts/2020/09/leetcode-hash-table/
tags:
  - LeetCode
  - Algorithms
  - code interview
  - hash table
---

to be updated

<!--more-->

[Two Sum](https://leetcode.com/problems/two-sum/)
```java
public int[] twoSum(int[] nums, int target) {
    HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
    for(int i = 0; i < nums.length; i++) {
        if(map.containsKey(target - nums[i])) return new int[] {map.get(target - nums[i]), i};
        map.putIfAbsent(nums[i], i);
        
    }
    
    return null;
}
```

[Group Anagrams](https://leetcode.com/problems/group-anagrams/)
```java
public List<List<String>> groupAnagrams(String[] strs) {
    List<List<String>> result = new ArrayList();
    HashMap<String, List<Integer>> map = new HashMap<String, List<Integer>>();

    for(int i = 0; i < strs.length; i++) {
        char[] c = strs[i].toCharArray();
        Arrays.sort(c);
        String s = new String(c);
        map.putIfAbsent(s, new ArrayList());
        map.get(s).add(i);
    }

    for(String s : map.keySet()) {
        List<String> res = new ArrayList();
        for(int i : map.get(s)) {
            res.add(strs[i]);
        }
        result.add(res);
    }

    return result;
}
```

[Repeated DNA Sequences](https://leetcode.com/problems/repeated-dna-sequences/)
```java
public List<String> findRepeatedDnaSequences(String s) {
    HashSet<String> seen = new HashSet<>();
    HashSet<String> result = new HashSet<>();
    
    for(int i = 0; i <= s.length() - 10; i++) {
        String sub = s.substring(i, i + 10);
        //if s has already appeared in previous iterations, 
        // the add operation would fail and it is one of the repeated DNA sequence
        if(!seen.add(sub)) {
            result.add(sub);
        }
    }
    
    return new ArrayList(result);
}
```

[Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)
```java
public List<Integer> findAnagrams(String s, String p) {
    List<Integer> result = new ArrayList<Integer>();
    if(s == null || "".equals(s) || p == null || "".equals(p)) return result;

    int M = s.length(), N = p.length();
    
    int[] table = new int[26];
    int[] slide = new int[26];
    for(int i = 0; i < N; i++) {
        table[p.charAt(i) - 'a']++;
        if(i < M) slide[s.charAt(i) - 'a']++;
    }
    for(int i = 0; i <= M - N; i++) {
        if(i > 0 && i <= M - N) {
            slide[s.charAt(i - 1) - 'a']--;
            slide[s.charAt(i + N - 1) - 'a']++;
        }
        if(Arrays.equals(table, slide)) result.add(i);
    }

    return result;
}
```

[4Sum II](https://leetcode.com/problems/4sum-ii/)
```java
public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
    int result = 0;
    HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
    for(int i = 0; i < A.length; i++) {
        for(int j = 0; j < B.length; j++) {
            map.put(A[i] + B[j], map.getOrDefault(A[i] + B[j], 0) + 1);
        } 
    }
    
    for(int i = 0; i < C.length; i++) {
        for(int  j = 0; j < D.length; j++) {
            result += map.getOrDefault(-(C[i] + D[j]), 0);
        }
    }
    
    return result;
}
```

