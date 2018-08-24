---
layout:     post
title:      Haskell Notes
subtitle:   Hello Haskell
date:       2018-08-20
author:     EC
catalog: true
tags:
        - Haskell
	- 编程
	- English Posts
---

# Haskell  Notes



> Functional programming is about programming with functions as values. This emphasis leads to programs that are usually <u>concise</u>, <u>precise</u> and <u>reasonable</u>.



### 0.   Origin

The origins of functional programming are generally agreed to be the **Lambda calculus**, sometimes written **λ-calculus**. This was introducted by **Alonzo Church** in the **1930s**. The Church-Turing thesis showed that the λ-calculus is Turing complete.

The λ-calculus is made up of three components:

* Variables `x`
* Lambda abstraction `λ x. M`
* Application `MN`

Example: `(λ x. x) 3 = 3`

We will study Haskell as a modern language that implements the **λ-calculus**.

Haskell was originally conceived in around **1990**, and officially released in **1998**. It was based on another language called **Miranda**.

Here is some Haskell:

```haskell
>	fib :: Int -> Int					-- fib is function name
>	fib  0 = 1							-- Int -> Int is type signature
>	fib  1 = 1							-- pattern matching
>	fib  n = fib (n-1) + fib (n-2)		-- recursion
```



### 1.   Data & Types

##### Plain-Old-Datatypes (PoDs)

Haskell has a number of built-in datatypes, including ***Int, Char, Bool, Double, Integer,*** and ***Float***.

```haskell
5     	:: Int		5    :: Integer		-- Int is fast but inaccurate

True  	:: Bool		3.14 :: Double

False 	:: Bool		'a'  :: Char

"Hello" :: String						-- String is list of Char
```



Each of these definition are of the form:

```haskell
5	::	Int								-- 5 is value and Int is type
```

You can think of this as `5 ∈ Int`.

Every value has a **<u>*principal type*</u>**, which means there exists a most general type such that all other types for that value are an instance of the principal type. 



##### Int vs. Integer

There are two main ways that Haskell stores integers: ***Int*** and ***Integer***. Ints are **bounded** and **limited in size**; Integers are **unbounded** and **have unlimited size**. This most evident when working with really BIG numbers. You can give this a go right now by typing a really BIG number into GHCi and telling the compiler that it should have type Int: 

`9876543798976543245678 :: Int `

See how the compiler gives you a wee warning? That is because Ints are bounded, it will even have told you the range. But why use Ints then? Well Ints are faster and easier for the computer to work with. However, you should only use Ints when you are sure that the number will stay within the range. 



##### Data Constructor

We can construct more complex data types in Haskell by using the data constructor. A simple example is the Bool data type that you have already met. This can be defined as: 

`data Bool = False | True`



### 2.   Functions

Functions are mappings from values to values.

Example:

```haskell
f :: Integer -> Integer

f x = x * x							-- the x after f is parameter
```



### 3.   Evaluation

```haskell
>	square :: Int -> Int		> two :: Int -> Int
>	square x = x * x			> two n = 2
```

Evaluation is about **substitution**.

```haskell
square 5
= { def square }
	5 * 5
= { def *}
	25
```

##### Eager Evaluation & Lazy Evaluation

There are many ways of evaluating expresions,

but there are two main ones:

| Lazy evaluation  | Easer evaluation  |
| :--------------: | :---------------: |
|   Normal order   | Application order |
| outer most first | inner most first  |

Example:

```haskell
-- Eagar evaluation							-- Lazy evaluation
	two (square 3)								two (square 3)
   = { def square }							   = { def two }
   = two (3*3)								   = 2
   = { def * }
   = two 9
   = {def two}
   = 2
```

```haskell
-- Eager evaluation						   -- Lazy evaluation
	square (3 +1)							   square (3 +1)
  = { def + }								  = { def square }
  = square 4								  = (3 + 1) * (3 + 1)
  = { def square }							  = { def + }
  = 4 * 4									  = 4 * (3 + 1)
  = {def *}								  = { def + }
  = 16										  = 4 * 4
											  = { def * }
											  = 16
```



Which evaluation strategy should we use?

The **Church-Rosser Theorem** says:

1. If evaluation terminates, when applied to a given expression, then normal order will terminate.
2. If two evaluations terminate for a given expresiion, thenthey will agree on the result.

Example:

```haskell
	infinity :: Integer
	infinity = 1 + infinity
```

Let's evaluate **'two infinity'** and beyonds:

```haskell
-- Eager evaluation						   -- Lazy evaluation
	two infinity							   two infinity
  = { def infinity }						  = { def two }
  = two (1 + infinity)						  = 2
  = {def infinity}
  = two (1 + (1 + infinity))...
```



### 4.   Lists

A list in Haskell has special notation:

```haskell
[3, 1, 4, 1, 5] :: [Integer]	-- All the elements have the same type.
								-- We call theme homogenous lists.
```

Infinite lists are fine in Haskell. Here is some shorthand:

