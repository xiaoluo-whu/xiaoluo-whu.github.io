---
title: 'A generic template for solving Union-Find questions'
date: 2020-09-02
permalink: /posts/2020/09/leetcode-union-find/
tags:
  - Leetcode
  - Algorithms
  - code interview
  - Union Find
---

In short, Union-Find is an algorithm that models a graph, connecting some pairs of vertices and separating the graph into several connected components.
There are several versions of Union-Find, like Quick-Union, Quick-Find and so on. Here we just list the most widely used version, Weighted-Quick-Union.
I suggest you memorize the following code template so that you can build a Union-Find model very quickly in a code interview.
```java
public class UF {

        //please note that you don't have to include all of the following variables and functions in your codes,
        //you can choose any of them according to the requirements of questions. 
        
        private int[] parent;  // parent[i] = parent of i
        private int[] rank;   // rank[i] = rank of subtree rooted at i (never more than 31)
        private int count;     // number of components
        private int[] sz;     //size of each component
        
        public UF(int n) {
            count = n;
            parent = new int[n];
            rank = new int[n];
            sz = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
                rank[i] = 0;
                sz[i] = 1;
            }
        }
        
        public int find(int p) {
            while (p != parent[p]) {
                parent[p] = parent[parent[p]];    // path compression by halving
                p = parent[p];
            }
            return p;
        }
        
        boolean connected(int p, int q) {
            return find(p) == find(q);
        }
        
        int top() {
            return size(0);
        }
        
        int size(int p) {
            return sz[parent[p]] - 1;
        }
        
        public int count() {
            return count;
        }
        
        public void union(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            if (rootP == rootQ) return;

            // make root of smaller rank point to root of larger rank
            if(rank[rootP] < rank[rootQ]) {
                parent[rootP] = rootQ;
                sz[rootQ] += sz[rootP];
            }
            else if (rank[rootP] > rank[rootQ]) {
                parent[rootQ] = rootP;
                sz[rootP] += sz[rootQ];
            }
            else {
                parent[rootQ] = rootP;
                rank[rootP]++;
                sz[rootP] += sz[rootQ];
            }
            count--;
        }

    }
```

Sample Union-Find questions on Leetcode
[Friend Circles](https://leetcode.com/problems/friend-circles/)
```java
class Solution {
    public class UF {
        //same code as above...
    }
    
    public int findCircleNum(int[][] M) {
        if(M == null || M.length == 0 || M[0].length == 0) return 0;
        int n = M.length;
        
        //initialize new UF object
        UF uf = new UF(n);
        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j < n; j++) {
                //connect every pair of friends
                if(M[i][j] == 1) uf.union(i, j);
            }
        }
        
        //number of components is the total number of friend circles
        return uf.count;
    }
}
```


[Bricks Falling When Hit](https://leetcode.com/problems/bricks-falling-when-hit/)
```java
class Solution {
    public class UF {
        //same code as above...
    }
    
    //get result by adding brick one by one reversely
    public int[] hitBricks(int[][] grid, int[][] hits) {
        int[] result = new int[hits.length];
        int M = grid.length;
        int N = grid[0].length;
        
        for(int t = 0; t < hits.length; t++) {
            //if the to be erased place does not have a brick, set hits to -1
            if(grid[hits[t][0]][hits[t][1]] == 0) {
                hits[t][0] = -1;
                hits[t][1] = -1;
            } else {
                //if the to be erased place has a brick, set grid to 0
                grid[hits[t][0]][hits[t][1]] = 0;
            }
        }
        
        //initialize Union-Find object, in total M * N elements. 
        // In addition, add a dummy virtual brick will save a lot of codes. 
        // The dummy brick will connect to the first row of bricks 
        UF uf = new UF(M * N + 1);
        for(int j = 0; j < N; j++) {
            if(grid[0][j] == 1) uf.union(0, j + 1);
        }
        
        //connect the brick at (i - j) to its four neighbors if there are bricks in there
        for(int i = 0; i < M; i++) {
            for (int j = 0; j < N; j++) {
                if(i > 0 && grid[i][j] == 1 && grid[i - 1][j] == 1) uf.union(i * N - N + j + 1, i * N + j + 1);
                if(i < M - 1 && grid[i][j] == 1 && grid[i + 1][j] == 1) uf.union(i * N + j + 1, i * N + N + j + 1);
                if(j > 0 && grid[i][j] == 1 && grid[i][j - 1] == 1) uf.union(i * N + j, i * N + j + 1);
                if(j < N - 1 && grid[i][j] == 1 && grid[i][j + 1] == 1) uf.union(i * N + j + 1, i * N + j + 2);
            }
        }
        
        //after all the erasures are done, the number of bricks left
        int original = uf.top();
        
        //add bricks from the last one removed to the first, calculate the difference of bricks connected to the dummy brick
        for(int t = hits.length - 1; t >= 0; t--) {
            if(hits[t][0] != -1) {
                grid[hits[t][0]][hits[t][1]] = 1;
                int i = hits[t][0];
                int j = hits[t][1];
                if(i > 0 && grid[i][j] == 1 && grid[i - 1][j] == 1) uf.union(i * N - N + j + 1, i * N + j + 1);
                if(i < M - 1 && grid[i][j] == 1 && grid[i + 1][j] == 1) uf.union(i * N + j + 1, i * N + N + j + 1);
                if(j > 0 && grid[i][j] == 1 && grid[i][j - 1] == 1) uf.union(i * N + j, i * N + j + 1);
                if(j < N - 1 && grid[i][j] == 1 && grid[i][j + 1] == 1) uf.union(i * N + j + 1, i * N + j + 2);
                if(i == 0 && grid[i][j] == 1) uf.union(0, j + 1);
                
                result[t] = (uf.top() - original - 1) > 0 ? (uf.top() - original - 1) : 0 ;
                original = uf.top();
            }
        }
        
        return result;
    }
}
```

[Smallest String With Swaps](https://leetcode.com/problems/smallest-string-with-swaps/)
```java
class Solution {
    public class UF {
        //same code as above...
    }
    
    public String smallestStringWithSwaps(String s, List<List<Integer>> pairs) {
        int N = s.length();
        
        //initialize new UF object
        UF uf = new UF(N);
        
        //connect every pair of indices which can be swapped
        for(List<Integer> pair : pairs) {
            uf.union(pair.get(0), pair.get(1));
        }
        
        //use a priority queue and a HashMap to store the least letter that can be presented at each index
        HashMap<Integer, PriorityQueue<Character>> map = new HashMap<Integer, PriorityQueue<Character>>();
        for(int i = 0; i < N; i++) {
            map.putIfAbsent(uf.find(i), new PriorityQueue<Character>());
            map.get(uf.find(i)).offer(s.charAt(i));
        }
        
        //every time, the priority queue pop out the least letter available, 
        // the final StringBuilder is the lexicographically smallest string
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i < N; i++) {
            sb.append(map.get(uf.find(i)).poll());
        }
        
        return sb.toString();
    }
}
```