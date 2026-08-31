# Pattern 6: Greedy

## 1. Pattern Name
**Greedy**

## 2. Explain the Pattern in Very Simple English
Imagine you're at a buffet and you can only take one item at a time, but you want to maximize how "full and happy" you are. A greedy person always picks whatever looks **best right now**, without worrying about later choices.

Greedy algorithms make the locally best choice at every step, hoping (and, for the right problems, **proving**) that this leads to the overall best solution — without ever going back to reconsider.

## 3. When Should I Think of This Pattern?
- Problem asks for **minimum/maximum** count of something (intervals, coins, jumps)
- Choices made early **don't need to be reconsidered** later
- Sorting the input first reveals an obvious "best next choice"
- Problem involves **intervals** (merging, scheduling, non-overlapping)
- Words like "maximum profit", "minimum number of", "as many as possible"

## 4. Mental Trigger
> "Whenever picking the locally best option at each step seems to never hurt the final answer, I think of Greedy."

## 5. Visual Explanation (ASCII)
```
Activity Selection (choose max non-overlapping intervals):

[1--3]
      [3----6]
         [4--5]     ← overlaps, skip
              [6--8]

Sort by end time, always pick the interval that ends earliest
and doesn't overlap with the previously picked one.
```

## 6. Generic Java Pseudocode Template
```java
// 1. Sort input based on the criteria that matters (end time, ratio, etc.)
Arrays.sort(items, (a, b) -> a.endTime - b.endTime);

int count = 0;
int lastEnd = Integer.MIN_VALUE;

for (Item item : items) {
    if (item.startTime >= lastEnd) {   // greedy choice: take it if it fits
        count++;
        lastEnd = item.endTime;
    }
    // else: skip this item, it doesn't fit — no need to reconsider it later
}
return count;
```

## 7. Time Complexity
- **Time:** Usually O(n log n) — dominated by the initial sort; the greedy pass itself is O(n).
- **Space:** O(1) extra (O(n) if sorting requires extra space, depending on the sort implementation).
- **Why it beats brute force:** Brute force might try all combinations of choices (exponential). Greedy proves that a simple rule (like "always pick the earliest-ending interval") never leads to a worse answer than considering all combinations, so it solves the problem in one clean pass after sorting.

## 8. Common Mistakes
- Using Greedy on a problem where it **doesn't actually work** (not every problem has the "greedy choice property" — always verify with a proof or counterexample, or check if DP is really needed)
- Sorting by the **wrong key** (e.g., sorting by start time instead of end time in interval scheduling)
- Forgetting to **update the tracking variable** (like `lastEnd`) after making a choice
- Assuming Greedy always gives the optimal answer — some problems that "look greedy" actually need DP (e.g., 0/1 Knapsack is NOT greedy, but Fractional Knapsack IS)
- Not handling **edge cases** like empty input or all-overlapping intervals

## 9. Real Interview Examples

| Problem | Difficulty | Why Greedy Works | Key Observation |
|---|---|---|---|
| Jump Game | Medium | Track the farthest reachable index; greedily extend it | If farthest ever falls behind current index, fail |
| Gas Station | Medium | If total gas ≥ total cost, a solution exists; greedily reset start point on deficit | Local failure means all previous starts also fail |
| Non-overlapping Intervals | Medium | Sort by end time, always keep the interval that ends earliest | Removing later-ending overlaps is never worse |
| Task Scheduler | Medium | Prioritize the most frequent task first to minimize idle time | Frequency-based greedy with cooldown math |
| Candy | Hard | Give minimum candies satisfying "more than neighbor" using two greedy passes | Left-to-right pass then right-to-left pass |

## 10. LeetCode Problems
- Easy: [Assign Cookies](https://leetcode.com/problems/assign-cookies/)
- Medium: [Jump Game](https://leetcode.com/problems/jump-game/)
- Medium: [Gas Station](https://leetcode.com/problems/gas-station/)
- Medium: [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)
- Medium: [Task Scheduler](https://leetcode.com/problems/task-scheduler/)
- Hard: [Candy](https://leetcode.com/problems/candy/)

## 11. Interview Variations
Instead of "maximize non-overlapping intervals," interviewers may ask you to:
- **Minimize** the number of removals to make intervals non-overlapping
- **Merge** overlapping intervals instead of selecting non-overlapping ones
- Apply greedy to **scheduling with deadlines** (maximize completed tasks)
- Combine greedy with a **heap/priority queue** (e.g., meeting rooms, task scheduler)
- Ask you to **prove** why the greedy choice is optimal (common in FAANG-style interviews)

## 12. Pattern Recognition Checklist
```
Does the problem ask for min/max count, with choices made step by step?
        ↓ yes
Can sorting by some key reveal an obvious "best next choice"?
        ↓ yes
Once I make a choice, do I never need to undo it?
        ↓ yes
→ Think Greedy (but double-check with a small example that it's truly optimal)
```

## 13. Similar Patterns (Don't Confuse These)
- **Greedy vs Dynamic Programming:** DP explores overlapping subproblems and remembers all possibilities (used when greedy's "never look back" assumption fails); Greedy commits to one choice permanently. If you're unsure, try Greedy first with a counterexample check — if it fails, switch to DP.
- **Greedy vs Backtracking:** Backtracking explores all choices and undoes bad ones; Greedy never explores alternatives once a choice is made.

## 14. Revision Summary (Cheat Sheet)
- **What it does:** Makes the locally optimal choice at each step, banking on it leading to a globally optimal result.
- **When to use:** Interval scheduling, min/max count problems where earlier choices don't need reconsideration.
- **Recognition clues:** "maximum non-overlapping", "minimum number of", sorting reveals an obvious rule.
- **Java template:** sort by key, then iterate making the greedy choice — see Section 6.
- **Time complexity:** O(n log n) time (due to sort), O(1) extra space.
- **Common mistakes:** applying greedy where it doesn't hold, sorting by the wrong key.
- **Top interview questions:** Jump Game, Gas Station, Non-overlapping Intervals, Task Scheduler.