```haskell
nats :: [Integer]						[5 .. 10]
nats = [0..]						   =[5, 6, 7, 8, 9, 10]
```

So a consequence is:

`[5, 6, [7, 8]]` is **illegal**!

 `[[0, 1, 2], [3, 4]]` is **good**.

`[[True], [3, 4]]` is **bad**. (<u>Hint: look at the types</u>)

Lists are always constructed in one of two ways:

```haskell
* Empty Lists		[] :: [a]
* Consing			x : xs :: [a]		
       	     --(which means attaching an element to the front of a list)
```

For example, the list `[3, 1, 4]` is simply the following:

```haskell
	[3, 1, 4]
=	3 : [1, 4]
=	3 : 1 : [4]
=	3 : 1 : 4 : []
```

As a guide, always consider `[]` and `(:)` when defining functions on lists.

##### Head & Tail

The **head function** returns the **first element** of a list:

```haskell
>	head :: [a] -> a
>	head (x:xs) = x						--x is a here and xs is [a]
```

Notice that head is **naughty**! We did not define the case for empty lists!

The function tail returns the list after the first element.

```haskell
>	tail :: [a] -> [a]
>	tail (x:xs) = xs					-- NB [xs] is incorrect because
										-- [xs] ::[[a]]
```

Tail is also naughty (guess why?)

```haskell
>	length :: [a] -> Integer
>	length [] = 0
>	length (x:xs) = 1 + length xs
```

Another function a list is empty, which checks to see if a list is empty.

```haskell
>	isEmpty :: [a] -> Bool
>	isEmpty [] = True
>	isEmpty x:xs =False
```

Here is one more pattern matching:

```haskell
>	isSingle :: [a] -> Bool
>	isSingle []  = False
>	isSingle [x] = True					-- Or alternating isSingle(x:[])
>	isSingle (x:xs) = False				-- Or isSingle _ = False
```

Alternating

```haskell
>	isSingle :: [a] -> Bool
>	isSingle [] = False
>	isSingle (x:xs) = isEmpty xs		-- isEmpty function has been defined previously.
```

This function takes a number of elements:

```haskell
>	take :: Int -> [a] ->[a]
>	take 0 xs = []
>	take n [] = []
>	take n x:xs = x : take (n-1) xs
```

Homework: define `drop :: Int -> [a] -> [a]`, which drops the first n elements.

Example:

```haskell
>	drop 3 ['a', 'b', 'c', 'd', 'e'] = ['d', 'e']
```

```haskell
>	drop :: Int -> [a] -> [a]
>	drop 0 xs = xs
>	drop n [] = []
>	drop n x:xs = drop (n-1) xs
```

We also make use of a function that appends two lists together:

```haskell
>	(++) :: [a] -> [a] -> [a]
>	[] ++ ys = ys
>	x : xs ++ ys = x : (xs ++ ys)
```

This covers all cases: whether the second parameter ys is `[]` or `z:zs`, the right-hand side applies.

##### Pattern Guards

Here are some functions that make use of pattern guards:

```haskell
>	filter :: (a -> Bool) -> [a] -> [a]
>	filter p [] = []
>	filter p x : xs = if p x then x : filter p xs
>							 else	  filter p xs
```

This version of filter used an ***if-then-else*** conditional.

A pattern guard can make things nicer:

```haskell
>	filter :: (a -> Bool) -> [a] -> [a]
>	filter p [] = []
>	filter p (x : xs) |	p x       = x : filter p xs
					  | otherwise = filter p xs
					  -- Otherwise :: Bool
					  -- Otherwise =  True
```

Example of filter is:

```haskell
>	filter odd [1, 2, 3, 4, 5] = [1, 3, 5]
```

This uses the function `odd`:

```haskell
>	odd :: Integer -> Bool
>	odd 0 = False
>	odd n = not (odd (n-1))
```

Pattern guards shoud not be confused with constructor alternatives. For instance:

```haskell
>	data Day = Monday | Tuesday | Wednesday | ...
					-- These '|' tell Haskell that we want different data constructors.
```

or

```haskell
>	data Bool = True
>			  | False
```



### 5.   Lambda

> A lambda introduces a function with no name.

The syntax is this:

```haskell
(λ (BLACKBOX) -> (BLACKBOX))
  --  input         output
```

You can think of this as a **black box**. So, `λ x -> (x+1)^2 ` means:

```haskell
x -> (BLACKBOX) -> (x+1)^2
```

Alternative notation for `λx -> f x` is `x |-> f x` or `λx. fx`



##### Function Composition

Function composition is about putting functions together.

We plug the output of one function into the input of the other.

For instance:

```haskell
						> plus :: Int -> Int -> Int
						> plus x y = x + y
						> square :: Int -> Int
> f x = x^2 + 3			> square x = x * x
```

This is a combination of `plus 3` and `square`.

We can define `f` as follows:

```haskell
>	f x = plus 3 (square x)
```

This can also be written as a single function applied to `x`:

