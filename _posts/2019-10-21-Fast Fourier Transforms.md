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

2. How to implement the fast Fourier transform (FFT) from scratch.

- By doing this we will reduce the time complexity of polynomial multiplication from $O(n^2)$ to $O(n\ log\ n)$. 
- Perhaps more importantly, you will also learn one of the most useful and widely deployed computational tools in science and engineering.



### Polynomials (1) 

- A degree $n − 1$ polynomial in $x$ can be seen as a function: 

$$
A(x)=\sum_{i=0}^{n-1}a_i \cdot x^i
$$

- Any integer greater than the degree of a polynomial is a degree-bound of that polynomial.

- The polynomial A in x is:
  $$
  a_0 \cdot x^0+a_1 \cdot x^1 + a_2 \cdot x^2 + ... +a_{n-1} \cdot x^{n-1}
  $$

- The values $a_i$ are the coefficients, the degree is $n − 1$ and $n$, for example, is a degree-bound.

- We can express any integer as a kind of polynomial by setting $x$ to some base, say for decimal numbers:
  $$
  A(x)=\sum_{i=0}^{n-1}a_i \cdot 10^i
  $$

### Polynomials (2)

- The variable $x$ allows us to evaluate the polynomial at a point:

- Evaluation just means **plugging a value** into the variable $x$.

- Forexample $A(3)=a_0\cdot 3 +a_1 ·3 +a_2 ·3 ···+a_{n−1}3^{n-1}$. 

- A fast way to evaluate a polynomial is using **Horner’s Rule**.

  - Instead of computing all the terms individually, we do

$$
A(3) = a_0 +3·(a_1 +3·(a_2 +···+3·(a_n−1)))
$$

  - This method requires $O(n)$ operations: 

```pseudocode
EVALUATE-HORNER(A, n, x)
    begin
      t ← 0
      for i = n − 1 downto 0 step −1 do
        t ← (t · x) + ai 
      return t
    end
```

**Example**

Consider $A(x) = 2 + 3x + 1 \cdot x^2$ We can evaluate this as  $A(x) = 2 + x(3 + 1\cdot x) $





### Coefficient Based Polynomial Arithmetic

- Once we have our polynomial representations, we might want to do some **arithmetic** with them. 

- For a coefficient representation, the addition $C = A + B$ constructs $C$ as the vector: 

  $$
  (a_0 +b_0,a_1 +b_1,a_2 +b2,...,a_{n−1} +b_{n−1})
  $$

- Strictly speaking, $A$ and $B$ should have **the same length** but in practice we can just **pad with zero** coefficients to make this so. 



### Point Value Representation of Polynomials (坐标表示多项式)

> **FACT**
>
> Given $n$ points$(x_i,y_i)$,with all $x_i$ distinct, there is a **unique polynomial** $A(x)$ of degree-bound n such that $y_k =A(x_k)$ for $k = 0,1,...,n−1$. 

<img src="/Users/elvischen/Desktop/Screenshot 2019-10-23 at 07.44.50.png" alt="Screenshot 2019-10-23 at 07.44.50" style="zoom:50%;" />



### Point Value Polynomial Arithmetic

- For a point-value representation, the addition **C = A + B** constructs **C** as: 

  $\{(x_0,y_0 +z_0),(x_1,y_1 +z_1),(x_2,y_2 +z_2),...,(x_{n−1},y_{n−1} +z_{n−1})\}$ where $x_i$ is a point, $y_i = A(x_i)$ and $z_i = B(x_i)$. 

- 􏰀  Note that the two point-value representations must use **the same evaluation points**. 

- 􏰀  Both these operations are $O(n)$ in terms of the time they take.



### Polynomial Multiplication

- Computing a polynomial multiplication, sometimes called **convolution**, is a little bit harder than addition.

- For a coefficient representation, the product **C = A × B** can be calculated with school-book long multiplication:

- $$
  C(x)=\sum_{i=0}^{2n-2}c_ix_i
  $$

  where
  $$
  c_i=\sum_{j=0}^ia_j\cdot b_{i-j}
  $$
  
