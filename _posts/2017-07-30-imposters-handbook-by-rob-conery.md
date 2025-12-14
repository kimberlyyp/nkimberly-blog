---
layout: post
title: "Imposter's Handbook by Rob Conery"
date: 2017-07-30 04:34:04 +0000
categories: ['Book Reviews']
---

#### *Rob Conery is the author of [The Imposter's Handbook](https://goo.gl/hPYjn1), founder of [Big Machine](https://bigmachine.io/), and a software developer living in Seattle. He also hosts and produces the podcast [This Developer's Life](http://thisdeveloperslife.com/). *
![imposter](https://nkimberly.wordpress.com/wp-content/uploads/2017/07/imposter.png)
#### 
#### Book Review #7
## Review
The Imposter's Handbook is a breath of fresh air.  The tone is conversational, the concepts aren't overestimated, and the range of topics is enormous. Rob was inspired to write this handbook in response to his own [Imposter's Syndrome](https://en.wikipedia.org/wiki/Impostor_syndrome) as a self-taught developer. Although Imposter's Syndrome never really goes away, Rob hopes to help manage the symptoms by providing us 700+ pages of essentials skills and ideas every developer should know.

The handbook makes no claim to be as detailed as the topics deserve. What the handbook does do well is stimulate curiosity: I found myself excited to read 5 pages of google for every 1 page I read of his book. I loved that.

Rob is the mentor who is wide-reaching enough in his conversations that I got a glimpse of all that I could learn, and yet substantial enough to provide the necessary vocabulary and mental paradigms I needed to learn more. Reading the handbook felt like a sit-down session with a mentor who cared enough to pass on his knowledge in a generous and masterful way. As Rob mentioned, this handbook is probably not a great book for beginners but certainly perfect for someone just a couple years in.

**Rating: 10/10**
## Notes
#### Chapter 1: Complexity Theory
The text starts off strong. Rob jumps right into complexity theory, which is concerned with the classification of computational problems.

When faced with a problem, there are various ways to reach the correct answer. At the risk of overgeneralization, problems can be said to be (1) solved directly, if the scaling complexity is in finite time and/or (2) verified from 'lucky guesses', if the decision problem is verifiable in finite time.

Some important terms introduced:

**Scaling complexity**: *Do more inputs cause the problem to scale in a polynomial fashion- or an exponential fashion?*

**Verification complexity**: *Can we verify that a 'lucky guess' is correct in polynomial time, exponential time, or neither? ***Decision** problem: A problem with a **yes** or **no** answer.

**Computational method requirement**: *Can we solve the problem using a deterministic computer (our everyday computers)? Or do we require a non-deterministic computer (theoretical Turing machine)? ***Deterministic** computing: When faced with a fork in the road, the computer knows which path to take during problem-solving session to get to the answer. **Non-deterministic** computing: When faced with a fork in the road, the computer clones itself to take both roads, and then at the end of the problem-solving session, the clones compare notes to find which path was the correct one. A non-deterministic computer can always solve deterministic problems (determinism is subset of non-determinism).

The three main complexity classes are P (solvable in polynomial time- easy), Exp (solvable in exponential time- hard), and R (solvable).

We can explore more subset classes of complexity theory by looking into its verification complexity and whether it requires deterministic or non-deterministic computing.

![complexity-classes.png](https://nkimberly.wordpress.com/wp-content/uploads/2017/07/complexity-classes.png)

**P (polynomial) i**s a complexity class that represents the set of all decision problems that can be solved and verified deterministically in polynomial time.

**NP (non-deterministic polynomial)** can be solved non-deterministically in polynomial time and verified deterministically in polynomial time.

	Those that can be solvable deterministically in polynomial time are also in P class, hence P is a subset of NP.
	Those that can be solvable deterministically in exponential time are also in Exp class, hence a portion of Exp is under NP.
	Decision problems that can be solvable in polynomial time are also in NP-Complete, hence NP-complete is under NP.

**NP-Hard **is considered** NP-complete** if it is verifiable deterministically in polynomial time. **NP-complete** sits at the intersection of** NP** and **NP-Hard**.  Otherwise, **NP-Hard** is simply not solvable non-deterministically in polynomial time nor verified deterministically in polynomial time.

Therefore, NP-hard is at least as hard as NP. Solving NP-Hard in polynomial time means that there is a solution to all NP problems in polynomial time.
#### Chapter 2: Big-O
It was such a relief to arrive at this chapter after the mind-spinning chapter on complexity theory.  Big-O is, put simply, concerned with quantifying the scaling complexity of real-world algorithms.  The [site](http://bigocheatsheet.com/) specifically is a good cheat sheet on how algorithms can scale as inputs scale over time. Ideally, we'd like to stay under O(n log n) territory.

![big-o.png](https://nkimberly.wordpress.com/wp-content/uploads/2017/07/big-o.png)
#### Lambda Calculus
Lambda Calculus is a system in mathematical logic that provides the semantics for computation, so to allow formal study of computation.  Alonzo Church developed lambda calculus to represent constructs that we take for granted today: conditional branches, loops, and higher order functions.

There are some pretty simple rules:

	No data types
	No numbers, dates, strings, etc. only representations of which
	only functions, and only 1 argument per function
	substitutions and reductions allowed

Term to know: **Combinators** are defined as higher-order functions that use only function application and earlier defined combinators to define a result from their arguments.

Let's see some lambda calculus in action. (Lambda calculus does not use integers, but for sake of examples, I am defining some here.)

let y = 3;
let z = 1;

**Constant function λx.y**
x is reassigned to z but return is not changed since this is a constant fn.

let fn1 = (x => y)(z); //x is the 'bound' variable while y is the 'free' variable.
console.log(fn1); => 3

**Identity function λx.x, or 'I combinator'**
z is reassigned to z and return is changed since this is identity fn.

let fn2 = (x => x)(z);
console.log(fn2); // => 1

**Currying**
λx.λy.t // such that function x has body of λy.t
Lambda functions only have arity of 1; 1 bound variable allowed
Left to right substitution: λx.λy.y x = λy.y[x:=x] = λy.y
Argument x is reassigned to x, reducing overall function to λy.y.

let first = (y => y);
let second = (x => first);
console.log(second(first)); // => first

**Order of Operations**
With parenthesis, x is assigned to y then fed into λx to produce a return of x.
λx.(λy.y x) = λx.(y[y:=x]) = λx.x
Without parenthesis, x is assigned x and fed into λy.y, which does nothing to λy.y since it is the I combinator.
λx.λy.y x = (λy.y)[x:=x] = λy.y

**Boolean**
If true then x else y can be written in lambda as,
λx.λy.x

**Loops**
The looping combinator is known as the Ω combinator.
λx.xx (λx.xx) = x[x:=λx.xx] = λx.xx λx.xx
The Ω combinator not only returns what's given but also its replica.
The Y combinator takes this a step further. Instead of just replicating xx, Y combinator replicates the application of f(xx). Additionally the Y combinator allows a second expression to represent number of times we want the loop to execute.
λf.(λx.f(xx))(λx.f(xx))
The meaning of lambda expressions is defined by how expressions can be reduced. There are three kinds of reduction:

	α-conversion: changing bound variables (alpha);
	β-reduction: applying functions to their arguments (beta);
	η-conversion: which captures a notion of extensionality (eta).

Let's look at the beta reduction of Y combinator with g application:

	Reduce λf by applying Y to g,

	λf.(λx.f(xx))(λx.f(xx)) g = (λf.g(xx))(λf.g(xx))

	Reduce λx by applying left function to right function,

	(λf.g(xx))(λf.g(xx)) = g((λf.g(xx))(λf.g(xx)))

	Apply second equality, which repeatedly gives our recursive function,

	g((λf.g(xx))(λf.g(xx))) = g(Yg) = g(g(Yg)) = g(...g(Yg)...)

#### Chapter 4: Computation
As Rob so eloquently puts it, "The very nature of the physical universe is indeed computed." The laws that govern the physical universe corresponds to algorithms rather well.  Cause and effect can be thought of as the universe's conditional branching. while planets index through a recursive loop through space. Physicists have made tremendous progress toward computing the mathematics of the universe. Through computation, we gain understanding of the computed world.

Charles Babbage, a mathematician, philosopher, and engineer designed **The Difference Engine **and the **The Analytical**** Engine**.

The idea behind The Difference Engine was to use gears and pullies to compute simple functions. Although his project was funded, he only got part of it done before the English government pulled the plug on his project. He had spent 10 years designing and redesigning the machine.

Babbage's next design was The Analytical Engine. His design uses the punch card to effect gear rotation to conduct mathematical calculations. The machine was heavily inspired by the [Jacquard Loom](https://www.youtube.com/watch?v=K6NgMNvK52A), which was a programmable loom that could create complex patterns in textiles, relying also on punch cards. It was Ada Lovelace who discovered the true effect of Babbage's Analytical Engine. A brilliant mathematician and widely regarded as the very first programmer, Lovelace introduced to Babbage and the world the notion and implementation of an **algorithm**. The two collaborated often, and the Analytical Engine became the first design for a general purpose computer that could be described in modern terms as Turing-complete. The machine incorporated an arithmetic logic unit, control flow in the form of conditional branching and loops, and integrated memory.

Afterwards, during the World Wars, Alan Turing designed and built the Bombe, which helped allies compute daily settings for the German encryption device, *Enigma*.

It would be fair to say that Babbage, Lovelace, Turing and Church did all of the hard work in terms of figuring out the blueprint of how a machine could solve problems for us. The 20th century then simply engineered on these blueprints.
#### Chapter 5: Theory to Machinery
First, let's talk about processing. Then we'll move onto memory.

**Getting the process right-**

**Markov Chains** describes a flow chart that allows us to make predictions on the future of a process solely based on the present state, independent of the process's history. For this reason, the process is also referred to as the drunk Markov Chain, allowing for equivalent probability of +1 or -1 steps from present state.

	The concept of a Markov chain is used to develop a **Finite State Machine**. There are two types of Finite State Machines: Deterministic and Nondeterministic.
	The **Deterministic Finite State Machine **does not have a rejection state. It simply progresses until it reaches the accepted state. Recall the deterministic method mentioned in Chapter 1 on complexity theory.

![deterministic-finite](https://nkimberly.wordpress.com/wp-content/uploads/2017/07/deterministic-finite.png?w=600)

	The **Nondeterministic Finite State Machine** allows for rejection states and is capable of transitioning backwards, at random, which more properly simulates the random factor present in reality. It can give you different outputs when the program is run multiple times, even with the same input! This leverages the drunken Markov chain process.

![nondeterministic-finite](https://nkimberly.wordpress.com/wp-content/uploads/2017/07/nondeterministic-finite.png?w=600)

**Creating memory-**

A **Stack **is quite literally a stack of data that you can add to or remove from. A finite state machine just looks at the input signal and the current state: it has no stack to work with. It chooses a new state, the result of following the transition. A **Pushdown Machine** differs from a finite state machine in two ways:

	It can use the top of the stack to decide which transition to take.
	It can manipulate the stack as part of performing a transition.

The Pushdown Machine allows us to **sum**, a powerful development on the FSM. Since the Pushdown Machine can only access what is on the very top of its stack, however, it is limited to only unidirectional movement through its program.

To overcome this, Alan Turing described what is now called a Turing Machine. A Turing Machine consists of,

	a tape, divided into cells with a symbol from some finite alphabet
	a head that can read and write symbols on the tape one cell at a time
	a register that stores the state of the machine
	and a finite table of instructions that takes the current state and the symbol on the tape as arguments to either erase/write over the symbol on the tape and then to either move to the left, right, or not at all.

The machine makes use of the the idea that we could leverage the result of a previous calculation to be fed into a future calculation. This is essentially the same notion Alonzo Church had when he described via lambda calculus how a function could take another function.

The Turing Machine is computationally more powerful than the Pushdown Machine because it is able to arbitrarily move back and forth over the tape, while the Pushdown Machine can only have access to the top of its stack. Pushdown Machines, however, can make use of two stacks and become just as computationally powerful- the use of two stacks allows the Pushdown Machine to feed a former calculation into a future calculation.

**The Halting Problem-**

The Turing Machine isn't invincible however. Turing was able to find an algorithm that the Turing Machine cannot solve. Indeed, that is, generally speaking, undecidable.

Let's say you have a program that determines whether or not another program will halt. If the program halts, then the program returns true; if it doesn't, then the program returns false. Now let's say we feed that program into itself. Then we get a self-contradiction:

	If the argument program halts, then the overall program will not;
	If the argument program does not halt, then the overall program will.

The Halting problem is unsolvable since it is a self-referencing paradox (think Epimenides paradox: "all Cretans are liars").

**From Theory to Abstraction-**

The greatest part about this section for me was discovering that Turing had submitted a design for an electronic "general purpose calculator" to the National Physics Laboratory just after the war. NPL wasn't sure that creating such a machine would work.  At the time, they were unaware that Turing had worked with the exact electronic computer (Colossus) every day to help break Nazi ciphers; he just wasn't allowed to tell them since he was sworn to secrecy.

**von Neumann (actually Turing, Eckert, and Mauchly) Architecture-**

Due to some soft-publishing drama (an initial draft being passed around with only von Neumann's name on it), this design is attributed to von Neumann despite that it really was Turing, Eckert, and Mauchly's idea.

In any case, the basic architecture engages an input -> CPU -> output. The CPU contains a control unit, logic unit, and memory unit.

Lambda calculus and Turing machines are theoretical concepts that were invented to explore the domain of computable problems. Von Neumann architecture is the architecture by which real computers were eventually constructed. ![von-neumann.png](https://nkimberly.wordpress.com/wp-content/uploads/2017/07/von-neumann-e1501384589952.png)

At long last, a real machine within a machine
#### Chapter 6: Data Structures
An **Array **is a linear data structure (contents are store sequentially) that allow for random access based on index number. It can be cumbersome to find memory space for it when programs require the array to constantly fluctuate in size.  There are static, dynamic, and two dimensional arrays. Static arrays are declared with a fixed size. Dynamic arrays have reservation space for additional elements and replicates itself in another location when it needs to become a larger array. Two dimensional arrays allow for both x and y indices.

A **Linked List** in its simplest form stores data with nodes that point to other nodes. Each node possesses one datum and one reference to another node. The linked list chains nodes together by pointing one node's reference to another.

A **Hash Table** is also a linear data structure that store data via a key alongside its datum. The contents can be randomly accessed using this non-sequential key. In terms of location, a hashing function accepts a key and returns an output unique to that key specifying the space in the memory to store the datum. Hash collisions are unavoidable, and there are several [methods](https://www.quora.com/How-is-collision-handled-in-HashMap) to circumvent them.

A **Graph **is a non-linear data structure with vertices and edges (unidirectional or bidirectional). To store a graph, one could store nodes as objects and edges as pointers. The memory of complexity for this strategy is O(n) for number of objects and O(n^2) for pointers up to n nodes. Time complexity for access would be O(n). Another way to go about it would be to store a matrix containing all edge weights between node x and node y. The memory complexity is O(n^2), but the time complexity for access is O(1)!

If you want to define a start point as well as parent-child relationships, you could opt for the **tree** data structure, which really is just a minimally connected graph having only one path between any two vertices.  There are, of course, **binary search trees** and **[binary] heap [trees]**. Binary search trees guarantee order (left vs. right), while heaps simply guarantee that elements on higher levels are greater.

![data-structure-complexity.png](https://nkimberly.wordpress.com/wp-content/uploads/2017/07/data-structure-complexity.png)
#### 
#### Chapter 7: Simple Algorithms
This chapter covers the time complexity of 5 simple algorithms as well as how they work.

Algorithms are extremely situation dependent, so it isn't precise to say that some are 'better' than others; however, if we evaluate based on range of time complexity, we can at least rank the algorithms on general efficiency.

I will cover the 'slower' algorithms first and then proceed to the more time-efficient algorithms (Selection  2 vs. 3 => 2 is min
2,3,4,1 => 2 vs. 4 => 2 is min
2,3,4,1 => 2 vs. 1 => 1 is min => move 1 to first position
1,2,3,4

**At its best, Bubble Sort can be Ω(n), but at its worst, it is O(n^2)**, so it can be a total time sink. This algorithm basically just 'bubbles up' the largest values.

The algorithm begins at the left of a list, and compares the first two numbers. The purpose of the comparison is to allow the algorithm to decide which is larger and therefore should be moved to the right.
*2,3,*4,1 => 2 vs. 3

The algorithm will keep crawling to the right until it reaches the end of the list.
2,*3,4*,1 => 3 vs. 4
2,3,*4,1* => 4 vs. 1

Once it reaches the end of a given list, it must pass over the list again and again and again until all items in the list are in order from smallest to largest. If there are 100 numbers in a list, and they are in reverse order (100, 99, 98, etc...) then the algorithm will literally have to pass over the list 100 times for all 100 items. That's a time complexity of n^2!
2,3,1,4 => 2 vs. 3
2,3,1,4 => 3 vs. 1
2,1,3,4 => 3 vs. 4
2,1,3,4 => 2 vs. 1 => 1,2,3,4

**At its best, Quick Sort can be Ω(n log(n)), but at its worst, it is O(n^2). **Quick Sort is one of those algorithms that when used in the proper situations, can be more powerful than merge sort but when used improperly can be exponentially slower.

By convention, quick sort algorithms will pick the very last element of a list to be a 'pivot' or 'partition' point.
2,3,4,1 => 1 is the pivot point.

With this point, we will make comparisons with the rest of the list such that everything lower than the pivot point is on the left and everything higher is on the right. Since 1 is less than 2, 1 and 2 get to switch places.
1 vs. 2 => 1,3,4,2
1 vs. 3 => 1,3,4,2
1 vs. 4 => 1,3,4,2
[nothing]  1,2,4,3
2 vs. 4 => 1,2,4,3
[]  1,2,3,4

As you can see, Quick sort can be entirely inefficient if the partition points aren't picked well. For this reason, it is mostly implemented with some pre-work to find the median of the list.

**Merge Sort is O(n log n) both at its best and at its worst. **Merge Sort is one of the most efficient ways you can sort. Unfortunately, unlike quick sort, which can be optimized, Merge sort both at its best and at its worst is O(n log(n)).

Merge Sorting can best be summarized as divide-and-conquer approach. The greatest benefit of a merged sort is that the leftmost numbers are always smaller, giving us a lot of power and generally saving us almost half of the time we would otherwise take with bubble sort.

First, the array is completely divided into pairs. Then the pairs are evaluated so that the left has the smaller of the pair while the right has the larger of the pair.
2,3,4,1 => 2 vs. 3 and 4 vs. 1 => 2,3 and. 1,4

Now adjacent pairs are compared.
2 vs. 1 => 1
2 vs. 4 => 1, 2
3 vs. 4 => 1, 2, 3, 4
1, 2, 3, 4

The lesser of the first pair is compared to the lesser of the second. The lesser of these are placed in first, while the greater of which is compared to the greater of the second pair. Finally the greater of both pairs are compared and the sort is completed.

**Like Merge Sort, Heap Sort is also O(n log n) both at its best and at its worst. **Heap sort can be thought of as a smarter selection sort. Instead of finding minimums, we find maximums and send them to the end of the sorted area. And unlike selection sort, which employs linear time search, we leverage a heap data structure to find the maximum. Heap data structures use binary splitting through a set of rules to reduce time complexity.

2,3,4,1 => 2[3,4] => 4 > 2: that violates heap rules so 4 and 2 are swapped.
4[3,2] => 4[3[1],2] => done with our tree, so now we just take the node as max and move down! 4 -> 3/2 -> 1!

[ ][ ][ ][4] => [ ][ ][3][4] => [ ][2][3][4] => [1][2][3][4]

Not too bad. Heap sort is not as efficient as an optimized Quick sort; however, it has the advantage of a more favorable worst-case O(n log n) run time.
#### Chapter 8: Dynamic Programming
Dynamic Programming is solving an optimization by guessing in a systematic way.

To use dynamic programming, our problem must,

	Be an optimization problem: *think bin packing problems*
	Be dividable into sub-problems: *think recursive methods*
	Have an optimal sub-structure: *think shortest path problems*
	Be reducible to P time through memoization: *think Fibonacci problems*

A great way to introduce the concept of memoization is to understand the Fibonacci problem. Fibonacci numbers, as we know, are found through the following eqn:
**F_n = F_n-1 + F_n-2,** where one must reduce to,
**F_1 = 1**
or
**F_0 = 0**
As an example, in order to get the 5th Fibonacci number,
F_5 = F_4 + F_3
F_5 = [F_3+F_2] + [F_2+F_1]
F_5 = [[F_2+F_1] + [F_1 + F_0]] + [[F_1 + F_0] + [F_1]]
F_5 = [[[F_1 + F_0] + F_1]] + [F_1 + F_0]] + [[F_1 + F_0] + [F_1]]
F_5 = 1 + 0 + 1 + 1 + 0 + 1 + 0 + 1
**F_5 = 5**
That's pretty insane huh? Imagine trying to find the 100th Fibonacci term... The solution has a complexity of O(2^n)!  It's obvious, however, that there are a lot of repetitions in these calculations that we could get rid of to optimize for our solution. This is where memo-ization comes into play. A simple memo table can be used to track the solutions to smaller elements as they build up toward our answer. The memo table would convert our complexity of n^2 to just n! Welcome to P time :)

![fibonacci](https://nkimberly.wordpress.com/wp-content/uploads/2017/07/fibonacci.png?w=512)

For example, both F_4 and F_3 required calculations of F_3 and F_2. If one of our branches is calculated (say we computed F_3 all the way down first), we can leverage those answers to compute F_4. Practically speaking, this could be implemented with a simple push to a sequence array. Memoization of the Fibonacci problem is a great use dynamic programming to optimize for a more efficient solution.
#### Chapter 9: Compilation
I don't personally find this chapter too interesting, so I'll keep it lean. Compilers convert code from one language to another-- generally a higher level language to a lower level one. A compiler is made up of a few module parts.

Lexer -> Parser -> Generator.

The lexer converts the source code to tokens. The parser converts the tokens into an abstract syntax tree. And the generator takes the AST and transforms it into the target language. See my poor diagram below. ;)

![22323819_1424353591015072_249372788_o.png](https://nkimberly.wordpress.com/wp-content/uploads/2017/07/22323819_1424353591015072_249372788_o.png)
#### Chapter 10: Functional Programming
Functional programming is a paradigm, as are Object-oriented programming and imperative programming.

Functional programming can be quickly summarized through a few 'commandments'-

	**Functions are king**
- Using functions, particularly in JS, is by consensus easier to maintain and debug.
	**Use 'pure' functions.**
- Pure functions prevent side effects by using the input and only the input to return one output, nothing more and nothing less.
	**Use higher-order functions**
- In functional programming, any type of data is fair game. We can pull this off this versatility by allowing functions to take on other functions as input.
	**Avoid iterations
**- Use map, reduce, filter, etc. instead.
	**Avoid mutating data
**- Mutating data can cause for hard-to-track bugs down the road. Immutability, however, does lend itself to some problems with efficiency. To get around this, make use of the Mori library or immutable.js. These leverage structural sharing to avoid copying over entire arrays. Instead, a new leaf is simply created and the root of the data tree is adjusted to branch to the new leaf rather than the old one.

#### Chapter 11: Databases
Normalization- A guideline.

**First Normal Form:** Convert to Atomic Values. If you've got a table with email, name, order_id, items and price -- make sure items has only item. Then you've completed 1NF.

**Second Normal Form:** Make sure there is a single primary key. At this point, we split out table into two. Name describes Email, which is the primary Key. As for our second table... Order_id is sort of described by item, price, customers, and email... Not really.

**Third Normal Form:** Nonkeys must describe key and nothing else. Now we've got to fix our second table. In fact, we really should have four tables.

	Customers: id, email, name
	Order: id, customer_id, date
	Order Items: id, order_id, SKU
	Products: SKU, price, item_name

Theoretically this is good but practically speaking it results in Order Items becoming a slowly-changing historical table. Not great for someone trying to track monthly sales revenue. How do we fix this?
#### Chapter 12: Design Patterns
#### Chapter 13: Design Principles
#### Chapter 14: Test-driven Development, Behavior-driven Development
#### Chapter 15: Essential Unix Tools
#### Wrapping Up