```haskell
>	f x = (plus 3 . square) x
```



##### Leibniz's Law

This law is used all the time in **algebraic manipulation**.

##### Composition

Earlier we saw that

```haskell
	f x = x^2 + 3		f = plus 3 . square
	g x = (x + 5)^2		g = square . plus 5
```

Notice that we can drop the `'x'` parameters.

Proof

```haskell
	g = square . plus 5
  => {--Leibniz law--}
  	g x = (square . plus 5) x
```

This is a cheeky application of Leibniz, where the LHS and RHS are functions to start with, and the function is `$x` , whichapplies `x` to each side.

The `$x` is a function that applies `x`. `$` is defined as follow:

```haskell
>	($) :: (a -> b) -> (a -> b)
>	f $ x = f x
```

**Composition** is also a function. It is defined as:

```haskell
>	(.) :: (b -> c) -> (a -> b) -> (a -> c)
>	(g . f) x = g (f x)
```



### 6.   Currying

> In haskell, ***every*** function takes in ***1*** argument, and outputs ***1*** value.

```haskell
>	add :: (Int, Int) -> Int
>	add (x, y) = x + y
```

To use this, we can write **add (3,4)**,

```haskell
>	plus :: Int -> Int -> Int
>	plus x y = x + y
										-- to use this, plus 3 4
										-- This could be written as Int -> (Int -> Int)
```

This means that we can **partially** apply functions.

For instance we might want this function:

```haskell
>	plusTwo :: Int -> Int
>	plusTwo y = 2 + y
```

We could alternatively say:

```haskell
>	plusTwo y = plus 2 y
```

Consider the type of plus 2, we have `plus 2 :: Int -> Int`.

We can convert between add and plus by using a method called currying (named after **Haskell B. Curry**).

The idea is that we want this equivalence:

```haskell
curry add = plus
add = uncurry plus
```

We can inplement curry and uncurry as functions.

```haskell
>	curry :: ((a, b) -> c) -> (a -> b -> c)
>	curry f x y = f (x, y)
									-- f is (a, b)-> c, x is a, y is b and f (x,y) is c
```

The reason `x : a` and `y : b` can be put on the left-hand side of `=` is that we can rewrite the type of curry:

```haskell
>	curry :: ((a, b) -> c) -> a -> b -> c
```

We could give a different definition **using λ**.

```haskell
>	curry f = λ x -> (λ y -> f(x, y))
```

or

```haskell
>	curry f = λ x y -> f(x, y))
```



##### Extensionality

Extensonality says that two functions `f` and `g` are equal if they agree on their output given any input.

```haskell
	f = λ x -> f x					-- Hint: 'λ' is '\'
```



##### Uncurrying

The inverse of **curry** is **uncurry**.

```haskell
>	uncurry :: (a -> b -> c) -> ((a,b) -> c)
> 	uncurry g = λ (x,y) -> g x y
```

or alternating:

```haskell
>	uncurry :: (a -> b -> c) -> ((a -> b) -> c)
>	uncurry g (x,y) = g x y
```

Let's try to show that **add** and **plus** are <u>roughly</u> equivalent.

We want to show that `plus x y = add (x, y)`.

Proof:

```haskell
>	  plus x y
>	= {def plus}
>	= x + y
>	= {def add}
>	= add (x,y)
```

Another interesting calculation converts the definition of plus.

```haskell
	plus x y = x + y
  ⇔ {extensionality}
  	plus x = λ x -> λ y -> x + y
  ⇔ {extensionality}
  	plus = λ x -> λ y -> x + y
  ⇔ {extensionality}
  	plus = λ x y -> x + y
```



### 7.   Maps

A map takes in a function f and a list xs, and produces a list where each element in xs is replaced by its **corresponding value**, manipulated by `f`.

For instance:

```haskell
			xs = [0, -2,  1, -2, 2]
				-- map (plus 3)
map (plus3) xs = [3,  1, -4,  1, 5]
```

The map function is defined as:

```haskell
>	map :: (a -> b) -> [a] -> [b]
>	map f [] = []
>	map f (x:xs) = f x : map f xs
```

Example.

Suppose we have a function that transforms numbers into characters.

```haskell
>	alpha :: Int -> Char
>	alpha 1  = 'a'				-- Exercise:
>	alpha 2  = 'b'				-- Come up with better definition.
>	...		  ...
>	alpha 26 = 'z'
```

Lets us calculate the result of map alpha [1, 3, 5].

```haskell
  map alpha [1, 3, 5]
= { def (:) }
  map alpha (1 : [3, 5])
= { def map }
  alpha 1 : map alpha [3, 5]
= { def alpha}
  'a' : map alpha [3, 5]
= { def (:) }
  'a' : map alpha (3: [5])
= { def map }
  'a' : alpha 3 : map alpha [5]
= { def alpha }
  'a' : 'c' : map alpha [5]
= { def map }
  'a' : 'c' : alpha 5 : map alpha []
= { def alpha }
  'a' : 'c' : 'e' : map alpha []
= { def map}
  'a' : 'c' : 'e' : []
= { def (:) }
  ['a', 'c', 'e']
```