- **@To do now**: multiply $7x^2 −10x +9$ and $2x^2 +4x −5$.

- For a point-value representation, **C = A × B** is a bit easier: 

- $$
\{(x_0,y_0\cdot z_0),(x_1,y_1·z_1),(x_2,y_2·z_2),...,(x_{n−1},y_{n−1}·z_{n−1})\} 
  $$

  where $x_i$ is a point, $y_i = A(x_i)$ and $z_i = B(x_i)$. 

- The first method is $O(n^2)$, the second method is $O(n)$ !

- Actually, we can do a bit better than the $O(n^2)$ case using the **divide and conquer** method due to **Karatsuba**. 

- This gives an algorithm with time complexity:

- $$
  O(n^{log_23}) = O(n^{1.59})​
  $$

  which is better than our previous method which took $O(n^2)$ operations.

- The problem is that even this is too slow ... we know that using a point-value representation is $O(n)$ ! 

- So a better technique would be to traverse around this diagram: 

<img src="/Users/elvischen/Desktop/Screenshot 2019-10-23 at 08.19.03.png" alt="Screenshot 2019-10-23 at 08.19.03" style="zoom: 50%;" />

- Note that the opposite of evaluation is called **interpolation**. 

  - So we evaluate to a *point-value representation*, multiply and then interpolate back again. 

  - The question is, are we quicker than the normal multiply?



### The Main Idea -Part 1

Develop two fast algorithms that for any polynomial:
$$
A(x)=\sum_{i=0}^{n-1}a_i\cdot x^i
$$
and a preselected set $\{x_0, x_1, . . . , x_{n−1}\}$ of numbers (to be specified before we know which polynomials we will have),

- 􏰀  Evaluate $A(x_0), A(x_1) . A(x_{n−1}) $(evaluate) 
- 􏰀  Given $A(x_0), A(x_1), . . . , A(x_{n−1})$, reconstruct A’s coefficients a0,a1,...am−1 (interpolate) 



### The Main Idea -Part 2

The main steps for fast multiplication of two polynomials ***A*** and ***B*** each of degree n are: 

1. ***Double degree-bound***

   Create coefficient representations of $A(x)$ and $B(x)$ as degree-bound $2n$ polynomials by adding $n$ high-order zero coefficients to each.

2. ***Evaluate***

   Compute point-value representations of *A*(*x*) and *B*(*x*) of length 2*n* through two applications of the FFT of order 2*n*.

3. ***Pointwise multiply***

   Compute a point-value representation of $C(x) = A(x)B(x)$ by multiplying the values pointwise.

4. ***Interpolate***

   Create a coefficient representation of *C*(*x*) through a single application of the *inverse* FFT. 



### Evaluation at Roots of Unity

- First let’s address evaluation: 

  - We need to evaluate a polynomial of degree $n$ at $n$ different points 

    (ignore the degree-bound doubling for the moment). 

  - Appears complexity of our method will be $O(n^2)$. 

  - Is there a faster way of doing this than just using Horner’s Rule? 

- Yes there is, we select the points we evaluate at to be **special**. 

- These special points are chosen to be the **N-th Complex Roots of Unity**: 

  - That is, the values $\omega_N =e^{2πij/N}$ for $j=0,1,...,N−1$. 

  - Say we are evaluating at $N$ points so we take the N-th complex roots of 

    unity $ω_N$. 

  - That is, we evaluate the polynomial at the points: 
  $$
  \omega_N^0,\omega_N^1,\omega_N^2...\omega_N^{N-1}
  $$

- What the hell am I talking about ? Try an example:
  - We know that $ω_N^j =e^{2πij/N}$ for $j=0,1,...,N−1$.
  - So given the well known identity $e^{iu} = cos(u) + i\ sin(u)$, we can draw the values $ω_N^j$ .
    An easy one to draw is for $N =8$.

<img src="https://tva1.sinaimg.cn/large/006y8mN6gy1g886x8kdx7j30qo0pi3zr.jpg" style="zoom:33%;" />



