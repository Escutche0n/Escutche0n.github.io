---
layout:     post
title:      Haskell 模式匹配
subtitle:   "Haskell Note II - Pattern Matching"
date:       2018-11-02
author:     "EC"
header-img: header-img: "img/post-abstract-2.jpg"
catalog: true
tags:
    - Haskell
    - 笔记
---

### Pattern Matching

**Pattern Matching** is the bread and butter of Haskell programming. If you are looking to define a function and you don’t know what to do - **pattern match**! All pattern matching consists of is going through case by case what you want your function to do. Normally this involves going through all the possible constructors of a data type. Observe:

```haskell
    germanTranslationRobot :: Bool -> String
    germanTranslationRobot False = "Falsch"
    germanTranslationRobot True = "Richtig"
```

Looking at the type signature of this function you can see that it takes in a `Bool` and returns a `String`. This function can only have two possible inputs: `True` or `False`, `Richtig` oder `Falsch`, making the pattern matching very simple. The first line of the defintion tells the function what to do if the input is `True`; the second line tells it what to output when it receives `False`.

Lines in a definition follow the same structure: name of function, inputs that you are pattern matching on, equals sign to seperate the context from the result, and an expression that tells the function what it’s actually going to do. The LHS of the equals is like the description of the case in which you want the matching code on the RHS to be executed. You are just comprehensively informing the function what you want it to do: “*germanTranslationRobot when you are given `True` could you please return the string `"Richtig"`. Thank you! :-)*”.

The above example shows pattern matching directly on constructor key words (in this case `True` or `False`) however you can also use the LHS of the equals to name the various input parameters so that you can refer to them easily on the RHS of the defintion where you will actually compute stuff with them. These are just names, you can chose any names you want.

```haskell
add :: Num a => a -> a -> a
add x y = x + y

plus :: Num a => a -> a -> a
plus water melon = water + melon
```

The above two functions do exactly the same thing, the only difference is that `add` uses more succinct and mathematical lables for the two inputs whereas plus is more fun and random. We like super short variable names because it’s far easier to type short things.

One pattern match that you will do so much that it will almost become part of your being is pattern matching on lists. Recall that lists have two constructors: `[ ]` and `(:)`. Since when you pattern match you always start with the base case the first line of a pattern match on lists will normally be telling the function what to do when it is given the empty list. The next line will say what to do with the rest of the list.

```haskell
sum :: Num a => [a ] -> a
sum [ ]      = 0
sum (x : xs) = x + sum xs
```

Hopefully the first line of the definition makes sense. sum is being told to return 0 if it is given `[ ]` as its input since well the sum of nothing is nothing. The second line is a little odd the first time you see it. `(x : xs)` just represents a list where you have called the first item `x` and the rest of the items `xs` so you can easily tell the function to add the first item to the sum of the rest of the items. Remember `x` and `xs` are just names. You could still write whatever you want, for example:

```haskell
sum (first : rest) = first + sum rest
```

Notice how `(x : xs)` is in brackets? This is so that the compiler can type check your pattern matching and know that `x : xs` is only refering to the **first parameter** of the function, something that will become more important when you progress to pattern matching on more than one input.

One final trick that you may see people use is the **underscore**. When naming inputs if there is an input that you don’t care about and don’t need to name since you are not doing to use it on the RHS you can just put an underscore as a place holder.

```Haskell
take :: Int -> [a] -> [a]
take [ ] = [ ]
take 0 = [ ]
take n (x : xs) = x : (take (n − 1) xs)
```

`take` is a function that takes in an `Int`, and a `list`, and produces a list, basically it plucks the first `n` elements of the inputted list and places then in the new list. The definiton of `take` is a good example of using the underscore. If `n = 0` we don’t care what the list is since we know that we are just going to return the empty list. Same with if the inputted list is empty. It doesn’t matter what `n` is since there is nothing to take from anyway.

### Guards

When you want to inspect the value of an input instead of just pattern matching on its constructors you should use guards.

