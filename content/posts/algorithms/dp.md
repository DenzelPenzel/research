---
title: "Algorithms / Dynamic Programming"
summary: My personal notes on Dynamic Programming
date: 2024-03-28
aliases: ["/dynamic-programming"]
tags: ["Algorithms", "Dynamic Programming"]
author: ["DenzelPenzel"]
draft: false
aliases: [/posts/algorithms/dynamic-programming]
weight: 3
---

---

## Getting Started 🚀

In this article, I want to talk to you about one of the most challenging topics in programming - ***Dynamic Programming***. Problems in this area often make even the most experienced programmers nervous, who have solved a huge number of tasks on this topic. This article will be periodically updated with various topics related to Dynamic Programming.

--- 
## Notes
> - Dynamic Programming requires the subproblem solved in topological order
> - In many problems, it coincides the natural order
> - For those who doesn't, one need perform topological sorting first
> - Therefore, for those problems with complex topology search with memorization is usually an easier and better choice

## Top-Down and Bottom-Up

Dynamic Programming offers two main strategies: top-down and bottom-up. 

Let's delve into their discrepancies:

**Top-Down Approach**:

> - Also known as memoization, the top-down approach starts with the original problem, breaking it into smaller subproblems
> - Subproblems are solved recursively, with solutions stored in a data structure (often an array or hash table) to prevent redundant computations
> - Typically involves crafting a recursive function with a base case and verifying if a solution for a subproblem has been computed before recalculating it

**Bottom-Up Approach**:
> - Conversely, the bottom-up approach tackles subproblems first, utilizing their solutions to construct the original problem's solution
> - It commences by solving the smallest subproblems and progressively moves towards resolving larger ones until reaching the original problem 
> - This method typically entails iterating through subproblems in a specific order and storing solutions in a table or array

---

## 0/1 Knapsack 🌟

Allows answering the question:

**"Are there multiple numbers in the set that can sum up to a certain value?"**

The knapsack will contain the maximum value from all possible combinations of the first ```i``` elements with a sum limited to ```w```.

The state of the knapsack is defined as: 

```
dp[i][w] = max(dp[i - 1][w], dp[i - 1][w-w[i]] + w[i])

if w - w[i] < 0: # take prev state
    dp[i][w] = dp[i-1][w]
else:
    dp[i][w] = max(dp[i - 1][w], dp[i - 1][w-w[i]] + w[i])    
```

The main thing here is to **determine up to which value of the sum it is necessary to check the summation**.

In this implementation, there is one subtlety:
- We have the ability to update the states of ```dp``` through an increasing increment of ```w``` or through a decreasing one
- By increasing ```w```, the previous partial result ```dp[w - coin]``` is the result that has already considered this coin
- By decreasing ```w```, the previous partial result ```dp[w - coin]``` is the result that has not yet considered the coin

### Problems:

**Unique ways. Repeating coins is ALLOWED**

Ask to find the number of unique ways to obtain the sum from the first ```i``` numbers.

**Example**:<br/>
Input: coins: [1]; target = 5<br/>
Explanation:<br/>
1 + 1 + 1 + 1

```
def coin_combinations(coins: List[int], target: int) -> int:
    mod = 10 ** 9 + 7
    dp = [0] * (target + 1)
    dp[0] = 1

    for coin in coins:
        for w in range(coin, target + 1):
            dp[w] = dp[w] + dp[w - coin]
            dp[w] %= mod
    
    return dp[target]
```

---

**Unique ways. Coins cannot be repeated**

Ask to find the number of unique ways to obtain the sum from the first ```i``` numbers.

**Example**:<br/>
Input: coins = [2, 3, 5], target = 9<br/>
Explanation:<br/>
2 + 2 + 5<br/>
3 + 3 + 3<br/>
2 + 2 + 2 + 3<br/>
```
def coin_combinations_unique(coins: List[int], target: int) -> int:
    mod = 10 ** 9 + 7
    dp = [0] * (target + 1)
    dp[0] = 1

    for coin in coins:
        for w in range(target, coin - 1, -1):
            dp[w] = dp[w] + dp[w - coin]
            dp[w] %= mod
    
    return dp[target]
```

---

**Non-unique combinations. Repeating coins is ALLOWED**

Ask to find the number of non-unique combinations that sum up to the target.<br/>

**Example**:<br/>
Input: coins = [1,2], target = 3<br/>
Explanation:<br/>
    1 -> 1 step: (1)<br/>
    2 -> 2 step: (1 + 1) (2)<br/>
    3 -> 3 step: (1 + 1) (1 + 2) (2 + 1)<br/>

**Example**:<br/>
Input: nums = [1,2,3], target = 4<br/>
Output: 7<br/>
Explanation:<br/>
    The possible combination ways are:<br/>
    (1, 1, 1, 1)<br/>
    (1, 1, 2) -> (1, 2, 1) -> (2, 1, 1)<br/>
    (1, 3) -> (3, 1)<br/>
    (2, 2)
```
def coin_combinations_not_unique(coins: List[int], target: int) -> int:
    mod = 10 ** 9 + 7
    dp = [0] * (target + 1)
    dp[0] = 1

    for w in range(1, target + 1):
        for coin in coins:
            if w - coin >= 0:
                dp[w] = dp[w] + dp[w - coin]
                dp[w] %= mod

    return dp[target]
```

### Practice 
- [Book Shop](https://cses.fi/problemset/task/1158)
- [Coin Combinations I](https://cses.fi/problemset/task/1635)
- [Coin Combinations II](https://cses.fi/problemset/task/1636)
- [Minimizing Coins](https://cses.fi/problemset/task/1634)
- [Money Sums](https://cses.fi/problemset/task/1745)
---
