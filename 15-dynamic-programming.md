# Pattern 7: Dynamic Programming (DP)

## 1. Pattern Name
**Dynamic Programming**

## 2. Explain the Pattern in Very Simple English
Imagine you're climbing a staircase and someone asks "how many different ways can you reach step 10?" If you calculate "ways to reach step 8" and "ways to reach step 9" separately every single time you need them, you're repeating work.

Dynamic Programming says: **solve each smaller sub-problem once, write the answer on a sticky note (memoize/store it), and reuse that note whenever you need it again** instead of recalculating.

It's basically "smart recursion with memory" — break the big problem into smaller overlapping problems, solve each once, and combine the answers.

## 3. When Should I Think of This Pattern?
- Problem asks for **maximum/minimum/count of ways** to do something
- You notice the same sub-problem is being **solved again and again** in a recursive solution
- Problem can be broken into a **decision at each step** (take it / skip it, choose this / choose that)
- Keywords: "number of ways", "minimum cost to reach", "longest/shortest", "can you reach", "maximum profit"
- Naive recursive solution has **exponential time** — DP can bring it down to polynomial

## 4. Mental Trigger
> "Whenever I write a recursive solution and see the same sub-problem being solved multiple times, I think of Dynamic Programming."

## 5. Visual Explanation (ASCII)
```
Fibonacci without memoization (repeats work):
                    fib(5)
                /            \
           fib(4)            fib(3)
          /      \           /     \
      fib(3)   fib(2)    fib(2)  fib(1)
      ...         (fib(3) and fib(2) computed multiple times!)

With memoization:
fib(5) → fib(4) → fib(3) → fib(2) → fib(1)
Each value computed ONCE, stored, and reused.
```

## 6. Generic Java Pseudocode Template

**Top-down (Memoization):**
```java
Map<Integer, Integer> memo = new HashMap<>();

int solve(int n) {
    if (n <= baseCase) return baseCaseValue;
    if (memo.containsKey(n)) return memo.get(n); // reuse stored answer

    int result = combine(solve(n - 1), solve(n - 2)); // or other recurrence
    memo.put(n, result);
    return result;
}
```

**Bottom-up (Tabulation):**
```java
int[] dp = new int[n + 1];
dp[0] = baseCase0;
dp[1] = baseCase1;

for (int i = 2; i <= n; i++) {
    dp[i] = combine(dp[i - 1], dp[i - 2]); // build up from smaller answers
}
return dp[n];
```

**2D DP (common for strings / grids / knapsack):**
```java
int[][] dp = new int[m + 1][n + 1];

for (int i = 1; i <= m; i++) {
    for (int j = 1; j <= n; j++) {
        if (/* match condition */) {
            dp[i][j] = dp[i - 1][j - 1] + 1;
        } else {
            dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
        }
    }
}
```

## 7. Time Complexity
- **Time:** Usually O(n) to O(n × m), depending on the number of unique sub-problems (states) — each state is computed exactly once.
- **Space:** O(n) or O(n × m) for the memo/DP table; often reducible to O(n) or O(1) using "rolling array" optimization if only the previous row/state is needed.
- **Why it beats brute force:** Plain recursion re-solves the same sub-problem exponentially many times → O(2^n). DP guarantees each unique sub-problem is solved exactly once, turning exponential time into polynomial time.

## 8. Common Mistakes
- Not identifying the **correct state** (what parameters uniquely define a sub-problem)
- Writing recursion **without a base case**, causing infinite recursion or stack overflow
- Forgetting to actually **check the memo** before recomputing (defeats the purpose of DP)
- Getting confused about **1-indexed vs 0-indexed** DP arrays
- Trying to force DP on a problem where **Greedy** would be simpler and correct
- Not considering **space optimization** when the array size is huge (can cause memory limit exceeded)

## 9. Real Interview Examples

| Problem | Difficulty | Why DP Works | Key Observation |
|---|---|---|---|
| Climbing Stairs | Easy | Ways to reach step n = ways to reach (n-1) + ways to reach (n-2) | Same recurrence as Fibonacci |
| House Robber | Medium | At each house, decide rob or skip; overlapping sub-problems | dp[i] = max(dp[i-1], dp[i-2] + nums[i]) |
| Longest Common Subsequence | Medium | Compare characters of two strings, build 2D table | dp[i][j] depends on dp[i-1][j-1], dp[i-1][j], dp[i][j-1] |
| Coin Change | Medium | Minimum coins for amount X depends on minimum coins for smaller amounts | Unbounded knapsack variant |
| Edit Distance | Hard | Insert/delete/replace operations map to 2D DP transitions | Classic string DP |

## 10. LeetCode Problems
- Easy: [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
- Medium: [House Robber](https://leetcode.com/problems/house-robber/)
- Medium: [Coin Change](https://leetcode.com/problems/coin-change/)
- Medium: [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)
- Medium: [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)
- Medium: [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)
- Hard: [Edit Distance](https://leetcode.com/problems/edit-distance/)
- Hard: [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/)

## 11. Interview Variations
Instead of "find the minimum cost," interviewers may ask you to:
- Return the **actual path/sequence**, not just the count/cost (requires backtracking through the DP table)
- Change from **1D to 2D DP** (adding another dimension like "at most k transactions")
- Ask for **all ways**, not just the count (combine DP with backtracking)
- Optimize **space** from O(n²) to O(n) or O(1) using rolling arrays
- Convert **top-down recursion + memo** into **bottom-up iterative** DP for better performance

## 12. Pattern Recognition Checklist
```
Does the problem ask for min/max/count of ways?
        ↓ yes
Can I define the problem recursively in terms of smaller sub-problems?
        ↓ yes
Do the same sub-problems repeat (overlapping sub-problems)?
        ↓ yes
→ Think Dynamic Programming
```

## 13. Similar Patterns (Don't Confuse These)
- **DP vs Backtracking:** Backtracking explores every possibility fresh; DP caches overlapping sub-problem results to avoid recomputation. If a Backtracking solution is too slow due to repeated states, DP (with memoization) is the fix.
- **DP vs Greedy:** Greedy commits without reconsidering; DP considers all options at each state and picks the best based on stored sub-results. Use DP when Greedy fails on a counterexample.
- **DP vs Divide and Conquer:** Divide and Conquer (like Merge Sort) splits into **independent** sub-problems; DP handles **overlapping** sub-problems that would otherwise be recomputed.

## 14. Revision Summary (Cheat Sheet)
- **What it does:** Breaks a problem into overlapping sub-problems, solves each once, and reuses stored results.
- **When to use:** "Number of ways", "min/max cost", overlapping recursive sub-problems.
- **Recognition clues:** repeated recursive calls, "minimum/maximum", "number of ways", "can you reach".
- **Java template:** top-down memoization or bottom-up tabulation — see Section 6.
- **Time complexity:** O(number of states), often O(n) or O(n×m).
- **Common mistakes:** wrong state definition, missing memo check, off-by-one indexing.
- **Top interview questions:** House Robber, Coin Change, Longest Common Subsequence, Edit Distance.
