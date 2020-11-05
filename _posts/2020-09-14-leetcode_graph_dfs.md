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

<!--more-->

> ![](https://xiaoluo-whu.github.io/files/images/graph_concepts.png)

Above left is a undirected graph. Basically, a graph consists of **vertices** and **edges**. Every **edge** has two **vertices**, one **vertex** on each of two ends.

A group of connected vertices is called **connected components**.

For undirected graph, the number of edges connecting to a specific vertex is the **degree** of this vertex.

> ![](https://xiaoluo-whu.github.io/files/images/directed_graph.jpg)

For directed graph, the number of edges starting from a specific vertex is the **out degree** of this vertex. 
The number of edges ending with it is called **in degree**.

For both undirected and directed graph, an internal loop is called a **cycle**.

Then, let's talk about depths-first-search(DFS).

> ![](https://xiaoluo-whu.github.io/files/images/graph_dfs.png)

The DFS sequence of vertices of above graph is 

> 0 -> 1 -> 2 -> 6 -> 4 -> 5 -> 3

Then, we will try to implement DFS algorithm.

> To visit a vertex v :
> * Mark vertex v as visited.
> * Recursively visit all unmarked vertices adjacent to v.

```java
private void dfs(Graph G, int v)
{
    marked[v] = true;
    for (int w : G.adj(v))
        if (!marked[w])
        {
            dfs(G, w);
            edgeTo[w] = v;
        }
}
```

Next, let's take a look at several LeetCode questions about DFS.
[Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)
```java
public void solve(char[][] board) {
    if (board == null || board.length == 0 || board[0].length == 0) return;

    //a little trick here: start from every 'O' on four edges, do DFS, mark every connected 'O' as '*'
    int M = board.length, N = board[0].length;
    for (int i = 0; i < M; i++) {
        boundryDFS(board, i, 0);
        boundryDFS(board, i, N - 1);
    }
    
    for (int j = 0; j < N; j++) {
        boundryDFS(board, 0, j);
        boundryDFS(board, M - 1, j);
    }
    
    //all the 'O's left are surrounded regions, which can be flipped to 'X'
    //don't forget to change '*' back to 'O' 
    for (int i = 0; i < M; i++) {
        for (int j = 0; j < N; j++) {
            if (board[i][j] == 'O') board[i][j] = 'X';
            if (board[i][j] == '*') board[i][j] = 'O';
        }
    }
}

private void boundryDFS(char[][] board, int m, int n) {
    int M = board.length, N = board[0].length; 
    if (board[m][n] != 'O') return;
    
    board[m][n] = '*';
    if (m > 0) boundryDFS(board, m - 1, n);
    if (m < M - 1) boundryDFS(board, m + 1, n);
    if (n > 0) boundryDFS(board, m, n - 1);
    if (n < N - 1) boundryDFS(board, m, n + 1);
}
```

[Number of Islands](https://leetcode.com/problems/number-of-islands/)
```java
public int numIslands(char[][] grid) {
    if(grid == null || grid.length == 0 || grid[0].length == 0) return 0;
    int result = 0, M = grid.length, N = grid[0].length;
    boolean[][] visited = new boolean[M][N];
    
    for(int i = 0; i < M; i++) {
        for(int j = 0; j < N; j++) {
            if(grid[i][j] == '0') continue;
            if(dfs(i, j, grid, visited, M, N)) result++;
        }
    }
    
    return result;
}

private boolean dfs(int i , int j, char[][] grid, boolean[][] visited, int M, int N) {
    if(visited[i][j]) return false;
    
    visited[i][j] = true;
    if(i > 0 && grid[i - 1][j] == '1') dfs(i - 1, j, grid, visited, M, N);
    if(i < M - 1 && grid[i + 1][j] == '1') dfs(i + 1, j, grid, visited, M, N);
    if(j > 0 && grid[i][j - 1] == '1') dfs(i, j - 1, grid, visited, M, N);
    if(j < N - 1 && grid[i][j + 1] == '1') dfs(i, j + 1, grid, visited, M, N);
    
    return true;
}
```