```haskell
compare :: Eq a => a -> a -> String
compare x y
    | x ≡ y = "They are the same!"
    | otherwise = "Different"
```

So the syntax. As ever you put the name of the function so that it knows this part of the definition is for it. Then you give names to the parameters you are taking in, here we have just called them `x` and `y`. Unlike pattern matching you don’t then have an equals, instead you go to a newline and place a guard `|`. Here we return to the familiar pattern of the LHS of the equals describing the case in which you want the code on the RHS to execute. The LHS can be any logical expression involving your variables you want and each guard represent a different case.

### λ

Sometimes in Haskell you may want to whip up a function on the fly and not bother naming it. It is like when you are baking and you realise you need a component that you didn’t initally plan for and you have to quickly make some. For example, you just need some butter icing to sandwich together your cake. Now are you really going to stop and carefully write a recipe for butter icing and then diligently measure out the butter and the icing sugar? No you’re a confident baker you know how to throw together some butter icing! You are also a confident programmer and you can quickly make wee functions that perform simple tasks (obviously only do this for simple functions!!!). Llambdas just allow these little helper functions to take in parameters.

```haskell
	type Llama = String
	type HappyLlama = String

	feedLlamas :: [Llama ] -> [HappyLlama ]
	feedLlamas = map (λllama -> llama ++ " :-)")
	feedLlamas ["Adam", "Alessio", "Ibrahim", "Jamie", "Charlie"]
	= ["Adam :-)", "Alessio :-)", "Ibrahim :-)", "Jamie :-)", "Charlie :-)"]

```

The above is an example of how you could use llambdas to make a helper function. All this function is doing is taking in a list of `Strings` (or `Llamas`) and appending similie faces to each String to create `HappyLlamas`! It does this by mapping (`λllama -> llama ++ " :-)"`) across the initial list (`map` is just a function that applies a function to each element in a list). I could easily have created and named this function like so:

```haskell
    toHappy :: Llama -> HappyLlama
    toHappy llama = llama ++ " :-)"
```

The two methods are completely equivalent, I could interchange the lambda function and `toHappy` without changing what `llamaFarm` does. To type a lambda it is just a backslash `\`. The syntax is pretty similar to that of pattern matching except there is an arrow in the place of the equals, and before you the parameters you place a `λ`.

This is just a general introduction on how to use lambdas with a silly trival example. I’m really selling lambdas short since later on you will see that they allow you to do many fun and cool things. (Although personally llamaFarm is clearly the pinnacle of Haskell. I’ve truly found the best use of this programming language :-)

### Function Composition

Function composition is a fancy way of doing one function followed by another. You can think of it like a production line, where you feed the value in the far right and it gets operated on by each function in order from right to left and the final result is spat out.

```haskell
    (◦) :: (b -> c) -> (a -> b) -> (a -> c)
```

It utilises the `(◦)` function. It may seem annoying that it is `(b -> c)` then `(a -> b)` but there is a reason for this. It is because the second function is done first. For example:

```haskell
	productionLine :: Int -> Int productionLine = (+7) ◦ (∗2)
```

This function takes in an Int, which is fed first to the `(∗2)` machine, which spits out and answer that is in turn fed to the `(+7)` machine. So if I feed `3` to the productionLine it would get multiplied by `2`, giving `6`, then `6` would have `7` added to it resulting in `13`.

### Laziness

Much like how in the real world you have people that are lazy and people that are not, when it comes to evalution order in programming languages you have lazy languages and eager languages. Haskell is lazy. It won’t do something unless it absolutely has to. You can think of this much like students and lecturers:

```haskell
    student :: Question -> Solution
    
    lecturer :: Int -> [Question ] -> [Solution ]
    lecturer n xs = take n (map student xs)