### 8.   Comprehensions

##### List Comprehensions

List comprehensions provide useful syntax for building lists.

For instance, we might want the square of all the odd numbers:

```haskell
	[ x * x | x <- nats, odd x]
```

This assumes that nates = `[0..]` and `odd :: Int -> Bool ` that is **true** when the value given is **odd**.

The expression above is syntactic sugar for:

```
	map square (filter odd nats)
```



##### Operators vs Functions

Operators are made up of symbols like `+` , `*` , `\` , etc.

Functions start with lowercase letters, followed by other alphanumeric characters, or ''primes" ie.

Operator are **infix** by default, which means that we use them between two values. e.g. x ***+*** y.

Functions are **prefix** by default, so we write them before the argument. e.g. `fib 5`, `plus 3 7`.

An operator can be made prefix by surrounding it by parenthesis. e.g.

```haskell
		x + y = (+) x y
```

This explains why the type of addition is `(+) :: Int -> Int -> Int`

A function can be made infix by surrounding it by backtick quote marks. e.g.

```haskell
		mod 3 5 = 3 `mod` 5
```



##### Sectioning

Sometimes it is useful to supply only one argument to a binarry operation.

For instance here are some ways of defining plusOne:

```haskell
	plusOne :: Int -> Int
	plusOne x = 1 + x
```

This could instead be written as:

``` haskell
	plusOne :: Int -> Int
	plueOne = (1+)
```

Alternatively, because `+` is commutative,

```
	plusOne = (+1)
```

To understand these, we could write as follows.

```haskell
	(x ⊕) = λ y -> x ⊕ y
```

also,

```haskell
	(⊕ y) = λ x -> x ⊕ y
```

The parentheses are essential have sectioning. Does not work for functions that are in infix form, so "`mod 5`" and "`3 mod`" are **illegal**.

```haskell
>	mod :: Int -> Int -> Int
```

we can write:

```haskell
>	mod 3
```

This is the same as `λy -> mod 3 y`. If we want partial application on the second parameter, we need to ''modify" the function with **flip**:

```haskell
> flip :: (a -> b -> c) -> (b -> a -> c)
> flip f y x = f x y
```

So we have `flip mod 5 = λ x -> mod x 5 `



### 9.   Folds

##### Recursion Schemes: Fold

Recursion can be dangerous and so we use programming language constructs to control it. 

A **fold** takes a list and catastrophically works on it to produce a final value. Technically, they are called ***catamorphisms***.

A fold over a list can be demonstrated by the sum function:

```haskell
		1 : 2 : 3 : 4 : []
		  ↓	  ↓   ↓   ↓  ↓
		  +   +   +   +  0
```

A **foldr** starts on the right-hand-side of a list, and reduces it, by first modifying the empty list `[]`, and then adding elements, to  the solution by transforming the cons, `:`.

We need to provide **two** ingredient : a way of dealing with `[].` and another one for `:`.

Usually we have `k :: b` for replacing `[]`

​			and `f :: (a -> b -> b)` for replacing `:`

```haskell
> 	foldr :: (a -> b -> b) -> b -> [a] -> b
>	foldr f k [] = k
>	foldr f k (x:xs) = f x (foldr f k xs)
```

The function sum can be written without a foldr as follows:

```haskell
>	sum :: [Int] -> Int
>	sum [] = 0
>	sum (x:xs) = x + sum xs
			   = (+) x (sum xs)
```

As a fold, we give this definition:

```haskell
>	sum :: [Int] -> Int
>	sum xs = foldr (+) 0 xs
```

This is equivalent to the following:

```haskell
>	sum :: [Int] -> Int
>	sum = foldr (+) 0
```

Let's see an example of folding a list:

```haskell
	sum [3, 1, 4]
=	{ def sum }
	foldr (+) 0 [3, 1, 4]
=	{syntactic sugar}
	foldr (+) 0 (3: 1: 4: [])
=	{ def foldr }
	(+) 3 ((+) 1 (foldr (+) 0 (4: [])))
=	{ def foldr }
	(+) 3 ((+) 1 ((+) 4 (foldr (+) 0 [])))
=	{ def foldr }
	(+) 3 ((+) 1 ((+)4 0))
=	{def (+)}
	(+) 3 ((+) 1 4)
=	{def (+)}
	(+) 3 5
=	{ def (+)}
```



The foldr is a general scheme, and we can use it to define many other functions. For instance, consider product:

```haskell
>	product [1, 2, 3, 4] = product (1: 2: 3: 4: [])
						 =			1* 2* 3* 4* 1
```

with this intuition, we can define product easily:

```haskell
>	product :: [Int] -> Int
>	product = foldr (*) 1
```

Another example is length:

```haskell
>	length :: [a] -> Int
>	length = foldr (const (+1)) 0
```

```haskell
	length [True, True, False]
