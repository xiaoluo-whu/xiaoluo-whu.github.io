---
title: 'Summary of object deep copy questions'
date: 2020-09-12
permalink: /posts/2020/09/leetcode-deep-copy/
tags:
  - Leetcode
  - Algorithms
  - code interview
  - hash table
  - deep copy
---

In some questions, you are required to return a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) of the original complicated object(like a graph, a linked list and so on).
Of course, you can not return the original object directly, or it would not be called "deep". 
In short, you have to recreate an object which is exactly identical to the original one and they must have the same values and internal relationships. 
And a whole new bunch of space must be allocated to the recreated object.

> ![](https://xiaoluo-whu.github.io/files/images/Deep_copy_in_progress.svg)

To tackle suck kind of questions, HashMap plays an important role. We can use HashMap to keep track of the relationship of the original elements and their counterparts in the to-be-deep-copy object in the first loop.
Then when in the second loop, we can recreate the original object very easily.

Please note that HashMap can take not only basic data types(like String, Integer, Long) as keys and values, but also complicated ones(like List, Set and Map).

Here I list serveral Leetcode questions about deep copy. 
[Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)
```java
public Node copyRandomList(Node head) {
      if (head == null) return null;

      Map<Node, Node> map = new HashMap<Node, Node>();

      // loop 1. copy all the nodes, map the original elements to their counterparts in the to-be-deep-copy object
      Node node = head;
      while (node != null) {
        map.put(node, new Node(node.val));
        node = node.next;
      }

      // loop 2. assign next and random pointers, recover(recreate) the original object
      node = head;
      while (node != null) {
        map.get(node).next = map.get(node.next);
        map.get(node).random = map.get(node.random);
        node = node.next;
      }

      return map.get(head);
}
```