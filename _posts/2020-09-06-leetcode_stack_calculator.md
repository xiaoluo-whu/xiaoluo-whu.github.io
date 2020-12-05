---
title: 'Summary of calculator questions'
date: 2020-09-06
permalink: /posts/2020/09/leetcode-stack-calculator/
tags:
  - LeetCode
  - algorithms
  - code interview
  - stack
---
Here we summarize questions involved with calculator design. Stack is frequently used in this kind of questions.

<!--more-->

## 1. [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)
```java
public int evalRPN(String[] tokens) {
    Stack<Integer> stack = new Stack<Integer>();
    
    int result = 0;
    for(int i = 0; i < tokens.length; i++) {
        if(tokens[i].equals("+") || tokens[i].equals("-") || tokens[i].equals("*") || tokens[i].equals("/")) {
            int b = stack.pop();
            int a = stack.pop();
            if(tokens[i].equals("+")) {
                stack.push(a + b);
            }
            
            if(tokens[i].equals("-")) {
                stack.push(a - b);
            }
            
            if(tokens[i].equals("*")) {
                stack.push(a * b);
            }
            
            if(tokens[i].equals("/")) {
                stack.push(a / b);
            }
        }
        else {
            stack.push(Integer.parseInt(tokens[i]));
        }
    }
    
    return stack.pop();
}
```

## 2. [Basic Calculator](https://leetcode.com/problems/basic-calculator/)
```java
public int calculate(String s) {
    Stack<Integer> stack = new Stack<Integer>();
    int result = 0, num = 0;
    char sign = '+';
    
    for(int i = 0; i < s.length(); i++) {
        if(Character.isDigit(s.charAt(i))) {
            num = num * 10 + (s.charAt(i) - '0');
            if(i + 1 >= s.length() || !Character.isDigit(s.charAt(i + 1))) {
                if(sign == '+')
                    result += num;
                else
                    result -= num;
                num = 0;
            }
        }

        if(s.charAt(i) == ' ') continue;
        if(s.charAt(i) == '+') {
            sign = '+';
        }
        if(s.charAt(i) == '-') {
            sign = '-';
        }
        if(s.charAt(i) == '(') {
            stack.push(result);
            result = 0;
            if(sign == '+')
                stack.push(1);
            else stack.push(-1);
            sign = '+';
        }
        if (s.charAt(i) == ')') {
            result *= stack.pop();
            result += stack.pop();
        }
    }

    return result;
}
```

## 3. [Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)
```java
public int calculate(String s) {
    Stack<Integer> stack = new Stack<Integer>();
    int num = 0;
    char sign = '+';
    s += "+";
    for(int i = 0; i < s.length(); i++) {
        if(s.charAt(i) >= '0' && s.charAt(i) <= '9') {
            num = num * 10 + s.charAt(i) - '0';
            continue;
        } 
        if(s.charAt(i) == ' ') {
            continue;
        }
        if(sign == '+') {
            stack.push(num);
        }
        if(sign == '-') {                    
            stack.push(-num);
        }
        if(sign == '*') {
            stack.push(stack.pop() * num);
        }
        if(sign == '/') {
            stack.push(stack.pop() / num);
        }
        sign = s.charAt(i);
        num = 0;    
    }
    
    int result = 0;
    for(int i : stack) {
        result += i;
    }
    
    return result;
}
```