=	length (True: True: False: [])
			 ↓		↓	  ↓	    ↓
=			 +1		+1	  +1	0
```

Remember that `(+1) :: Int -> Int` is a sectioned `(+)`, so it has the wrong type for foldr. What we really want is `λ x y -> y + 1`, or `const (+1)` :

```haskell
> const :: a -> b -> a
> const x y =x
```

So `const (+1) :: b -> Int -> Int` ignores its first parameter and add one to its second.



### 10. Monoids

> A monoid is an operation that in associative and has a neutral element. For instance, + is a monoid, with neutral element 0. More generally, we can think of a monoid as an operation ⊕ with a neutral element ∅ , such that the following laws hold:

```haskell
	x ⊕ Ø = x				  Ø-right-identity
	Ø ⊕ y = y				  Ø-left-identity
	x ⊕ (y ⊕ z)=(x ⊕ y) ⊕ z   ⊕-associative
```

Here are some monodies:

|  m   |  ⊕   |  Ø   | foldr ⊕ Ø |
| :--: | :--: | :--: | :-------: |
|  ℕ   |  +   |  0   |    Sum    |
|  ℕ+  |      |      |           |
|      |      |      |           |
|      |      |      |           |
|      |      |      |           |
|      |      |      |           |
|      |      |      |           |
|      |      |      |           |
|      |      |      |           |
|      |      |      |           |
|      |      |      |           |



##### Type Classes

A type class defines a family of types, all related by their operations. Type Classes are usually defined in terms of their operations, and the laws that the operations must satisfy.

For monoids, we introduce a new typeclass as follows:

```haskell
class Monoid m where
>	mappend :: m -> m -> m	-- ⊕
>	mempty  :: m			-- ∅
```

A valid instance of the Monoid class respects the monoid laws.

Here is how we would write an instance of this class.

```haskell
>	instance Monoid Int where
>		mappend = (+)
>		mempty  = 0
```

This allows us to define a generic operation like crush, as follows:

```haskell
>	crush :: Monoid m => [m] -> m
>	crush = foldr mappend mempty
```

The problem with the monoid instance above is that it is only valid for Int, and we cannot have another instance for Int'. For example we might want to have multiplication as the monoid.

To solve this we will use a newtype constructor. This introduces a new type that behaves exactly like Int, but it is of a different type.



##### Newtypes

To introduce a newtype in Haskell we write:

```haskell
newtype PInt = PInt Int
```

To convert from **Int** to a **PInt**, we simply use the constructor **PInt**:

```haskell
> toPInt :: Int -> PInt		-- this is therefore redunctant
> toPInt  x = PInt x		-- since we can use PInt
```

To convert the other way around:

```haskell
> fromPInt :: PInt -> Int	-- this is a deconstructor
> fromPInt (PInt x) = x		-- for PInt
```

Because we have that PInt is a newtype of Int, we can assume that:

```haskell
> toPInt . fromPInt = id
> fromPInt . toPInt = id
```

The functions **toPInt** and **fromPInt** witness that PInt 约等于 Int.

An alternative notation for newtype automatically introduces the deconstructor:

```haskell
>	newtype PInt = PInt { unPInt :: Int}
```

So note that `unPInt :: PInt -> Int`.

To use this newtype for the product monoid, we treat it like any other type:

```haskell
> instance Monoid PInt where
> 	--mappend :: PInt -> PInt -> PInt
>  	  mappend (PInt x) (PInt y) = PInt (x * y)
> 	--mempty :: PInt
>     mempty = PInt 1
```

The following crush will have the right type:

```haskell
> crush :: [PInt] -> PInt
```

However we really wanted a function of type `[Int] -> Int`. We will achieve this as follows:

```haskell
		[Int]
		  ↓		map PInt		-- This is what we
		[PInt]					-- want, and it is code!
		  ↓		crush			--
		 PInt					-- > unPInt.crush. map PInt
		  ↓		unPInt
		 Int
```



##### Type Classes II

There are lots of type classes in Haskell. Here are some from Prelude:

```haskell
> class Eq a where
> (==) :: a -> a -> Bool
```

For instance, equality on Bool is as follows:

```haskell
>	instance Eq Bool where
>  	--(==) :: Bool -> Bool -> Bool
>		True  == True  = True
>		False == False = True
>		-	  == -     = False
```

Haskell is able to derive the Eq type class by itself. For instance, we might have had this is the Prelude instead:

```haskell
>	data Bool = True | False
>		deriving Eq
```

Another useful type class is Num, which allows us to write numbers:

```haskell
>	class Num a Where
>		(+) :: a -> a -> a
>		(-) :: a -> a -> a
> 		(*) :: a -> a -> a
>		negate :: a -> a -> a
>		abs :: a -> a -> a
>		signum :: a -> a -> a
>		fromInger :: Int -> a
```

This is implemented for `Int`, `Integer`, `Float`, `Double`.

Adiitioanlly there is a class called show, which turn input into a string.

```haskell
>	class Show a where
>		show :: a -> String
```

It is conventional (but not required) that `Show` produces the same output as the constructors,but as a **string**. For example:

```haskell
>	instance Show Bool where
>		show True  = "True"
>		show False = "False"
```



##### Deriving Folds

The definition for folds can usually be derived in a fairly automatic process, starting from an example.

One task is to define `group :: Eq a => [a] ->[[a]]` as a **foldr**. For instance,

```haskell
group [1, 2, 2, 3] = [[1], [2,2], [3]]
```

It has the property that `concat (group xs) = xs`.

To define this as a foldr, we can do this by example:

```haskell
	group [1, 2, 2, 3]
