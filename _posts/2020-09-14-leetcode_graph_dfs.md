---
title: 'Summary of depth first search application'
date: 2020-09-14
permalink: /posts/2020/09/leetcode-graph-dfs/
tags:
  - Leetcode
  - Algorithms
  - code interview
  - graph
  - depth first search
---

Before we go into the discussion of depth first dearch(DFS), let's briefly introduce several basic concepts of graph.

> ![](https://xiaoluo-whu.github.io/files/images/graph_concepts.png) ![](https://xiaoluo-whu.github.io/files/images/directed_graph.jpg)

Above left is a undirected graph. Basically, a graph consists of **vertices** and **edges**. Every **edge** has two **vertices**, one **vertex** on each of two ends.

A group of connected vertices is called **connected components**.

For undirected graph, the number of edges connecting to a specific vertex is the **degree** of this vertex.

For directed graph, the number of edges starting from a specific vertex is the **out degree** of this vertex. 
The number of edges ending with it is called **in degree**.

For both undirected and directed graph, an internal loop is called a **cycle**.