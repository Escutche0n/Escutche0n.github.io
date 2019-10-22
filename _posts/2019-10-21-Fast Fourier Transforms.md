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

- Evaluation just means plugging a value into the variable $x$.

- Forexample $A(3)=a_0\cdot 3 +a_1 ·3 +a_2 ·3 ···+a_{n−1}3^{n-1}$. 

- A fast way to evaluate a polynomial is using Horner’s Rule.

  - Instead of computing all the terms individually, we do

  $$
  A(3) = a_0 +3·(a_1 +3·(a_2 +···+3·(a_n−1)))
  $$

  - This method requires $O(n)$ operations: 

```pseudocode
EVALUATE-HORNER(A, n, x)

begin
	t<-0
	for i = n − 1 downto 0 step − 1 do
		t <- (t · x) + ai 
	return t
end
```

Example

Consider $A(x) = 2 + 3x + 1 \cdot x^2$ We can evaluate this as  $A(x) = 2 + x(3 + 1\cdot x) $

(List of array)

### Coefficient Based Polynomial Arithmetic

- Once we have our polynomial representations, we might want to do some arithmetic with them. 

- 􏰀  For a coefficient representation, the addition $C = A + B$ constructs $C$ as the vector: 

  $$
  (a_0 +b_0,a_1 +b_1,a_2 +b2,...,a_{n−1} +b_{n−1})
  $$

- Strictly speaking, $A$ and $B$ should have the same length but in practice we can just pad with zero coefficients to make this so. 



##### FACT

Given n points(xi,yi),with all $x_i$ distinct, there is a unique polynomial A(x) of degree-bound n such that $y_k$ =A($x_k$)fork = 0,1,...,n−1. 



### Point Value Polynomial Arithmetic

- For a point-value representation, the addition C = A + B constructs C as: 

  {($x_0,y_0 +z_0$),(x1,y1 +z1),(x2,y2 +z2),...,(xn−1,yn−1 +zn−1)} where xi is a point, yi = A(xi) and zi = B(xi). 

- 􏰀  Note that the two point-value representations must use the same evaluation points. 

- 􏰀  Both these operations are $O(n)$ in terms of the time they take.



### Polynomial Multiplication

- Computing a polynomial multiplication, sometimes called convolution, is a little bit harder than addition. 

- For a coefficient representation, the product C = A × B can be calculated with school-book long multiplication: 

- C(x) = 􏰂 cixi 

  i=0 

  i
  \ ci =􏰂aj ·bi−j 

  j=0

- To do now: multiply 7x2 −10x +9 and 2x2 +4x −5 

- For a point-value representation, C = A × B is a bit easier: {(x0,y0 ·z0),(x1,y1 ·z1),(x2,y2 ·z2),...,(xn−1,yn−1 ·zn−1)} 

  where xi is a point, yi = A(xi) and zi = B(xi). 

- 􏰀  The first method is O(n2), the second method is O(n) !

- Actually, we can do a bit better than the O(n2) case using the divide and conquer method due to Karatsuba. 

- This gives an algorithm with time complexity:

- $O(n^{log_23}) = O(n^{1.59})$

- which is better than our previous method which took O(n2) operations.



Polynomial Multiplication 

- 􏰀  The problem is that even this is too slow ... we know that using a point-value representation is O(n) ! 

- 􏰀  So a better technique would be to traverse around this diagram: 

- Note that the opposite of evaluation is called interpolation. 

  - 􏰀  So we evaluate to a point-value representation, multiply and then 

    interpolate back again. 

  - 􏰀  The question is, are we quicker than the normal multiply?



### The Main Idea -Part 1

Develop two fast algorithms that for any polynomial: 

and a preselected set x0, x1, . . . , xn−1 of numbers (to be specified before 

we know which polynomials we will have), 

- 􏰀  Evaluate A(x0), A(x1), . . . , A(xn−1) (evaluate) 
- 􏰀  Given A(x0), A(x1), . . . , A(xn−1), reconstruct A’s coefficients a0,a1,...am−1 (interpolate) 



### The Main Idea -Part 2

The main steps for fast multiplication of two polynomials *A* and *B* each of degree *n* are: 

1. *Double degree-bound:* Create coefficient representations of *A*(*x*) and *B*(*x*) as degree-bound 2*n* polynomials by adding *n* high-order zero coefficients to each.
2. *Evaluate:* Compute point-value representations of *A*(*x*) and *B*(*x*) of length 2*n* through two applications of the FFT of order 2*n*.
3. *Pointwise multiply:* Compute a point-value representation of *C*(*x*) = *A*(*x*)*B*(*x*) by multiplying the values pointwise
4. *Interpolate:* Create a coefficient representation of *C*(*x*) through a single application of the *inverse* FFT. 



### Evaluation at Roots of Unity

- First let’s address evaluation: 

  - 􏰀  We need to evaluate a polynomial of degree *n* at *n* different points 

    (ignore the degree-bound doubling for the moment). 

  - 􏰀  Appears complexity of our method will be *O*(*n*2). 

  - 􏰀  Is there a faster way of doing this than just using Horner’s Rule ? 

- 􏰀  Yes there is, we select the points we evaluate at to be special. 

- 􏰀  These special points are chosen to be the *N*-th Complex Roots of Unity: 

  - 􏰀  Thatis,thevaluesω*N* =*e*2π*ij*/*N* for*j*=0,1,...,*N*−1. 

  - 􏰀  Say we are evaluating at *N* points so we take the *N*-th complex roots of 

    unity ω*N* . 

  - 􏰀  That is, we evaluate the polynomial at the points: 

    012 *N*−1 ω*N*,ω*N*,ω*N*,...,ω*N*.

- ⚠️ 这里是逆时针的 一般网上的是顺时针的

- 



### A Couple of Lemmas

*The Halving Lemma:* *IfN*>0*iseventhenthesquaresoftheNcomplex N-th roots of unity are the N*/2 *complex N*/2*-th roots of unity.* 

