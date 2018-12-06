---
layout:     post
title:      Haskell 笔记 I
subtitle:   Haskell Note 1 - PoD, Evaluation & Currying
date:       2018-11-18
author:     "EC"
header-img: "img/post-abstract-3.jpg"
catalog: true
comments: false
guid: urn:uuid:bd69ccfc-e840-4bc4-99ec-54cefac7f749
tags:
    - Haskell
---

<p style="color: grey;font-family: Avenir;font-weight:600;">
    Note 10/01, Week 1 Lecture 1 (1)
</p>
<p style="font-size: 2em;font-family: Avenir;font-weight:600;text-align: center">
    Functional Programming
</p>

<div id="top">
<blockquote>
    The <a href="#LC03">purpose</a> of abstraction is not to be vague, but to create a new semantic level in which one can be <em>absolute</em> <b>precise</b>.
</blockquote>
<p style="text-align: right"> - Edsker Dijkstra</p>
</div>

**Functional Programming** is about programming with *values* rather than *statements*.

It encourages code that is:

- consice
- precise
- reasonable

Here is an example of code in Haskell:

```haskell
	fib :: Integer -> Integer
	fib		  0    =     1
	fib		  1    =     1
	fib       n    =   fib (n-1) + fib (n-2)
```

To understand this code, we evaluate it:

```haskell
	fib 2
= { def fib }
	fib (2 - 1) + fib(2 - 2)
= { def (-) }
	fib 1 + fib 0
= { fib 1 }, { fib 0 }
	1 + 1
= { def (+) }
	2
```

---
---

<p style="color: grey;font-family: Avenir;font-weight:600;">
    Note 10/03, Week 1 Lecture 2 (2)
</p>

### PLAIN OLD DATATYPES

**Plain old datatypes** (PoDs) are the building blocks of many values.

```haskell
	5 :: Integer	-- this is expressing that 5 is a value
					-- from the set Integer.
```

Numbers in Haskell are overloaded[^2.1] so:

```haskell
	5 :: Int	5 :: Float	5 :: Double	
```

Other values are:

| Character     | String[^2.2]        | Boolean        |
| ------------- | ------------------- | -------------- |
| `'e' :: Char` | `"elvis" :: String` | `True :: Bool` |

The smallest type is `Void`. It corresponds to a type for the empty set. There are no values, except for `⊥` .

`⊥` is pronounced "*bottom*". It expresses an **undefined** value. It is written:

```haskell
	undefined :: a
```

The type with one element is called ***unit***.

In Haskell we express the value of unit as `()` the type is also written as `()`.

```haskell
	() 		:: 		()
  --value  		 --type
```
One thing we can do is add types ***together***.

We can define a type with 7 elements in it, one for each day of the week as follows:

```haskell
	data Day = Monday
			 | Tuesday
			 | Wednesday | Thursday | Friday
			 | Saturday | Sunday
			 -- Constructors 用大写区分， functions 用小写
```

This introduces ***constructors*** such as `Monday`, with **type** Day.

```haskell
	Wednesday :: Day
```

We can make a type with a value by adding the type **`Bool`** to the type **`Day`**. This is achieved with **`Either`**:

```haskell
	data Either a b = Left a | Right b
```

This has introduced two constructors:

```haskell
	Left 	:: a -> Either a b
	Right 	:: b -> Either a b
```

Together with **`Bool`** and **`Day`**, we have introduced **`Either Bool Day`** which is a type with a value. They are the following.

```haskell
	Left True	 :: Either Bool Day
	Left False	 :: Either Bool Day
	Right Monday :: Either Bool Day
	...
	Right Sunday :: Either Bool Day
```

Pairs correspond to multiplications.

```haskell
	data (a, b) = (a, b)
```

So this gives us:

```haskell
	(True, Monday)  :: (Bool, Day)
	(True, Tuesday) :: (Bool, Day)
	...
	(False, Sunday) :: (Bool, Day)
```

### Functions

You will be familiar with functions in mathematics such as:
$$
f(x)=x^2
$$
In Haskell we might write this as follows:

```haskell
	square :: Int -> Int
	square x = x * x
```

A function takes input values to output values.

Every function in Haskell takes exactly one input type to one output type.

