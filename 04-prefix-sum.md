# Pattern: Prefix Sum

---

## 1. Pattern Name
**Prefix Sum**

---

## 2. Explain the Pattern in Very Simple English

Imagine you're tracking your total savings every day in a diary. Instead of writing just "today I earned ₹500", you write your **running total**: "Day 1: ₹500 total, Day 2: ₹1200 total, Day 3: ₹1800 total".

Now, if someone asks "How much did I earn between Day 2 and Day 3?" — you don't need to add up individual days again. You just do: `total(Day 3) - total(Day 2)`.

That's Prefix Sum: precompute the **running total** up to every index once, so that the sum of *any range* can be answered instantly using subtraction, instead of adding elements one by one every single time.

---

## 3. When Should I Think of This Pattern?

- You need the **sum of a range/subarray** multiple times
- "Range sum query" mentioned
- "Number of subarrays with sum equal to K"
- You'd otherwise recompute a sum in a loop **repeatedly** for different ranges (wasteful)
- Involves cumulative totals, running balances, or "sum so far"
- Often combined with a **HashMap** when counting subarrays (not just finding one)

---

## 4. Mental Trigger

> "Whenever I need the sum of a range more than once, or I keep seeing 'subarray sum', I first think of Prefix Sum."

---

## 5. Visual Explanation (ASCII)

```
arr:     [2, 4, 1, 5, 3]
prefix:  [2, 6, 7, 12, 15]
          (running total as you go left to right)

Sum of range [1..3] (values 4,1,5) = prefix[3] - prefix[0] = 12 - 2 = 10
```

With a leading 0 for easier indexing:
```
index:    0  1  2  3  4  5
prefix: [ 0, 2, 6, 7, 12, 15]

Sum of arr[i..j] = prefix[j+1] - prefix[i]
```

---

## 6. Generic Java Pseudocode Template

**Building the prefix sum array:**
```java
int[] prefix = new int[arr.length + 1];
prefix[0] = 0;

for (int i = 0; i < arr.length; i++) {
    prefix[i + 1] = prefix[i] + arr[i];
}

// sum of arr[i..j] inclusive:
int rangeSum = prefix[j + 1] - prefix[i];
```

**Counting subarrays with sum == k (Prefix Sum + HashMap):**
```java
Map<Integer, Integer> prefixCount = new HashMap<>();
prefixCount.put(0, 1);  // empty prefix

int sum = 0, count = 0;

for (int num : arr) {
    sum += num;

    if (prefixCount.containsKey(sum - k)) {
        count += prefixCount.get(sum - k);
    }

    prefixCount.put(sum, prefixCount.getOrDefault(sum, 0) + 1);
}
```

---

## 7. Time Complexity

| Aspect | Value | Why |
|---|---|---|
| Preprocessing | O(n) | One pass to build the prefix array |
| Each range query | O(1) | Just a subtraction: `prefix[j+1] - prefix[i]` |
| Space | O(n) | Extra array to store cumulative sums |
| Why efficient | Brute force recalculates sum for every query in O(n) → O(n·q) for q queries. Prefix sum answers each query in O(1) after one O(n) setup |

---

## 8. Common Mistakes

- Off-by-one errors — forgetting that `prefix[i]` usually represents the sum of the **first i elements**, not including index i
- Not initializing `prefix[0] = 0` (or `prefixCount.put(0, 1)` in the HashMap version) — this misses subarrays that start from index 0
- Recomputing prefix sums inside a loop when they should be precomputed once
- Using this pattern for problems that need a **contiguous window with variable size** (Sliding Window is often better there) instead of a fixed range-sum query
- Forgetting prefix sums can be negative — don't assume sums only increase (they can decrease if array has negative numbers)

---

## 9. Real Interview Examples

1. **Range Sum Query – Immutable** (Easy) — Why it works: precompute prefix sums once; answer every future range-sum query in O(1). Key observation: perfect textbook use-case for prefix sum.

2. **Subarray Sum Equals K** (Medium) — Why it works: track how many times each prefix sum has occurred; if `(currentSum - k)` was seen before, that many subarrays end at the current index with sum k. Key observation: combines prefix sum with HashMap for O(n) counting.

3. **Product of Array Except Self** (Medium) — Why it works: it's the same idea applied to products instead of sums — prefix product from the left, suffix product from the right. Key observation: a variation of prefix "sum" generalized to any associative operation.

4. **Continuous Subarray Sum** (Medium) — Why it works: use prefix sum **modulo k**; if two prefix sums have the same remainder, the subarray between them is divisible by k. Key observation: prefix sum combined with modular arithmetic.

5. **Maximum Size Subarray Sum Equals k** (Medium) — Why it works: store the *first* index where each prefix sum occurs; look for `(sum - k)` to compute max length. Key observation: store index instead of count when you need the *length* of the subarray, not the *count*.

---

## 10. LeetCode Problems

**Easy**
- [Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/) done
- [Find Pivot Index](https://leetcode.com/problems/find-pivot-index/) done

**Medium**
- [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)
- [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)
- [Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)
- [Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/)

**Hard**
- [Count of Range Sum](https://leetcode.com/problems/count-of-range-sum/)

---

## 11. Interview Variations

Instead of: "Find sum of a range"
They may ask:
- **Count** how many subarrays have a given sum (Prefix Sum + HashMap)
- Find the **longest** subarray with a given sum
- Extend to **2D prefix sum** (matrix range sum queries)
- Use prefix **XOR** instead of prefix sum (for subarray XOR problems)
- Combine with **modulo** arithmetic (divisibility problems)
- Prefix **product** instead of prefix sum (Product of Array Except Self)

---

## 12. Pattern Recognition Checklist

```
Do I need range-sum info repeatedly?
        ↓ yes
Is recomputation from scratch each time wasteful?
        ↓ yes
Am I counting/finding subarrays based on their sum?
        ↓ yes
Think Prefix Sum (often + HashMap).
```

---

## 13. Similar Patterns

- **Prefix Sum vs Sliding Window:** Sliding Window works great when the array has only **non-negative** numbers and the window naturally grows/shrinks in one pass. Prefix Sum works even with **negative numbers**, and is better when you need **arbitrary range sums** or need to count subarrays rather than just find the longest/shortest one.
- **Prefix Sum vs HashMap:** They're often combined — Prefix Sum generates the running totals, HashMap gives O(1) lookup of "have I seen this total before?" for counting subarrays.
- **Prefix Sum vs Dynamic Programming:** Prefix sum is technically a very simple 1D DP (each value depends on the previous one) — recognizing this helps when problems ask for cumulative statistics of any kind (sum, product, min, max-so-far).

---

## 14. Revision Summary (Cheat Sheet)

| Aspect | Details |
|---|---|
| **What it does** | Precomputes running totals so any range sum can be found via subtraction |
| **When to use** | Repeated range-sum queries, subarray sum counting, cumulative statistics |
| **Recognition clues** | "range sum", "subarray sum equals k", "cumulative", "running total" |
| **Java template** | `prefix[i+1] = prefix[i] + arr[i]`; range sum = `prefix[j+1] - prefix[i]` |
| **Time complexity** | O(n) build, O(1) per query, O(n) space |
| **Common mistakes** | Off-by-one indexing, forgetting `prefix[0]=0`, ignoring negative numbers |
| **Top interview Qs** | Range Sum Query – Immutable, Subarray Sum Equals K, Product of Array Except Self, Continuous Subarray Sum |
