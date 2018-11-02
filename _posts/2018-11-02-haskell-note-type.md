---
layout:     post
title: "Haskell 数据类型"
subtitle:   "Haskell Note I - data types"
date:       2018-11-02
author:     "EC"
header-img: "img/post-abstract-1.jpg"
catalog: true
tags:
 - Haskell
 - 笔记
---

### Types

What are types? Types are how you describe the data you will work with. They allow you to classify parameters of functions. This should help you when designing functions: whenever you want to make a function always think about what its type should be. Before you give the definition of a function you normally write the type signature using the `::` notation. For example,

```haskell
lion :: BigCat
```

is like the function introducing itself and saying, “*I am lion and I have type BigCat*”.

The Haskell compiler is very passionate about its types. Knowledge and understanding of types should be integral in your implementation process, helping you get to solutions quicker, and allowing you to get on with the compiler well. BEWARE running blindly into smashing out a definition without considering types will lead to a compiler strop, and who can blame it? Seriously though, considering types allows you to improve your development practice. The typed nature of Haskell also means that most errors are found at compile time as opposed to run time, meaning that software written in Haskell is usually more reliable than that of other languages.

Types also have a really cool relationship to algebra and this short [article](https://codewords.recurse.com/issues/three/algebra-and-calculus-of-algebraic-data-types) explains it really well.

### PoDs

The ***plain old data types***, know more formally as ***primitive data types***, in Haskell are `Int`, `Char`, `Bool`, `Double`, `Integer`, and `Float`. These are your most basic building blocks in Haskell. Numbers can be stored as `Ints` or `Integers` if they are whole, and `Doubles` or `Floats` if they are fractional. Likewise, characters will be stored as `Chars`, and Booleans will be stored as `Bools`.

### Int vs. Integer

There are two main ways that Haskell stores integers: `Int` and `Integer`. `Ints` are bounded and **limited in size**; `Integers` are unbounded and have **unlimited size**. This most evident when working with really **big** numbers. You can give this a go right now by typing a really **big** number into GHCi and telling the compiler that it should have type `Int`:

```haskell
	9876543798976543245678 :: Int
```

See how the compiler gives you a wee warning? That is because `Ints` are bounded, it will even have told you the range. But why use `Ints` then? Well `Ints` are faster and easier for the computer to work with. However, you should only use `Ints` when you are sure that the number will stay within the range.

### Data Constructors

We can construct more complex data types in Haskell by using the data constructor. A simple example is the Bool data type that you have already met. This can be defined as

```haskell
	data Bool = False | True
```

This says that the type `Bool` is defined as being either `False` or `True` (or the `|` symbol).

In other words, `Bool` is a type that can only be two values. Of course, data types can have more than two constructors, for example, the question sheet defines the data type of `Colour` and dictates that a `Colour` can only be one of seven colours of the rainbow. Equally if you wanted to create a data type to describe the different positions in Quidditch you could write something like this:

```Haskell
	data QuidditchPlayer = Keeper | Chaser | Beater | Seeker
```

All this is saying is: “If you are a `QuidditchPlayer` you will either be a `Chaser`, a `Keeper`, a `Beater`, or a `Seeker`”, or thinking about it the other way round: “If you are a `Keeper` you are a `QuidditchPlayer` (`Keeper :: QuidditchPlayer`)”. The same could be done for ice cream flavours. You can define a data type for any discrete collection you want.

Here we are only scratching the surface of the power of making your own data types since there are many more possibilities that will take you beyond these trivial uses.

### Common Structures: Tuples and Lists

Tuples are a data structure that allows you to collect together elements of different types. You can have any type you want, for example, if you wanted to represent a playing card you could create a pair tuple:

```haskell
	favCard :: (Char, String)
	favCard = ('Q', "Clubs")
```

Tuples have a fixed size after creation and the constructor for a tuple is `(, )`. Having one comma will make you a pair tuple, adding more commas will create bigger tuples with more slots for all your favourite data types (but the max tuple size is 62):

```haskell
	(, ) :: a -> b -> (a, b)
	(, , ) :: a -> b -> c -> (a, b, c)
	(, , , ) :: a -> b -> c -> d -> (a, b, c, d)
```

The type of the pair tuple constructor `(, )` says “you gimme an `a`, and a `b` and I’ll bung them in a tuple for you”. It is like ordering lots of presents for yourself from the same online shop and having them sent to you in the one parcel to save on postage. For bigger tuples, the alphabet is used to enumerate subsequent slots you wish to have in your tuple. What happens when the alphabet runs out though?! Don’t worry you aren’t limited to 26 slots in a tuple, Haskell has an inspired naming system for slots after slot z. If you’re curious just ask GHCi for the type of `(, , , , , , , , , , , , , , , , , , , , , , , , , , )`.

**Lists** are a data structure that you will become very familiar with. You will use them literally all the time. We love lists. Lists are the best. Lists can be of infinite size, so if you ever want to freak out the general public: style your terminal with a black background and coloured font (something I hope you have already done like seriously if you have black font on white what are you doing with your life?), put your terminal full screen, open GHCi, type `[1 . .]`. All `[1 . .]` is an infinite list of numbers starting from one **but** your terminal will start spitting out these numbers onto the terminal as fast as it can. Looks like some serious hackery is going down.

```haskell
	data [ ] a = [ ]
			   | a : [a]
```

where:

```haskell
	[ ] :: [a ]
	(:) :: a → [a ] → [a ]
```

The list data type has two <u>constructors</u>: a list can either be empty `[ ]`, or it can have an element (of type `a`) prefixed onto another list. The type of `[ ]` is hopefully fairly straight forward: it has type list of a, it just so happens that in this context the empty list constructor has been chosen. The type of `(:)` is a little more involved. It takes an element and a list and returns a new list. This new list is just the old list but with the element added to the head. As indicated by the `a`, all the elements must be of the same type.

### Universal Quantification

In Haskell, you can generalise types. If you label a parameter of type a it can represent any data type.

For example if you wanted to make a function that calculated the length of a list you wouldn’t make one for each different type of list, `[Int] -> Int | [Char] -> Int`

Instead you just write `[a] -> Int` meaning “*give me a list of anything and I will return you an `Int`*”. It is almost like a wildcard, if you define a function with these general types you’re basically saying that you don’t mind what type that parameter is. This allows you to create more general functions. These functions are polymorphic since they are not specific about what data types they take in.

### Type Classes

We already know that when declaring the type of a function you can either be super specific by giving the exact data type of a parameter (e.g. `Int` or `Char` etc.); or you can be super general using universal quantification, but what if you want a half-way house? Say you are making a function that two adds numbers. You don’t care whether the numbers are stored as `Ints` or `Doubles` you just want to add them. If you chose the specific option and define `add :: Int -> Int -> Int` it will only work on Ints; likewise you also hit trouble if you take the general route: you can’t define `add :: a -> a -> a` what if someone gives you Chars? How does one even add characters?

Luckily the clever people at Haskell have gifted us **type classes**. You can think of type classes as families of types. Each family has its own rules and to be a member of a family a type must possess certain qualities. Once a member of a class the type must fulfil the promises of the type class. Each type class is defined with certain functions that any member must be able to perform. The Num class, for example, is the perfect class to solve our specificness problem. It is the class of types that basic arithmetic (add, subtract, multiply, negate) can be performed on:

```Haskell
    class Num a where
        (+) :: a -> a -> a
        (-) :: a -> a -> a
        (*) :: a -> a -> a
        negate :: 	a -> a
```

Now we can define `add` so that it will work on any numeric `a`:

```haskell
	add :: Num a => a -> a -> a
	add x y = x + y
```

Notice how the class only gives the type signatures of the functions it wants its members to be able to perform. That is because every type is different and unique. I don’t write letters in the same way that you write letters, similarly `Ints` will add themselves differently from `Doubles`. When a type joins a type class we say that it is an **instance** of that class. It is at this moment that the definitions for these functions are provided. You can think of this as a sort of initiation.

So let’s make a new data type and take it through a fresher fair style tour of which type classes it could join.

```Haskell
data Fresher = CompSci | Maths
```

Our bright eyed new type looks about at all the type classes it could join. It could join the `Eq` type class and learn how to compare different constructors of Freshers, learn that a `Maths` student is not the same as a `CompSci` student. It could join the `Show` society: a type class obsessed with showing off, requiring all its members to be confident enough to print out results onto the terminal. Then you’ve got the `Ord` type class where it could once and for all be decided which degree is the best. Don’t forget the mythical Monad class, a class from legend that will only reveal itself to the truly worthy.

Now like the dutiful caring parents we are: let’s help our type class initiate itself into the `Show` type class so that it can introduce itself.

```Haskell
    class Show a where
        show :: a -> String

    instance Show Fresher where
        show ComSci = "Hello I am a computer scientist!"
        show Maths = "Hi I study maths!"
```

Now GHCi knows that if it wants to print out a `Maths` Fresher it needs to print “`Hi I study maths!`”.