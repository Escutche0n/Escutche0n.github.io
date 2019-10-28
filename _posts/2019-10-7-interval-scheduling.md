---
layout:     post
title:      Topic 1: Interval Scheduling
subtitle:   
date:       2019-10-07
author:     "Elvis"
catalog: true
comments: true
guid: urn:uuid:bd69ccfc-e840-4bc4-99ec-54cefac7f749
tags:
    - Algorithm
---

### Satellite scheduling

Suppose you’re running a satellite imaging service.

Taking a satellite picture of an area isn’t instant — it takes time proportional to its latitude, and it can only be done **at a specific time of day** (when your satellite’s orbit is lined up correctly).

You have a set of requested images, each of which can only be taken at specific times, and you can only take one picture at once.

How can you satisfy as many requests as possible?



### Satellite scheduling: An example

Requested satellite times:

12:00–12:30, **12:05–12:20**, 12:15–12:55, **12:20-12:25**, **12:25–12:40**, 12:38–12:50, and **12:45-13:00**.

![w1_time](https://svgshare.com/i/Fju.svg)

Here, we can satisfy **four** requests.
We might as well give our times integer labels for simplicity, though.



### Interval scheduling: a formal definition

A **request** is a pair of integers $(s, f )$ with $0 ≤ s ≤ f$.

We call $s$ the **start time** and $f$ the **finish time**.

A set `A` of requests is **compatible** if for all distinct  $(s, f ), (s′, f ′) ∈ A$ , either $s′ ≥ f$ or $s ≥ f ′$ — that is, the requests’ time intervals don’t overlap.

##### Interval Scheduling Problem

**Input**: An array $R$ of $n$ requests $(s_1, f_1), . . . , (s_n, f_n)$.

**Desired Output**: A compatible subset of $R$ of <u>maximum possible size</u>.

Our satellite problem above is an example of this — the **maximum compatible subset** is the list of image requests we accept.



### Solving interval scheduling

**Idea**: Use a **greedy** approach: start with an empty output and slowly build it up, making choices that look good in the moment, without worrying about future implications.

**Heuristic**: Tie up the satellite for as little time as possible before the next request. So at each stage, we accept the request which finishes earliest (out of those we haven’t already missed the start time for).

![w1_diagram_2](https://svgshare.com/i/Fjt.svg)

```pseudocode
Algorithm: GreedySchedule
Input    : An array R of n requests.
Output   : A maximum compatible subset of R.

begin
	Sort R’s entries so that R ← [(s_1, f_1), . . . , (s_n, f_n)] where
	f_1 ≤ ··· ≤ f_n. Initialise A ← [ ], lastf ← 0.
	foreach	i ∈ {1,...,n}	do
		if	s_i ≥ lastf	then
			Append (s_i,f_i) to A and update lastf ← f_i.
	Return A
```

---

##### Time analysis

Step 2 takes $O(n\ log\ n)$ time, as covered exhaustively in COMS10007.

Steps 3–6 all take $O(1)$ time and are executed at most n times.

So overall the running time is $O(n\ log\ n) + O(n) · O(1) = O(n\ log\ n)$. 



##### Proof that the output is a compatible subset of $R$ (Warmup!) 

Intuitively: A was compatible at the start of the iteration, and the set we’ve added doesn’t break compatibility because `s ≥ lastf​` and `lastf` is the latest finish time already in `A`.

Formalise this via loop invariant — at the start of the $i$’th iteration of 3–5:

- `A` contains a compatible subset $\{(S_1, F_1), . . . , (S_t , F_t )\}$ of $R$ ;
- `lastf` = $max(\{0\} ∪ \{F_j : j ≤ t\})$.

**Base case** $(i = 0)$: immediate because `A = [ ]`. 

**Induction step**: `A` was compatible at the start of the iteration, and if we append a pair $(s_i,f_i)$ to `A` then `s ≥ lastf ≥ Fj` for all `j≤t`. So $(s_i,f_i)$ is compatible with `A`. 



##### Proof that the output is maximum

Intuitively: GreedySchedule “stays ahead” of a max. compatible set.

Formalise this via loop invariant. At the start of the i’th iteration of 3–5, write $A = [(S_{1},F_{1}),...,(S{ti},F_{ti} )]$ where $F_1 < ··· < F_{ti}$. We claim:

> for **all** maximum compatible subsets $S = \{(S_1′,F_1′),...,(S_l′,F_l′)\}$, we have $F_1 ≤ F_1,...,F_{t_{i}} ≤ F'_{t_{i}}$.


Thinking of this invariant is hard! But proving it is a standard induction. 



At the start of the i’th iteration of 3–5, write $A = [(S_1,F1),...,(S_{t_i},F_{t_i})]$. We show by induction that: 

> for **all** maximum compatible subsets $S = \{(S_1′,F_1′),...,(S_l′,F_l′)\}$, we have $F_1 ≤ F'_1,...,F_{t_i}≤ F'_{t_i}$ . 



Induction step: We know $F_1 ≤ F_1',...,F_t ≤ F_t′$ by induction. We have $S′ ≥F′ ≥F$, so ($S′_{t_i+1},F′_{t_i+1}$) is compatible with A.

So the pair ($S_{i+1},F_{i+1}$) we are considering must have $f_{i+1} ≤ F_{t_i+1}$. So whether we add it or not, the induction hypothesis stays true. 



### Greedy algorithms in general

**Greedy algorithm** is a very informal term, and different people have incompatible definitions.

My definition (see e.g. KT’s book) is:

- They start with a sub-optimal (often trivial) solution, e.g. A = [ ], and gradually turn it into the output.
- They look over **all possible improvements** and pick the one that “looks best at the time”.
  - e.g. adding the fastest-ending compatible request.
- They never backtrack in “quality”, e.g. the size of A never goes down.

Some people (see e.g. CLRS’ book) have stricter definitions. But this will never actually matter to you! 

What matters is being able to **use** and **design** algorithms like this one. 

The idea for our correctness proof — using an invariant to show that our algorithm “stays ahead” of an optimal solution — is often used for other greedy algorithms too. Worth practising!



### Greedy algorithms are not unique, and may fail

A greedy algorithm is not “just do the obvious thing at each stage”.

Sometimes there are multiple obvious things, and only a few will work!



E.g. What if instead of choosing the **fastest-finishing request** to add at each stage, we chose the fastest-starting request?

![w1_diagram_1](https://svgshare.com/i/Fjs.svg)



Or we chose the shortest interval? 

![w1_diagram_1](https://svgshare.com/i/FmW.svg)

Still no luck.

> The lesson is: don’t give up trying if **“the” greedy algorithm** <u>doesn’t work</u>, because there are lots of possible greedy algorithms!

