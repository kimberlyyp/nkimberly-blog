---
layout: post
title: "Exploring a Caterpillar Problem"
date: 2018-08-06 19:21:32 +0000
categories: ['Python']
---

Say,

 	All caterpillars traverse n leaves.
 	A caterpillar with jump number j eats leaves at positions that are multiples of j until it reaches the end and stops and build its cocoon.

**Given n leaves and a set k elements of caterpillars, we need to determine the number of uneaten leaves.**

*(kTypically, I like to solve a simple example by hand just to get a general idea of the problem. Thinking through this, I came up with two approaches. I'll describe the logic first, then delve into the implementation, and end with a performance comparison.
![38509181_604192003310827_7029190042669023232_n](https://nkimberly.wordpress.com/wp-content/uploads/2018/08/38509181_604192003310827_7029190042669023232_n.jpg)

Thinking through this problem, I came up with two concepts for solutions. I'll describe the logic first, then delve into the implementation, and end off with a performance comparison.
### Walking Through Leaves via Nested Loops
The naive approach:

 	Set 2 loops, the first running from 1 to n and the second running from 0 to len(k)
 	Calculate modulo to see if the current leaf is a multiple of any caterpillar's jump value
 	Count it as eaten if it is
 	Subtract eaten count from total number of leaves to obtain leftover count

We can optimize slightly by breaking out of the inner k loop when a leaf is eaten; if one caterpillar is eating it, we don't care if others want it too. It's gone.
As you can see in the implementation below:

 	I've initialized an eat_count integer variable as 0 and an eaten boolean as False
 	Then I walk through each leaf, and within that loop, check each caterpillar's jump sequence to determine whether anyone is going to eat that leaf
 	I set eaten as True and break out of the inner loop the moment we have a taker, and count that leaf as eaten
 	Finally, I subtract the eaten count from the total number of leaves and we have our leftover count

https://gist.github.com/n-kimberly/2c1719ea7fc41aba911f5a803e23d6b1
### II. Thinking about the Number Patterns
First, I'm thinking to divide the total number of leaves by each k value and sum the results. If we have a jump number of 2, it makes sense to say that 50 leaves have been eaten.

 	100 leaves / jump number of 2 = 50 eaten leaves

https://gist.github.com/n-kimberly/7caa5b45e333792e66072ca66cc5e870
Now there is the complication of double counting leaves. For example, jump sequences of [2, 4] can lead to the following calculation:

 	100 leaves / jump number of 2 = 50 eaten leaves.
 	If we have jump numbers 2 and 4, we want to subtract 100/4 = 25, because all leaves that are multiples of 4 have already been counted in 2's total.
 	If a caterpillar eats every 3 leaves and another eats every 4 leaves, then we'll want to subtract the double-counted values, which occur every 12th leaf. In n = 100, this is 100 // 12 = 8 leaves. (We use floor division since leaves are either there or not.)

If you couldn't tell already, our brains are instinctively computing the least common multiple.
Now let's implement this. We first write a find_lcm method. Then we use that method in our function to compute what we need to subtract back out.
https://gist.github.com/n-kimberly/f6de46700a3b17fcb9fa88a4d233ce00
Cool. Now what if we have three caterpillars instead of two? Let's look at a Venn diagram of two features.
![38796558_2041046642573606_1712773120160432128_n1.jpg](https://nkimberly.wordpress.com/wp-content/uploads/2018/08/38796558_2041046642573606_1712773120160432128_n1-e1533577782567.jpg?w=852)
Previously, we computed the LCM of each pair to account for the shared space and subtracted that. What happens when we have a Venn diagram of three features?
Now there are double-counted spots and triple-counted spots! To complicate things further, when we subtract the double-counted spots, we subtract too much, since they share the triple-counted region with each other. Thus we need to add the triple-counted values back in.
Is this starting to seem insane? Not to worry. This is a well-known technique called the inclusion-exclusion principle. The pattern is:

 	Add counts for single elements
 	Subtract counts for pairs
 	Add counts for triples
 	Subtract counts for quadruples
 	And so on, alternating signs

More generally: subtract even-sized subsets, add odd-sized subsets.
If we draw a Venn diagram of 4 overlapping circles, we'll find the same pattern holds. We add single counts, subtract pairs, add triples, subtract quadruples.
There's one complication: once we have more than 2 caterpillars, we need to compute all combinations of subsets to feed into our LCM method. This is O(2^k) since there are 2^k subsets. I've used recursion to compute subsets in my implementation. Feel free to use your algorithm of choice. Note that I've constrained mine to avoid empty subsets.
One implementation note: when computing LCMs across multiple jump values, the result can grow large quickly. In practice, you'll want to check whether the LCM exceeds n (and short-circuit to 0 if so) to avoid overflow issues.
We leverage this subset method to compute the values we want to add or subtract. Then, as mentioned, we subtract all even-length subsets and add all odd-length subsets.
https://gist.github.com/n-kimberly/36ee644d79dca7c65a4f7e85398e35a2
That's all for the implementation! Now let's check performance.

## Performance: O(2^k) vs. O(n*k)
We have two inputs: n number of leaves and k number of caterpillars (with their corresponding jump numbers).

### I. Scaling n number of leaves, keeping len(k)=2
If we raise n, it's trivial to say that our nested loops method ("2Loops") will slow down. It walks through each leaf, so even if we keep k constant, the time complexity is O(n).
On the other hand, our LCM-driven method ("Math") stays flat. At k = 2, we're conducting the same 3-4 math operations regardless of n, so time complexity with respect to n is O(1).
![performance_n](https://nkimberly.wordpress.com/wp-content/uploads/2018/08/performance_n.png)
### II. Scaling k number of caterpillars
Now what happens if k increases?
In our Math method, we have to compute exponentially more subsets (pairs, triplets, quadruplets, etc.) as we increase k. If we keep n constant, our time complexity is O(2^k). We're also storing them, so there is a space complexity of O(2^k).
You can see from this plot how quickly Math degrades as we scale k up. In the zoomed-in plot below, you can see that at around k = 4, Math started losing to 2Loops.
Our nested loop only runs through each number once, and each k up to once, giving O(n × k).
![performance_k](https://nkimberly.wordpress.com/wp-content/uploads/2018/08/performance_k.png)
**So which should you use?**
This is where the problem constraints matter. The problem states k 

 	O(2^k) with k = 15: roughly 32,768 operations
 	O(n × k) with n = 10^9 and k = 15: roughly 15 billion operations

For the actual problem constraints, Math wins decisively. The nested loop approach would time out on any competitive programming judge with n = 10^9.
The crossover analysis above used small n values. When n is tiny, the constant factors in the Math approach dominate, and 2Loops can win. But the whole point of the Math approach is that it's independent of n, which matters enormously when n is a billion.
General rule: if n is large relative to 2^k, use Math. If n is small relative to 2^k, use 2Loops. For this problem's constraints, Math is the clear choice.