The type of **`square`** is **`Int -> Int`**. The type of **`square 5`** is just **`Int`**.

Here is another function:

```haskell
	two :: Int -> Int
	two	    n  =   2
```

This return 2 regardless of the input. `two 5 = 2` `two undefined = 2`[^2.3]

---

<p id="LC03" style="color: grey;font-family: Avenir;font-weight:600;">
    Note 10/05, Week 1 Lecture 3 (3)
</p>


We can combine functions by applying one followed by the other.

e.g.

```haskell
	square (two 5)			|	two (square 5)
= { square }				| = { two }
	two 5 * two 5			| 	2
= { two }					|
	2 * 2					|
= {(*)}						|
	4
```

We could combie these functions into one manually:

```haskell
	squareTwo :: Int -> Int
	squareTwo n = square (two n)
```

Or

```haskell
	twoSquare :: Int -> Int
	twoSquare n = two (square n)
```

Doing this is painful. We need a way to **compose functions** [^3.1]with an operation:

```haskell
	(.) :: (b -> c) -> (a -> b) -> (a ->c)
	(g . f) x = g (f x)
```

### Evaluation

The order of evaluation for functions is important. There are two main strategies:

- normal evaluation (lazy evaluation)
  - this evaluates the *<u>outermost</u>* first before moving inwards.
- applicative evaluation (eager evaluation)
  - this evaluates the *<u>innermost</u>* parameters first, that assembles outwards.

e.g.

```haskell
		Normal | Lazy					|	Applicative | Eager
	square(two 5)						|		square(two 5)
= { def square }						|	= { two }
	two 5 * two 5						|		square 2
= { two }								|	= { square }
	2 * two 5							|		2 * 2
= { two }								|	= { (*) }
	2 * 2								|		4
= { * }									|
	4									|
```

Which strategy is better?

**Eager** appears to generate result faster. However, consider the following consider following program:

```haskell
	infinity :: Int
	infinity = infinity + 1
```

What is the result of **`square (two infinity)`**?

The calculation for this in a **`lazy`** setting is similar to before.

```haskell
		Lazy							|		Eager
	square (two infinity)				|		square ( two infinity)
= { square }							|	= { infinity }
	two infinity * two infinity			|		square ( two ( infinity + 1 ))
= { two }								|	= { infinity }
	2 * two infinity					|		square ( two ( (infinity + 1) +1)
= { two }								|	...
	2 * 2								|
= {	(*) }								| CTRL + c to kill it.
	4
```

**The Church-Rosses Theorem**

This theorem tells us important facts about evaluation strategies:

1. If evaluation of a term terminates then normal evaluation will terminate.
2. If two evaluation strategies terminate for a term then they agree on the result.

### Currying

In Haskell it is possible to take ***functions*** as input, and to product functions as output.

Consider two functions, expressing addition:

```haskell
	add :: (Int, Int) -> Int
	add (x, y) = x + y
```

```haskell
	plus :: Int -> (Int -> Int)
	plus x y = x + y
```

How does **`plus`** work? Consider **`plus 7`**.

```haskell
	plus 7 :: Int -> Int		-- plusSeven :: Int -> Int
	plus 7 y = 7 + y			-- plusSeven y = 7 + y
```

To use this, we might write:

```haskell
	plus 7 3 = 10
```

> A language that does't affect the way you think about programming is not worth knowing.

Currying allows us convert from functions that take pairs as input to functions that produce functions as output.

For example, currying allows us to take a function like **`add`**, and produce a function like **`plus`**. This works with any function with the type **`(a, b) -> c`**.

The function `curry` is defined as follows:

```haskell
	curry :: ((a, b) -> c) -> (a  -> (b -> c))
	curry f x y = f (x, y)
```

For example, `curry add` behaves just like `plus`.

```haskell
	uncurry :: (a -> (b -> c)) -> ((a, b) -> c)
	uncurry g (x, y) = g x y
```

For example, `uncurry plus` behaves just like `add`.

---

[^2.1]: Overloaded？
[^2.2]: String is equivalent to a list of characters. e.g. `"elvis" :: [Char]` 
[^2.3]: 不确定 ⊥ 怎么打，但 undefined 是可行的。
[^3.1]: this is called "function composition". you write it with a full stop.