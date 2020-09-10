---
title: 'My experiences and suggestions for cracking Leetcode'
date: 2020-08-26
permalink: /posts/2020/08/leetcode-0/
tags:
  - Leetcode
  - Algorithms
  - code interview
---

#### 1.solving questions should always be combined with [Princeton's online algorithm course](https://algs4.cs.princeton.edu/)
I know some of you can not wait to write codes and solve questions. However, if you were not in CS major or 
you have forgotten things learned in college, you may get stuck in some subtle details. For example, [Reverse Pairs](https://leetcode.com/problems/reverse-pairs/) is related to 
merge sort and require you to write down the codes of merge sort at first, then slightly modify it to solve the question. Without a good grasp of 
data structure and algorithm, you may feel it very hard to make progress.

[Algorithms](https://algs4.cs.princeton.edu/) offered by Princeton is one of the most prestigious and comprehensive CS online courses. It covers nearly every mainstream and prevalent data structure and algorithm.
Prof. Sedgewick does a great job in combining abstract theory and concrete java implementations(Although CLRS is also a great option, I have to 
say that it contains much theories and Math equations. Besides, it is based on C++ and may not be friendly to java programmers).
You don't need to do the exercises of the textbook(since we already have plenty of questions on Leetcode), just watch the course videos carefully and review the [source codes](https://algs4.cs.princeton.edu/code) regularly to see whether they can 
serve as code templates to solve questions of Leetcode.

#### 2. make use of Leetcode's question filter. Learning and solving questions tag by tag
> ![](https://xiaoluo-whu.github.io/files/leetcode_filter.jpg)
> ![](https://xiaoluo-whu.github.io/files/leetcode_tag.jpg)

every question on Leecode has at least one tag, which categorizes the question and makes it convenient for you to search and think.
To maximize your efficiency, you should make good use of these tags and the question filter. I suggest you solve questions of the same tag in a certain period of time(say, one or two weeks)
and crack LeetCode tag by tag.

Of course, some tags are fairly easy to solve and some are very hard. My order of solving questions is nearly the same as the order of chapters of Princeton's [Algorithms](https://algs4.cs.princeton.edu/) course:

[Union Find](https://leetcode.com/problemset/algorithms/?topicSlugs=union-find)

[Stack](), [Queue]()

[Sort]()

[Heap(Priority Queue)]()

[Binary Search](), [Binary Search Tree](), [Line Sweep]()

[Hash Table]()

[Graph](), [Depth First Search](), [Breadth First Search](), [Topological Sort]()

[Greedy](), [Tree]()

[Tries]()

[Divide and Conquer](), [Two pointer]()

[Bit Manipulation]()

[Linked List]()

[Dynamic Programming]()

[Math]()

[String](), [Array]()  

In general, put generic tags at the end of your queue and crack those specific and concrete tags first.

#### 3. ignore difficulty level
New users of LeetCode might be confused about Leetcode's difficulty level: easy, medium and hard. And Some may be scared by those dark red "hard" questions.
Actually, there is a good news that a significant amount of questions' levels are not accurate. For example, [Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/) is labeld as "easy",
however, it is much harder and cost no less time than other Hard questions. On the contrary, [N-Queens](https://leetcode.com/problems/n-queens) is labeld as "hard". But as long as you 
master the trick of backtracking, it is fairly easy to solve.

So, you don't need to worry about the levels, just filter questions by a specific tag, and solve questions one by one in order.  

#### 4. first 500 questions is the most important
When the first time I use Leecode, there were only 150 question in total. Nowadays, it increases to nearly 1400.
However, you don't need to be frustrated. You should focus on the first 500 questions. 
LeetCode questions are not perfect from the very start. After the contributor initially submits the question, the test cases and question descriptions are usually not 
comprehensive. It requires users' effort to improve the description and complement those corner cases. 

Consequently, you don't need to hurry to finish those new questions(like question 500 - 1500). The first 500 questions are typical, classic and 
submitted early enough that their description and test cases are much better than those fresh questions. 
You may finish the first 500 questions and then go ahead to do question 500 - 1500.

#### 5. coding on Leetcode's webpage or on your computer's notepad. Don't use IDE before you finish the whole coding process.
Please notice that during a real code interview, you would usually write your code on a whiteboard or scratch paper, no IDEs like IDEA or Eclipse are available. So I strongly
recommend that during daily leetcode training, you code directly on Leetcode webpage which has no features like autocompletion, or on your PC's notepad. After you finish the coding process,
you can copy & paste your codes into your IDE to check errors and debug.

If you are accustomed to writing codes in IDE, you possibly will make many subtle mistakes during code interview, so you should practice writing codes without IDE. Fortunately, interviewers will not require
you to write perfect codes on whiteboard or scratch paper. Minor mistakes are tolerated as long as they do not affect the main structure of your codes.