= foldr f k 	(1 : 2 : 2 : 3 : [])
			   f 1(f 2(f 2(f 3   k))) 
```

```haskell
						   f 3   k = [[3]]
					   f 2[[3]]    = [[2],[3]]
				   f 2[[2], [3]]   = [[2,2],[3]]
			   f 1[[2, 2],  [3]]   = [[1],[2,2],[3]]
```

The type of f is clear from the definition of **foldr**.

```haskell
	f :: Eq a => a -> [[a]] ->[[a]]
	f x ((y : ys) : yss)
		| x == y    = (x : y: ys):yss
		| otherwise = ([x]:(y:ys)):yss
	f x []				= [[x]]
	k :: [[a]]	k = []
```



##### Maybe

Some datatypes contain other volumes. An example is lists. Another example is Maybe a:

```haskell
>	data Maybe a
>		= Nothing
>		| Just a
```

This is parameterised by `a`, which means that we have defined a **family of types**: a type for each type. For example, `Maybe Int`, `Maybe Bool`, `Maybe [Int]`, `Maybe [Maybe Int]`, and so on.

It may be useful to change the contents of a `Maybe` by applying function. This is similar to `map` for `lists`.

```haskell
>	mapMaybe :: (a -> b) -> Maybe a -> Maybe b
>	mapMaybe f	Nothing = Nothing
>	mapMaybe f  (Just x) = Just (f x)
```

This datatype allows us to define safe version of functions that may not return the promised values. For instance consider `head :: [a] -> a` and `tail :: [a] -> [a]`.

```haskell
>	headMay :: [a] -> Maybe a
>	headMay [] = Nothing
>	headMay (x:xs) = Just x
```

To define a safe recursion of `tail` we do this:

```haskell
>	tailMay	:: [a] -> Maybe [a]
>	tailMay [] = Nothing
>	tailMay (x:xs) = Just xs
```



### 11. Trees

There are many different strapped trees. Arguably, lists and Maybe values are trees too. Two main tree types are **Tree** and **Bush**.

A **Tree** has values at its **leaves**, and **fork** into two:

```haskell
		 ·			-- tree
		/  \
	   ·    ·		-- dot is a fork
	  /\    /\		-- num is a leaf
	 3  5  ·  2
	      /\
	     6  8
```

A **Bush** has values at its **nodes**, but not at the tips.

```haskell
		3
	   / \
	  4   7
	 / \ / \		-- num is node
	 2 · · ·		-- dot is tie
	/ \
	· ·
```

In Haskell we define these structures as follows:

```haskell
>	data Tree a					>	data Bush a
>	= Leaf f					>	= Tip
>	| Fork (Tree a)(Tree a)		>	| Node (Bush a) a (Bush a)

```

A small example of a tree is:

```haskell
		·
	   / \
	   ·  5
	  / \
	 3   4			-- Fork(Fork(Leaf 3)(Leaf 4))(Leaf 5)
```

A bush example:

```haskell
		3
	   / \
	  4   ·
	 / \
	·   ·			-- Node (Node Tip 4 Tip) 3 Tip
```



##### Mappings

We might want to transform the contents of a Tree or a Bush by using a function. This is the same idea as `map` for lists, or `mapMaybe` for Maybe.

```haskell
>	mapTree :: (a -> b) -> Tree a -> Tree b
>	mapTree f (Leaf x) = Leaf (f x)
>	mapTree f (Fork l r) = Fork (mapTree f l) (mapTree f r)
```

Now we define a map function for Bush:

```haskell
>	mapBush :: (a -> b) -> Bush a -> Bush b
>	mapBush f (Node l x r) = Node (mapBushe f l) (f x) (mapbush f r)
```



### 12. Functor

A functor is a structure whose content can be changed. Notice that lists, Maybes, Trees, and Bushes all have something in common; they have a map function:

```haskell
		map ::	(a -> b) ->       [a] ->       [b]
   mapmaybe	::	(a -> b) -> Maybe [a] -> Maybe [b]
    mapTree	::	(a -> b) -> Tree   a  -> Tree   b
    mapBush	::  (a -> b) -> Bush   a  -> Bush   b
```

All of these maps are generalised into map:

```haskell
	fmap :: (a-b) -> f a -> f b
