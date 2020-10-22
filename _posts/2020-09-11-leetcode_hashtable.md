---
title: 'Summary of hash table'
date: 2020-09-11
permalink: /posts/2020/09/leetcode-hash-table/
tags:
  - Leetcode
  - Algorithms
  - code interview
  - hash table
---

to be updated
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