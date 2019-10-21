---
layout:     post
title:      Fast Fourier Transforms
subtitle:   
date:       2019-10-21
author:     "Elvis"
catalog: true
comments: true
guid: urn:uuid:bd69ccfc-e840-4bc4-99ec-54cefac7f749
tags:
    - Algorithm
---

### Lecture Overview 

In this lecture we will discuss two main related topics:

1. How to multiply two large polynomials (and therefore integers) using Fourier transforms.

2. How to implement the **fast Fourier transform** (FFT) from scratch.

- By doing this we will reduce the time complexity of polynomial multiplication from $O(n^2)$ to $O(n\ log\ n)$. 
- Perhaps more importantly, you will also learn one of the most useful and widely deployed computational tools in science and engineering.



### Polynomials (1) 

- A **degree** $n − 1$ polynomial in $x$ can be seen as a function: 

$$
A(x)=\sum_{i=0}^{n-1}a_i \cdot x^i
$$

- Any integer greater than the degree of a polynomial is a **degree-bound** of that polynomial.

- The polynomial *A* in *x* is:
  $$
  a_0 \cdot x^0+a_1 \cdot x^1 + a_2 \cdot x^2 + ... +a_{n-1} \cdot x^{n-1}
  $$

- The values $a_i$ are the coefficients, the degree is $n − 1$ and $n$, for example, is a degree-bound.

- We can express any integer as a kind of polynomial by setting $x$ to some base, say for decimal numbers:
  $$
  A(x)=\sum_{i=0}^{n-1}a_i \cdot 10^i
  $$

