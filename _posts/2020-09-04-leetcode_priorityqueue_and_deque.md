---
title: 'Summary of priority queue and deque(double-ended-queue)'
date: 2020-09-04
permalink: /posts/2020/09/leetcode-priorityqueue-and-deque/
tags:
  - Leetcode
  - Algorithms
  - code interview
  - priority queue
---

to be updated

[Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
```java
public ListNode mergeKLists(ListNode[] lists) {
    PriorityQueue<ListNode> pq = new PriorityQueue<ListNode>((n1, n2) -> n1.val - n2.val);
    for (ListNode listNode : lists) {
        if(listNode != null)
        pq.offer(listNode);
    }

    ListNode dummy = new ListNode(0);
    ListNode temp = dummy;
    while (!pq.isEmpty()) {
        ListNode poll = pq.poll();
        temp.next = new ListNode(poll.val);
        temp = temp.next;
        if (poll.next != null)
            pq.offer(poll.next);
    }
    
    return dummy.next;
}
```

[Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
```java
public int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> pq = new PriorityQueue<Integer>();
    for(int i = 0; i < nums.length; i++) {
        pq.offer(nums[i]);
        if (pq.size() > k) pq.poll();
    }

    return pq.peek();
}
```

[Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)
```java
public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
    if(nums1.length == 0 || nums2.length == 0) return new ArrayList();
    PriorityQueue<int[]> pq = new PriorityQueue<int[]>((n1,n2) -> (nums1[n1[0]] + nums2[n1[1]] - nums1[n2[0]] - nums2[n2[1]]));
    List<List<Integer>> result = new ArrayList();

    for (int i = 0; i < nums1.length; i++) {
        pq.offer(new int[] {i, 0});
    }


    while(!pq.isEmpty()) {
        int[] a = pq.poll();
        if (result.size() >= k) {
            return result;
        } else {
            List<Integer> res = new ArrayList();
            res.add(nums1[a[0]]);
            res.add(nums2[a[1]]);

            result.add(res);

            if (a[1] + 1 < nums2.length)
            pq.offer(new int[] {a[0], a[1] + 1});
        }

    }

    return result;
}
```

[Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)
```java
public static class Elem {
    public int r;
    public int c;

    public Elem(int r, int c) {
        this.r = r;
        this.c = c;
    }
}

public int kthSmallest(int[][] matrix, int k) {
    int N = matrix.length;
    PriorityQueue<Elem> pq = new PriorityQueue<>((n1, n2) -> matrix[n1.r][n1.c] - matrix[n2.r][n2.c]);

    for (int i = 0; i < N; i++) {
        pq.offer(new Elem(i, 0));
    }

    Elem temp;
    for (int i = 1; i <= k - 1; i++) {
        temp = pq.poll();
        if (temp.c < N - 1)
            pq.offer(new Elem(temp.r, temp.c + 1));
    }

    return matrix[pq.peek().r][pq.peek().c];
}
```

[Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)
```java
class MedianFinder {

    PriorityQueue<Integer> pq1; 
    PriorityQueue<Integer> pq2; 
    /** initialize your data structure here. */
    public MedianFinder() {
        pq1 = new PriorityQueue<Integer>();
        pq2 = new PriorityQueue<Integer>((n1, n2) -> (n2 - n1));
    }
    
    public void addNum(int num) {
        if(pq1.size() < pq2.size()) pq1.offer(num);
        else pq2.offer(num);
    
        if(!pq1.isEmpty()) {
            while(pq1.peek() < pq2.peek()) {
                pq2.offer(pq1.poll());
                pq1.offer(pq2.poll());
            }
        }
        
    }
    
    public double findMedian() {
        if(pq1.size() == pq2.size()) return (pq1.peek() + pq2.peek()) / 2.0;
        else return (double) pq2.peek();
    }
}
```

[Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/solution/)
```java
public int[] topKFrequent(int[] nums, int k) {
    // O(1) time
    if (k == nums.length) {
        return nums;
    }
    
    // 1. build hash map : character and how often it appears
    // O(N) time
    Map<Integer, Integer> count = new HashMap();
    for (int n: nums) {
      count.put(n, count.getOrDefault(n, 0) + 1);
    }

    // init heap 'the less frequent element first'
    Queue<Integer> heap = new PriorityQueue<>(
        (n1, n2) -> count.get(n1) - count.get(n2));

    // 2. keep k top frequent elements in the heap
    // O(N log k) < O(N log N) time
    for (int n: count.keySet()) {
      heap.add(n);
      if (heap.size() > k) heap.poll();    
    }

    // 3. build an output array
    // O(k log k) time
    int[] top = new int[k];
    for(int i = k - 1; i >= 0; --i) {
        top[i] = heap.poll();
    }
    return top;
}
```

[Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)
```java
class Letter {
    char c;
    int count;
    
    Letter(char c, int count) {
        this.c = c;
        this.count = count;
    }
}

public String frequencySort(String s) {
    int[] table = new int[128];
    for(int i = 0; i < s.length(); i++) table[s.charAt(i)]++;
    
    PriorityQueue<Letter> pq = new PriorityQueue<Letter>((n1, n2) -> (n2.count - n1.count));
    for(int i = 0; i < table.length; i++) pq.offer(new Letter((char) i, table[i]));
    
    StringBuilder sb = new StringBuilder();
    while(!pq.isEmpty()) {
        Letter l = pq.poll();
        for(int i = 0; i < l.count; i++) sb.append(l.c);
    }
    
    return sb.toString();
    
}
```

[Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)
```java
public int[] maxSlidingWindow(int[] nums, int k) {
    int[] result = new int[nums.length - k + 1];
    Deque<Integer> deque = new ArrayDeque<Integer>();
    
    for (int i = 0; i < nums.length; i++) {
        //remove those elements whose index is on the left of the sliding window
        while (!deque.isEmpty() && deque.peek() < i - k + 1) deque.poll();
        
        //for the elements whose value is smaller than nums[i], we no longer need them since we seek the maximum of every step
        while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) deque.pollLast();
        deque.offer(i);
        
        //result's length is nums.length - k + 1
        if (i >= k - 1)
        result[i - k + 1] = nums[deque.peek()];
    }
    return result;
}
```