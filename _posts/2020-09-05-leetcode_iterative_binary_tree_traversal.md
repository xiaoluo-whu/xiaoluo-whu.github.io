---
title: 'Summary of iterative binary tree traversal'
date: 2020-09-05
permalink: /posts/2020/09/leetcode-iterative-binary-tree-traversal/
tags:
  - LeetCode
  - algorithms
  - code interview
  - binary tree  
  - stack  
---

For binary tree traversal, recursive solution is trivial. Conversely, iterative traversal is more challenging.
Here we summarize iterative solutions of several binary tree traversal orders. Usually, a stack is indispensable in each solution.

<!--more-->

## 1.Binary Tree Inorder Traversal 
```java
public List<Integer> inorderTraversal(TreeNode root) {
    Stack<TreeNode> stack = new Stack<TreeNode>();
    List<Integer> result = new ArrayList();
    if(root == null) return result;
    
    stack.push(root);
    while(root.left != null) {
        stack.push(root.left);
        root = root.left;
    }
    
    while(!stack.isEmpty()) {
        TreeNode temp = stack.pop();
        result.add(temp.val);
        if(temp.right != null) {
            stack.push(temp.right);
            TreeNode tmp = temp.right;
            while(tmp.left != null) {
                stack.push(tmp.left);
                tmp = tmp.left;
            }
        }
    }
    
    return result;
}
```
## 2.Binary Tree Preorder Traversal
```java
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList();
    if(root == null) return result;
    
    Stack<TreeNode> stack = new Stack();
    stack.push(root);
    
    while(!stack.isEmpty()) {
        TreeNode current = stack.pop();
        result.add(current.val);
        if(current.right != null) stack.push(current.right);
        if(current.left != null) stack.push(current.left);
    }
    
    return result;
}
```

## 3.Binary Tree Postorder Traversal
```java
public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList();
    if(root == null) return result;
    
    Stack<TreeNode> stack = new Stack<TreeNode>();
    
    stack.push(root);
    while(!stack.isEmpty()) {
        TreeNode current = stack.pop();
        result.add(0, current.val);
        if(current.left != null) stack.push(current.left);
        if(current.right != null) stack.push(current.right);
        
    }
    
    return result;
}
```

## 4.Binary Tree Level Order Traversal
Level Order Traversal is actually the breath first search of the binary tree. We use a queue(FIFO, first in first out) to store every level of nodes.
```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    Queue<List<TreeNode>> queue = new ArrayDeque<>();
    List<TreeNode> list = new ArrayList<>();
    if(root == null) return result;
    list.add(root);
    queue.offer(list);

    while (!queue.isEmpty()) {
        List<TreeNode> nodeList = queue.poll();
        List<Integer> res = new ArrayList<>();
        list = new ArrayList<>();
        for (TreeNode treeNode : nodeList) {
            res.add(treeNode.val);
            if (treeNode.left != null)
                list.add(treeNode.left);
            if (treeNode.right != null)
                list.add(treeNode.right);
        }

        if (list != null && list.size() > 0)
            queue.offer(list);
        if (res != null && res.size() > 0)
        result.add(res);
    }

    return result;
}
```