### Discrete Fourier Transform 



### A Couple of Lemmas

*The Halving Lemma:* *IfN*>0*iseventhenthesquaresoftheNcomplex N-th roots of unity are the N*/2 *complex N*/2*-th roots of unity.* 



### Fast Fourier Transform



### Example of Divide Step



### Fast Fourier Transform



### FFT - Worked Example



### Fast Fourier Transform - Analysis 



### Polynomial Evaluation - Summary 

Remember that our aim was to evaluate two polynomials *A* and *B* of degree-bound *n* at the roots of unity. We also needed to double the degree-bound to help us perform the multiplication later on. 

1. Pad the coefficient vector for *A* with zeros so that their length is 2*n* (assume *n* is a power of two). 

2. Define *A*(*x*) = 􏰁2*n*−1 *a**i**x**i* (half of the coefficients are zero). *i*=0 

   *j* 􏰁*n*−1 *ji* 

3. Define *y**j* = *A*(ω*n*) = *i*=0 *a**i*ω*n*. 

4. Then the vector *y* = (*y*0, *y*1, *y*2, . . . , *y*2*n*−1) is the 2*n*-element DFT of *A*. 

5. We have our point-value representation as: 

   {(ω0 ,*y* ),(ω1 ,*y* ),(ω2 ,*y* ),...,(ω2*n*−1,*y* )} 2*n* 0 2*n* 1 2*n* 2 2*n* 2*n*−1 

Repeat the same process for polynomial *B*. 



### Inverse Fourier Transform - Interpolation 

􏰀 We will use the Inverse DFT to interpolate polynomials. This relies on a Theorem that shows how to invert the DFT: 

1 *n*−1
 *a*= 􏰂*yw*−*ji* 

*i**n**jn j*=0 

Although we won’t prove this here, this allows us to convert the 

point-value representation of a polynomial to coefficient form. 

􏰀 So if we can compute the DFT, the Inverse DFT simply does the same thing with a few amendments: 

\1. Switching roles of *a* and *y* 2. Replace ω*n* by ω−1, 

*n
\* 3. Divide the final result by *n*. 

􏰀 The Inverse DFT can therefore be computed in the same time complexity as the DFT. I.e. both take *O*(*n* log *n*) time. 



### Polynomial Multiplication - Summary

We have shown how to perform the main steps involved in polynomial multiplication 

1. *Double degree-bound:* Create coefficient representations of *A*(*x*) and *B*(*x*) as degree-bound 2*n* polynomials by adding *n* high-order zero coefficients to each. *O*(*n*) time. 
2. *Evaluate:* Compute point-value representations of *A*(*x*) and *B*(*x*) of length 2*n* through two applications of the FFT of order 2*n*. *O*(*n* log *n*) time. 
3. *Pointwise multiply:* Compute a point-value representation of *C*(*x*) = *A*(*x*)*B*(*x*) by multiplying the values pointwise. *O*(*n*) time 
4. *Interpolate:* Create a coefficient representation of *C*(*x*) through a single application of the *inverse* FFT. *O*(*n* log *n*) time. 

Therefore the overall time complexity of polynomial multiplication is *O*(*n* log *n*). This is a lot better than the *O*(*n*2) time we started with! 



### Conclusions

- We are able to multiply two polynomials of degree-bound $n$ in $O(n\ log\ n)$ operations. 
- Therefore we can multiply polynomials faster than even the Karatsuba approach.
  - The extra operations mean this method is only faster for reasonably large polynomials. 

- Consider the context: 
  - Graphics and signal processing applications use FFT **a lot** on very large data sets. 
  - Even a small improvement in asymptotic time complexity will help massively when the input size is large. 
  - FFTs can also be used for string matching problems. 
- We’ve taken a step in the right direction using two **underlying principles**: 
  - Divide and conquer is a very powerful tool. 
  - A little mathematics can go a very long way. 



### Further Reading 

- Introduction to Algorithms
  - Chapter 30 – Polynomials and the FFT
- Algorithm Design
	- Chapter 5 – Divide and Conquer 