```

The fmap function is a member of the Functor type class:

```haskell
>	class Functor f where
>		fmap :: (a -> b) -> f a -> f b
```

There are two laws associated to the functor class:

1. Functor **identity**

   `fmap id = id`

2. Functor **composition**

   `fmap g. fmap f = fmap (g.f)`

So since we have a type class, there should be instances of this, one for each of the relevant type.

```haskell
>	instance Functor [] where
>		fmap f []	  = []
>		fmap f (x:xs) = f x : fmap f xs
```

Or, alternatively:

```haskell
>	fmap = map
```

Similarly for the others:

```haskell
>	instance Functor Tree where
>		fmap f (Leaf x)   = Leaf (f x)
>		fmap f (Fork l r) = Fork (fmap f l) (fmap f r)
```

Or, alternatively:

```haskell
>	fmap = mapTree
```

For intuition consider this: `fmap double. fmap succ`

```haskell
	 ·					·				   ·
	/ \	  fmap succ	   / \	fmap double	  / \
   3   ·  --------->  4   ·	 --------->  8   ·
	  / \				 / \				/ \			
	 4   5				5   6			   10 12
				-- double x = 2 * x
				-- succ   x = x + 1
```



##### Finding Elements

Here is a function that tells you if an element is prest in a list:

```haskell
>	elem :: Eq a => a -> [a] -> Bool
>	elem x [] = False
>	elem x (y:ys)
>		 |	x == y 		= True
>		 |	otherwise	= elem x ys
```

Finding the element takes at best `1 == operation` and at worst `n == operations` (if the list is of length `n`), an average something like `n/2` steps. We write this as O(n). 

We will try to improve this by using a Bush instead. To do this we create an ordered bush, where elements to the left are smaller than those to the right:

```haskell
				5
			  /	  \
			1		6
		   / \	  /	  \
		  ·   ·  5    39
		  		/ \  /  \
		  	   ·  · 38   ·
		  	   	   /  \
		  	   	  9   38
		  	   	 / \  / \
		  	   	·   ··   ·
```

We build a sorted bush by repeatidly inserting; and this required us to have a way to compare elements, which can be done with `Ord`, a class that contains the operations `<`,`>`,`≤`,`≥`.

```haskell
>	insert :: Ord a => a -> Bush a -> Bush a
>	insert  x Tip = Node Tip x Tip
>	insert x (Node l y r)
>		|  x < y  = Node (insert x l)
>		|  x ≥ y  = Node (insert x r)
```

Now that we know how to insert a single element into a sorted bush, we have everything we need to convert a list of elements into a Bush.

```haskell
>	toBush :: Ord a => [a] -> Bush a
>	toBush  = foldr f k where			-- This is the very
>	-- f :: a -> Bush a -> Bush a	-- verbose version. it
>	f = insert						-- is equivalent to:
>	-- k :: Bush a				-- toBush = foldr insert Tip
>	k = Tip
```

Now we can implement an algorithm to see if an element is in the Bush.

```haskell
>	elemBush :: Ord a => a -> Bush a -> Bool
>	elemBush x Tip = False
>	elemBush x (Node l y r)
>		| x  < y = elemBush x l
>		| x == y = True
>		| x  > y = elemBush x r
```

If the bush that we have created is fully balanced, and contains n elements, then we have at worst `log_2 n` comparisons, so we would say that this has complexity `O(log n)` under these conditions.

##### Generic Folding

The foldr function for lists captured many useful algorithms. We can derive similar functions for other datatypes such as `Maybe a`, `Tree a`, and `Bush a`.

Where does the strucutre of the foldr function come from?

```
>	foldr :: (a -> b ->b) -> b -> [a] -> b
```

Looking at this signature, the last part `[a] -> b` is fixed. We might ask ourselves where `(a -> b -> b)` and `b` come from. Let's see the structure of lists.

There are two constructors:

```haskell
	[] 	:: [a]
	(:)	:: a -> [a] -> [a]
```

These two constructors correspond to the two arguments of `foldr`, where `[a]` has been replaced with`b` 

Let's redefine `foldr` where we name the variables something that remind us of this fact:

```haskell
>	foldr :: (a -> b -> b) -> b -> [a] -> b
>	foldr cons empty [] 	= empty
>	foldr cons empty (x:xs) = cons x (foldr cons empty xs)
```

It turns out that we can also fold a maybe:

First, we look at the structure of Maybe:

```haskell
>	data Maybe a = Nothing | Just a
```

This has introduced the two constructor functions called `Nothing` and `Just`.

The types of thse functions are:

```haskell
	Nothing :: Maybe a
	Just	:: a -> Maybe a
```

Now we can define foldMaybe:

```haskell
>	foldMaybe :: b -> (a -> b) -> Maybe a -> b
>	foldMaybe nothing just	Nothing = nothing
>	foldMaybe nothing just	(Just x) = just x
```

So now let's apply this to folding a Tree.

The types of these constructor is:

```haskell
	Leaf	:: a -> Tree a
	Fork	:: Tree a -> Tree a -> Tree a
