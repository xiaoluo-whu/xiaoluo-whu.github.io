---
title: 'Some tricky questions can be solved by using stack'
date: 2020-09-13
permalink: /posts/2020/09/leetcode-stack-tricky/
tags:
  - Leetcode
  - Algorithms
  - code interview
  - stack
---

to be updated

<!--more-->

[Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)
```java
public int[] dailyTemperatures(int[] T) {
     int N = T.length, j;
     int[] result = new int[N];
     Stack<Integer> stack = new Stack<>();
     for (int i = 0; i < N; i++) {
        while (!stack.isEmpty() && T[i] > T[stack.peek()]) {
            int index = stack.pop();
            result[index] += (i - index);
        }
        stack.push(i);
     }

     return result;
 }
```

[Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)
```java
public String removeDuplicateLetters(String s) {
    if(s == null || "".equals(s)) return "";
    int[] alphabet = new int[26];
    Stack<Character> stack = new Stack<Character>();
    boolean[] visited = new boolean[26];
    
    for(int i = 0; i < s.length(); i++) {
        alphabet[s.charAt(i) - 'a']++;
    }
    
    for(int i = 0; i < s.length(); i++) {
        alphabet[s.charAt(i) - 'a']--;
        if(visited[s.charAt(i) - 'a']) continue;
        
        while(!stack.isEmpty() && stack.peek() > s.charAt(i) && alphabet[stack.peek() - 'a'] > 0)
            visited[stack.pop() - 'a'] = false;
        
        visited[s.charAt(i) - 'a'] = true;
        stack.push(s.charAt(i));
    }
    
    StringBuilder sb = new StringBuilder();
    for(char c : stack) sb.append(c);
    
    return sb.toString();
}
```