```

Think of a `student` as a function that will turn a `Question` into a `Solution`, and think of a `lecturer` as a function that assigns a certain number `n` of `Questions (xs)` to student in order to get a list of Solutions. In the world of Haskell everyone is lazy so here is how the Solutions would be extracted. Say the lecturer is going to assign 3 Questions, i.e. n = 3. lecturer sees that n isn’t zero yet so it says to (map student xs) “Okay I need a Solution”, and the student reluctantly replies spitting out one Solution. Now n = 2, the lecturer needs more Solutions so it kicks the inside function of student demanding that they do more work, and this pattern continues until the lecturer is satifed. Like taking blood from a stone.

If this was not Haskell and we were working with an eager language things would be very different. The student would be a very keen bean and immediately do all the Questions in the list, regardless of how many the lecturer assigns. This seems every diligent but think about what would happen if the list of Questions was infinite. The function would never terminate. Our poor student would work themselves to the bone perpetually answering Questions. Not very healthy.

### Structural Inductive Proofs

Since Haskell is super cool and mathsy you can do mathematical things like proofs on functions. You can literally prove that your programs do what they should do. A structural inductive proof is very similar to a proof by induction. As ever you begin with a base case. For lists your base case will be the `[ ]`. Basically what you want to do is write the statement you want to prove with the `[ ]` inserted, then manipulate the expressions using the defintions of your functions until the LHS matches the RHS. Normally for the base case this is fairy easy.

The normal case will always be more difficult and you won’t be able to get the RHS to match the LHS only using the defintions of your functions, you will also need to use the `Induction Hypothesis`. This is the statement that you are tying to prove. Often I think I’m stuck in a proof since I’ve run out of definitions to use but actually all I need to do is apply the `Induction Hypothesis`.

Don’t worry if that doesn’t make sense. It is much easier to show you through example. Lets prove the following statement `∀ n > 0`:

```haskell
take n (map f xs) = map f (take n xs)
```

Intuitively this makes sense. For example if f was `(+2)`: adding two to a whole list, then selecting `n` elements from that list is the same as selecting n elements from the inital list then adding two to these.

When it comes to proofs it is always handy to have the definitions of the functions involved written close by for easy referance:

```Haskell
    map :: (a -> b) -> [a] -> [b]
    map f [] = []
    map f (x : xs) = f x : (map f xs)

    take :: Int -> [a] -> [a]
    take _ [] = []
    take 0  _ = []
    take n (x : xs) = x : (take (n - 1) xs)
```

First we have to prove the base case: the case in which `xs = []`

```haskell
    take n (map f [])
    = { definition map }
    take n []
    = { definition take }
    []
    = { definition map }
    map f []
    = { definition take }
    map f (take n [])
```

All that I have done is copied the statement we want to prove with `[]` instead of `xs (take n (map f [ ]) = map f (take n [ ]))`. Then I took the LHS and manipulated the expression using the definitions of map and take until I got to the LHS. So going from the first line to the second line (map f [ ]) has been exchanged for [ ] following the first line in the definition of map.

Now we must prove our statement for the case (x : xs):

```
take n (map f (x : xs)) = { definition map }
```

take n (map f (x : xs)) = { definition take } (f x : take (n − 1) (map f xs)) = { induction hypothesis } (f x : map f (take (n − 1) xs)) = { definition map } map f (x : (take (n − 1) xs)) = { definition take } map f (take n (x : xs)) This stage is done in exactly the same way except we now have an extra substitution that we can make. We can use the induction hypothesis! This means that if at any point if we have an expression in the form (take n (map f xs)) we can swap it our for (map f (take n xs)) or vice versa, in the example proof the IH happens when n = (n − 1) which is perfectly valid. Clearly we couldn’t have used this immediately due to the presence of the x before the xs. Since we have now isolated the smaller case of xs we can use the IH because induction is based on assuming something for a smaller csse making the bigger case true.

Proofs are a game of applying defintions in the right way so that the RHS equals the LHS, and when there is an induction hypothesis involved I just normally apply defintions to make it look sort of similar then normally you will find that all you have to do is apply the induction hypothesis and you’re done!