```

So now we use these signatures, where`Tree a` is `b` for the signature of our `fold`:

```haskell
>	foldTree :: (a -> b) -> (b -> b -> b) -> Tree a -> b
>	foldTree leaf fork (Leaf x)   = leaf x
>	foldTree leaf fork (Fork l r) =
			fork (foldTree leaf fork l)(foldTree leaf fork r)
```

Now with `Bush`:

```haskell
>	data Bush a = Tip
>				| Node (Bush a) a (Bush a)
```

Step 1: What are the types of the constructors?

Step 2: What is the type of `foldBush`?

Step 3: What is the definition of foldBush?

1. The types of the constructors are:

   ```haskell
   	Tip  :: Bush a
   	Node :: Bush a -> a -> Bush a -> Bush a
   ```

2. The type of `foldBush` is:    3. The definition too:

   ```haskell
   >	foldBush :: b -> (b -> a -> b -> b) -> Bush a -> b
   >	foldBush tip node Tip = tip
   >	foldBush tip node (Node l x r)
   >		= node (foldBush tip node l) x (foldBush tip node r)
   ```

   Suppose we want to find the number of elements in a Bush, then this can be defined with a `fold`:

   ```haskell
   >	sizeBush :: Bush a -> Int
   >	sizeBushe = foldBush tip
   >		 tip :: Bush a -> Int
   >		 tip  = 0
   >		node :: Int -> a -> Int -> Int
   >		node l x r = l + 1 + r
   ```

   Now we try to find the height of a bush:

   ```haskell
   >	heightBush :: Bush a -> Int
   >	heightBush = foldBush tip node where
   >		tip :: Int
   >		tip = 1
   >		node :: Int -> a -> Int -> Int
   >		node l x r = 1 + max l r
   ```

   A harder example is `fromBush :: Bush a -> [a]`

   ```haskell
   >	fromBush :: Bush a -> [a]
   >	fromBush = foldBush tip node where
   >		tip :: [a]
   >		tip = []
   >		node :: [a] -> a -> [a] -> [a]
   >		node ls xrs = ls ++ [x] ++ rs
   				or:	  ls ++ x:rs
   ```

   The definition of `fromTree` is similar:

   ```haskell
   >	fromTree :: Tree a -> [a]
   >	fromTree  = foldTree leaf fork where
   >		leaf :: a -> [a]
   >		leaf  x = [x]
   >		fork :: [a] -> [a] -> [a]
   >		fork ls rs = ls ++ rs
   ```

   We can use this to define `sizeTree`:

   ```haskell
   >	sizeTree :: Tree a -> Int
   >	sizeTree = length . fromTree
   			Int <- [a] <- Tree a
   ```

   

### 13. Map

Maps are datastructures that hold keys and their associated values. Maps will be our first example at abstract data type, where we do not have access to the internal structure. All we have are the operations.

For intuition, we can think of a Map as a function of the type:

```haskell
		k -> Maybe V		-- k is keys and v is values
```

In other words, we provide a Map with a key `k`, and we maybe got a value back. The classic example isa phone book.

The type of a map is : `Map k v` .(In practice, we import them from the `Data.Map`).

The most basic map operation returns the empty map:

```haskell
>	empty :: Map k v
```

We can extract values from a map using the lookup function:

```haskell
>	lookup :: Ord k => k -> Map k v ->Maybe v
```

We can understand the operations by their interactions:

for instance, we have:

`lookup k empty = Nothing for all k.`

We also have a way to insert values into the Map:

```haskell
>	insert :: Ord k => k -> v -> Map k v ->Map k v
```

We expect that the following holds:

```haskell
lookup k (insert k v m) = Just v
```

This tells us that if we insert a key into the map that already exsits, then we overwritethe old value.

### 14. Tries

A Trie structure is very efficient for mapping keys that can decomposed into lists.

Here is a Trie structure for names:![Trie](/Users/elvischen/Desktop/Haskell/Trie.jpg)

We can implement a `Trie` using a `Map`.

```haskell
>	data Trie k v = Trie (Maybe v)(Map k (Trie k v))
```

We can think of a `Trie k v` as being a `Map [k] v`.

A Trie can be empty:

```haskell
>	empty :: Trie k v
>	empty =  Trie Nothing Map.empty
							-- this is the empts
```

We can have a single element Trie:

```
>	single :: v -> Trie k v
>	single x = Trie (Just x) M.empty
```

You will notice that we have been reusing names such as empty (late we will reuse lookup and insert). To avoid name clashes we import external modules in a qualified way. The following line would be at the top of the file:

```haskell
import Data.Map qualified as M
```

The next function we define allows us to look up values.

```haskell
>	lookup :: Ord k => [k] -> Trie k v -> Maybe v
>	lookup [] (Trie mv kvs) = mv
>	lookup (k:ks) (Trie mv kvs) =
>		case Map.lookup k kvs of
>			Nothing -> Nothing
>			Just ts	-> lookup ks ts
```







