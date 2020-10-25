---
title: 'Summary of merge sort application'
date: 2020-09-07
permalink: /posts/2020/09/leetcode-merge-sort/
tags:
  - Leetcode
  - Algorithms
  - code interview
  - Merge Sort
---

Among all of the sorting algorithms, merge sort has several obvious advantages.

<!--more-->

1.it is easy to implement(recursion + divide and conquer)

2.its time complexity is O(NlgN) guaranteed which is better than other frequently used sorting algorithms.

3.it is stable, which means that the relative positions of elements with the same value will not be altered during sorting process.

Hence, merge sort is not rare in code interview questions. 

> ![](https://xiaoluo-whu.github.io/files/images/merge_sort_overview.jpg)

## Implementations
The implementation of merge sort uses the idea of divide and conquer, which recursively divide the array into two halves, sorting each half and then merge the two parts.
```java
public class MergeSort {
    
    public void sort(int[] a) {
        int[] aux = new int[a.length];
        sort(a, aux, 0, a.length-1);
    }
    
    // mergesort a[lo..hi] using auxiliary array aux[lo..hi]
    private void sort(int[] a, int[] aux, int lo, int hi) {
        if (hi <= lo) return;
        int mid = lo + (hi - lo) / 2;
        sort(a, aux, lo, mid);
        sort(a, aux, mid + 1, hi);
        merge(a, aux, lo, mid, hi);
    }

    // stably merge a[lo .. mid] with a[mid+1 ..hi] using aux[lo .. hi]
    private void merge(int[] a, int[] aux, int lo, int mid, int hi) {

        // copy to aux[]
        for (int k = lo; k <= hi; k++) {
            aux[k] = a[k]; 
        }

        // merge back to a[]
        int i = lo, j = mid+1;
        for (int k = lo; k <= hi; k++) {
            if      (i > mid)              a[k] = aux[j++];
            else if (j > hi)               a[k] = aux[i++];
            else if (aux[j] < aux[i]) a[k] = aux[j++];
            else                           a[k] = aux[i++];
        }
    }
}
```
However, recursion is never constant space complexity. If you are required to use as little space as possible,
there is a bottom-up version of merge sort, which uses loop instead of recursion.
```java
public class BottomUpMergeSort {

    public static void sort(int[] a) {
        int n = a.length;
        int[] aux = new int[n];
        for (int len = 1; len < n; len *= 2) {
            for (int lo = 0; lo < n - len; lo += len + len) {
                int mid  = lo + len - 1;
                int hi = Math.min(lo + len + len - 1, n - 1);
                merge(a, aux, lo, mid, hi);
            }
        }
    }

    // stably merge a[lo..mid] with a[mid+1..hi] using aux[lo..hi]
    private void merge(int[] a, int[] aux, int lo, int mid, int hi) {

        // copy to aux[]
        for (int k = lo; k <= hi; k++) {
            aux[k] = a[k]; 
        }

        // merge back to a[]
        int i = lo, j = mid+1;
        for (int k = lo; k <= hi; k++) {
            if      (i > mid)              a[k] = aux[j++];  
            else if (j > hi)               a[k] = aux[i++];
            else if (aux[j] < aux[i]) a[k] = aux[j++];
            else                           a[k] = aux[i++];
        }

    }
}
```
## Proof of time complexity
We know that merge sort is O(NlgN) guaranteed, here is why.

C(N) = C(N / 2) + C(N / 2) + N     

C(N) denotes the operations needed to sort a array of size(N). Then, it can be decomposed into 3 parts, C(N / 2) operations to sort the left N / 2 elements and right N / 2 elements, N to merge the 2 parts.
Then we have the following equation,

C(N) = 2C(N / 2) + N

Since merge sort will recursively divide the original array and merge two sorted halves. We can illustrate the above equation using following picture

> ![](https://xiaoluo-whu.github.io/files/images/merge_sort_proof.jpg)

Next, we use induction to prove C(N) = NlgN.

1.when N = 1, C(N) = 0;

2.suppose C(N) = NlgN;

3.then C(2N) = 2NlgN + 2N = 2Nlg(2N)

Proved!

Here, I list several LeetCode questions involved with merge sort.
## 1. [Sort List](https://leetcode.com/problems/sort-list/)
this questions is actually easy. To sort the list using constant space, bottom-up merge sort would work.

(However, I strongly suggest that do not sort a linked list. Linked list is a data structure to connect each node, making it easy to search and query, but it is not designed to sort.)
```java
public ListNode sortList(ListNode head) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    int n = 0;
    while (head != null) {
        head = head.next;
        n++;
    }
    
    for (int step = 1; step < n; step <<= 1) {
        ListNode prev = dummy;
        ListNode cur = dummy.next;
        while (cur != null) {
            ListNode left = cur;
            ListNode right = split(left, step);
            cur = split(right, step);
            prev = merge(left, right, prev);
        } 
    }
    
    return dummy.next;
}

private ListNode split(ListNode head, int step) {
    if (head == null) return null;
    
    for (int i = 1; head.next != null && i < step; i++) {
        head = head.next;
    }
    
    ListNode right = head.next;
    head.next = null;
    return right;
}

private ListNode merge(ListNode left, ListNode right, ListNode prev) {
    ListNode cur = prev;
    while (left != null && right != null) {
        if (left.val < right.val) {
            cur.next = left;
            left = left.next;
        }
        else {
            cur.next = right;
            right = right.next;
        }
        cur = cur.next;
    }
    
    if (left != null) cur.next = left;
    else if (right != null) cur.next = right;
    while (cur.next != null) cur = cur.next;
    return cur;
}
```

## 2. [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)
Since merge sort is stable, we can count the number that will jump from right to left during merging process, which is the count of smaller number after self.
```java
class Solution {
    private void merge(int[] a, int[] indexes, int lo, int mid, int hi) {
        int[] new_indexes = new int[indexes.length];

        int i = lo, j = mid+1, rightCount = 0;
        for (int k = lo; k <= hi; k++) {
            if (i > mid) {
                new_indexes[k] = indexes[j++];
            }
            else if (j > hi) {
                new_indexes[k] = indexes[i];
                count[indexes[i]] += rightCount;
                i++;
            }
            else if (a[indexes[j]] < a[indexes[i]]) {
                new_indexes[k] = indexes[j++];
                rightCount++;
            }
            else {
                new_indexes[k] = indexes[i];
                count[indexes[i]] += rightCount;
                i++;
            }
        }

        for (int k = lo; k <= hi; k++) {
            indexes[k] = new_indexes[k];
        }

    }

    private void sort(int[] a, int[] indexes, int lo, int hi) {
        if (hi <= lo) return;
        int mid = lo + (hi - lo) / 2;
        sort(a, indexes, lo, mid);
        sort(a, indexes, mid + 1, hi);
        merge(a, indexes, lo, mid, hi);
    }

    int[] count;

    public List<Integer> countSmaller(int[] nums) {
        List<Integer> result = new ArrayList();
        count = new int[nums.length];
        int[] indexes = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            indexes[i] = i;
        }
        sort(nums, indexes, 0, nums.length - 1);

        for (int i = 0; i < nums.length; i++) {
            result.add(count[i]);
        }

        return result;
    }
}
```

## 3. [Count of Range Sum](https://leetcode.com/problems/count-of-range-sum/)
```java
class Solution {
    private void merge(long[] a, long[] aux, int lo, int mid, int hi) {
        for(int k = lo; k <= hi; k++) aux[k] = a[k];

        int i = lo, j = mid+1, start = mid + 1, end = mid + 1;
        for(int m = i; m <= mid; m++) {
            while (start <= hi && aux[start] - aux[m] < lower) start++;
            while (end <= hi && aux[end] - aux[m] <= upper) end++;
            count += end - start;
        }
        for (int k = lo; k <= hi; k++) {

            if (i > mid) {
                a[k] = aux[j++];
            }
            else if (j > hi) {
                a[k] = aux[i++];
            }
            else if (aux[j] < aux[i]) {
                a[k] = aux[j++];
            }
            else {
                a[k] = aux[i++];
            }
        }

    }

    private void sort(long[] a, long[] aux, int lo, int hi) {
        if (hi <= lo) return;
        int mid = lo + (hi - lo) / 2;
        sort(a, aux, lo, mid);
        sort(a, aux, mid + 1, hi);
        merge(a, aux, lo, mid, hi);
    }

    int count = 0;
    int lower;
    int upper;
    public int countRangeSum(int[] nums, int lower, int upper) {

        long[] sum = new long[nums.length + 1];
        long[] aux = new long[nums.length + 1];
        for(int i = 0; i < nums.length; i++) {
            sum[i + 1] = sum[i] + nums[i];
        }
        this.lower = lower;
        this.upper = upper;

        sort(sum, aux, 0, sum.length - 1);

        return count;
    }
}
```

## 4. [Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)
```java
class Solution {
    private void merge(int[] a, int[] aux, int lo, int mid, int hi) {
        for(int k = lo; k <= hi; k++) aux[k] = a[k];

        int i = lo, j = mid+1, index = mid + 1;
        for(int m = lo; m <= mid; m++) {
            while(index <= hi && a[index] < a[m] / 2.0) index++;
            count += (index - mid - 1);
        }
        for (int k = lo; k <= hi; k++) {

            if (i > mid) {
                a[k] = aux[j++];
            }
            else if (j > hi) {
                a[k] = aux[i++];
            }
            else if (aux[j] < aux[i]) {
                a[k] = aux[j++];
            }
            else {
                a[k] = aux[i++];
            }
        }

    }

    private void sort(int[] a, int[] aux, int lo, int hi) {
        if (hi <= lo) return;
        int mid = lo + (hi - lo) / 2;
        sort(a, aux, lo, mid);
        sort(a, aux, mid + 1, hi);
        merge(a, aux, lo, mid, hi);
    }

    int count = 0;
    
    public int reversePairs(int[] nums) {
        int[] aux = new int[nums.length];
        sort(nums, aux, 0, nums.length - 1);

        return count;
    }
}
```
