# Pattern: Sliding Window

---

## 1. Pattern Name
**Sliding Window**

---

## 2. Explain the Pattern in Very Simple English

Imagine you have a train with a fixed number of windows, and you're moving it slowly along a long wall painted with numbers.

At each moment, you can only see the numbers inside the current window. As the train moves forward, one number leaves from the back of the window, and one new number enters from the front.

Instead of stopping the train and re-reading every number in the window from scratch each time (slow), you just **subtract what left** and **add what entered** (fast).

That's Sliding Window: keep a "window" (a contiguous chunk) over the array/string, and slide it forward, updating the answer incrementally instead of recalculating everything.

---

## 3. When Should I Think of This Pattern?

- Problem involves a **contiguous subarray** or **substring**
- Words like "maximum/minimum sum of subarray of size K"
- "Longest substring with X property" (no repeats, at most K distinct chars, etc.)
- "Smallest subarray with sum ≥ target"
- You'd otherwise use **nested loops** to check every subarray/substring (O(n²) or worse)
- The window size is either **fixed** or **variable** but always contiguous

---

## 4. Mental Trigger

> "Whenever I see 'contiguous subarray' or 'substring' combined with a size, sum, or uniqueness condition, I first think of Sliding Window."

---

## 5. Visual Explanation (ASCII)

**Fixed-size window:**
```
[2][1][5][1][3][2]   k = 3

Window 1: [2 1 5] sum=8
Window 2:    [1 5 1] sum=7   (2 leaves, 1 enters)
Window 3:       [5 1 3] sum=9  (1 leaves, 3 enters)
```

**Variable-size window (shrink/grow):**
```
left                right
 ↓                    ↓
[a][b][c][d][e][f]
      → expand right until condition breaks
      → then shrink left until condition holds again
```

---

## 6. Generic Java Pseudocode Template

**Fixed-size window:**
```java
int windowSum = 0;

// build first window
for (int i = 0; i < k; i++) {
    windowSum += arr[i];
}

int maxSum = windowSum;

// slide the window
for (int i = k; i < arr.length; i++) {
    windowSum += arr[i] - arr[i - k];   // add new, remove old
    maxSum = Math.max(maxSum, windowSum);
}
```

**Variable-size window (expand and shrink):**
```java
int left = 0;
int windowState = 0; // e.g. sum, count of distinct chars, etc.
int best = 0;

for (int right = 0; right < arr.length; right++) {
    // 1. Expand: include arr[right] into windowState

    while (/* window is invalid, e.g. windowState > target */) {
        // 2. Shrink from the left
        left++;
    }

    // 3. Update the answer using current valid window [left..right]
    best = Math.max(best, right - left + 1);
}
```

---

## 7. Time Complexity

| Aspect | Value | Why |
|---|---|---|
| Time | O(n) | Each element is added to the window once and removed once — `right` and `left` each move forward at most n times total, never backward |
| Space | O(1) or O(k) | O(1) for simple sums; O(k) if tracking a HashMap/HashSet of window contents (e.g., character counts) |
| Why efficient | Brute force recomputes the sum/state for every possible window → O(n·k) or O(n²). Sliding window reuses previous work by only adjusting for the element entering/leaving |

---

## 8. Common Mistakes

- Recomputing the window sum/state from scratch every time instead of incrementally updating it
- Forgetting to **shrink** the window when the condition becomes invalid (infinite growth)
- Off-by-one in window size calculation: `right - left + 1`, not `right - left`
- Not handling the case where **no valid window exists** (return 0 / empty)
- Using a fixed-size template when the problem actually needs a **variable-size** window (or vice versa)
- Forgetting to update the "best" answer at the correct point (before or after shrinking, depending on the problem)

---

## 9. Real Interview Examples

1. **Maximum Sum Subarray of Size K** (Easy) — Why it works: fixed-size window; just add the incoming element and subtract the outgoing one. Key observation: never recompute the full sum — pure O(1) update per step.

2. **Longest Substring Without Repeating Characters** (Medium) — Why it works: expand the window; when a repeat is found, shrink from the left until it's valid again. Key observation: use a HashSet/HashMap to track characters currently in the window.

3. **Minimum Size Subarray Sum** (Medium) — Why it works: expand right to grow the sum; once sum ≥ target, shrink from left to find the smallest valid window. Key observation: this is the "grow then shrink" variable window pattern.

4. **Longest Substring with At Most K Distinct Characters** (Medium) — Why it works: track character frequency in a map; shrink the window when distinct count exceeds K. Key observation: window validity is based on the *size of the map*, not just a sum.

5. **Sliding Window Maximum** (Hard) — Why it works: use a **deque** to maintain indices of useful elements in decreasing order, so the max is always at the front. Key observation: a more advanced sliding window that needs an auxiliary data structure, not just two pointers.

---

## 10. LeetCode Problems

**Easy**
- [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/) done
- [Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/) done

**Medium**
- [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
- [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)
- [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)
- [Permutation in String](https://leetcode.com/problems/permutation-in-string/)

**Hard**
- [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)
- [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

---

## 11. Interview Variations

Instead of: "Find the max sum of a fixed-size window"
They may ask:
- Find the **minimum** length window satisfying a condition (variable-size)
- Find the **number of windows** satisfying a condition (count, not just one answer)
- Track **distinct characters/elements** instead of a numeric sum
- Return the actual window (substring/subarray), not just its length or sum
- Combine with a **deque** for max/min tracking inside the window (Sliding Window Maximum)
- Two-pointer + frequency map combo for **anagram-in-string** type problems (Permutation in String, Minimum Window Substring)

---

## 12. Pattern Recognition Checklist

```
Is it about a CONTIGUOUS subarray/substring?
        ↓ yes
Is there a size/sum/uniqueness condition?
        ↓ yes
Would brute force recompute the window from scratch each time?
        ↓ yes
Think Sliding Window.
```

---

## 13. Similar Patterns

- **Sliding Window vs Two Pointers:** Sliding Window is really a *specialized* two-pointer technique where both pointers move in the **same direction** and the region between them (the window) must stay contiguous and valid. Classic Two Pointers (opposite ends) doesn't have this "window" concept.
- **Sliding Window vs Prefix Sum:** Prefix Sum precomputes cumulative sums to answer range-sum queries in O(1) after O(n) preprocessing — useful when the window isn't strictly "slide forward only" or when you need arbitrary range sums. Sliding Window is better when the window naturally grows/shrinks as you scan once.
- **Fixed vs Variable Window:** Don't confuse the two sub-types — fixed-size windows never shrink below k; variable-size windows have a `while` loop to shrink dynamically.

---

## 14. Revision Summary (Cheat Sheet)

| Aspect | Details |
|---|---|
| **What it does** | Maintains a contiguous window over an array/string, updating incrementally as it slides |
| **When to use** | Contiguous subarray/substring problems with size, sum, or uniqueness conditions |
| **Recognition clues** | "subarray", "substring", "contiguous", "at most K", "size K", "longest/shortest window" |
| **Java template** | Fixed: `sum += arr[i] - arr[i-k]`. Variable: `for(right) { expand; while(invalid) shrink; update answer }` |
| **Time complexity** | O(n) time, O(1)–O(k) space |
| **Common mistakes** | Recomputing from scratch, forgetting to shrink, off-by-one in window size |
| **Top interview Qs** | Longest Substring Without Repeating Characters, Minimum Size Subarray Sum, Sliding Window Maximum, Minimum Window Substring |
