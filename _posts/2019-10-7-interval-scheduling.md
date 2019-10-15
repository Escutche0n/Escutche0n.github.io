---
layout:     post
title:      Interval Scheduling
subtitle:   
date:       2019-10-07
author:     "Elvis"
catalog: true
comments: true
guid: urn:uuid:bd69ccfc-e840-4bc4-99ec-54cefac7f749
tags:
    - Algorithm
---
# Interval Scheduling



### Interval scheduling: a formal definition



A **request** （请求） is a pair of integers $(s, f )$ with $0 ≤ s ≤ f$.

We call **s** the start time and **f** the finish time.

A set A of requests is **compatible** if for all distinct  $(s, f ), (s′, f ′) ∈ A$, either $s′ ≥ f$ or $s ≥ f ′$ — that is, the requests’ time intervals （间隔） don’t overlap.

##### Interval Scheduling Problem

**Input**: An array $R$ of $n$ requests $(s_1, f_1), . . . , (s_n, f_n)$.
Desired Output: A compatible subset of $R$ of <u>maximum possible size</u>.



### Solving interval scheduling

**Idea**: Use a **greedy** approach: start with an empty output and slowly build it up, making choices that look good in the moment, without worrying about future implications. These algorithms are very common and useful!



Greedy 贪婪算法：从一个 empty output 开始，选一个看起来正确的选项，不要管之后的implications。



**Heuristic**: Tie up the satellite for as little time as possible before the next request. So at each stage, we accept the request which finishes earliest (out of those we haven’t already missed the start time for).



Heuristic 启发式算法：在每一个 stage，我们接受我们选择那个最早完成的 request（在那些尚未错过的选项里）。

---

**Algorithm**: GreedySchedule
**Input** : An array $R$ of $n$ requests.
**Output**: A maximum compatible subset of $R$.

```markdown
**begin**

​		Sort R’s entries so that $R ← [(s_1, f_1), . . . , (s_n, f_n)]$ where

​				$f_1 ≤ ··· ≤ f_n$. Initialise $A ← [\ ]$, lastf ← 0.

​		**foreach** i ∈ {1,...,n} **do**

​				**if** $s_i ≥ lastf$ **then**

​						Append (si,fi) to A and update $lastf ← f_i$.

​		Return A.
```

---



##### Time analysis

Step 2 takes $O(n\ log_n)$ time, as covered exhaustively in COMS10007.

Steps 3–6 all take $O(1)$ time and are executed at most n times.

So overall the running time is $O(n\ log_n) + O(n) · O(1) = O(n\ log_n)$. 

---



### Greedy algorithms in general



**Greedy algorithm** is a very informal term, and different people have incompatible definitions. My definition (see e.g. KT’s book) is:

- they start with a sub-optimal (often trivial) solution, e.g. $A = [\ \ ]$, and gradually turn it into the output.
- they look over all possible improvements and pick the one that “looks best at the time”, e.g. adding the fastest-ending compatible request.
- they never backtrack in “quality”, e.g. the size of A never goes down.



1. 目标是 A compatible subset of $R$ of <u>maximum possible size</u>.
2. 首先尝试最早完成的
3. 其次选择最早开始的
4. 最后尝试从最短的开始



Greedy algorithms are not unique, and may fail!

