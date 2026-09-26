# Pattern: Binary Search

---

## 1. Pattern Name
**Binary Search**

---

## 2. Explain the Pattern in Very Simple English

Imagine you're looking for a word in a printed dictionary. You don't start from page 1 and flip one page at a time — that would take forever.

Instead, you open the dictionary in the **middle**. If the word you want comes before that page, you throw away the entire second half and search only the first half. If it comes after, you throw away the first half.

You keep doing this — always jumping to the middle of whatever's left — until you find the word or run out of pages.

That's Binary Search: repeatedly **cut the search space in half** by comparing the middle element, instead of checking every element one by one.

---

## 3. When Should I Think of This Pattern?

- The array is **sorted** (or can be thought of as sorted / monotonic)
- You need to search for an element, or the **first/last position** matching a condition
- The problem says "find minimum X such that condition holds" (binary search on the **answer**, not just the array)
- Time complexity needs to be **O(log n)**
- "Find peak", "search in rotated sorted array", "find in infinite/unknown-size array"
- Whenever brute-force linear scan is O(n) but the problem clearly expects something faster

---

## 4. Mental Trigger

> "Whenever I see a sorted array, or a question asking for the smallest/largest value satisfying some condition, and it expects O(log n), I first think of Binary Search."

---

## 5. Visual Explanation (ASCII)

```
low                mid                high
 ↓                  ↓                   ↓
[1] [3] [5] [7] [9] [11] [13] [15] [17]

target = 13
arr[mid] = 9 -> 13 > 9 -> discard left half, search right half

        low        mid    high
         ↓           ↓      ↓
        [11] [13] [15] [17]
        arr[mid] = 15 -> 13 < 15 -> discard right half

        low  high
         ↓    ↓
        [11] [13]
        found target at correct mid!
```

---

## 6. Generic Java Pseudocode Template

**Classic binary search (find exact element):**
```java
int low = 0, high = arr.length - 1;

while (low <= high) {
    int mid = low + (high - low) / 2;   // avoids integer overflow

    if (arr[mid] == target) {
        return mid;
    } else if (arr[mid] < target) {
        low = mid + 1;
    } else {
        high = mid - 1;
    }
}
return -1; // not found
```

**Binary search on the answer (find smallest value satisfying a condition):**
```java
int low = minPossible, high = maxPossible;

while (low < high) {
    int mid = low + (high - low) / 2;

    if (isValid(mid)) {
        high = mid;       // mid works, try to find something smaller/equal
    } else {
        low = mid + 1;    // mid doesn't work, must go higher
    }
}
return low; // smallest value that satisfies isValid()
```

---

## 7. Time Complexity

| Aspect | Value | Why |
|---|---|---|
| Time | O(log n) | Every step eliminates **half** of the remaining elements, so after k steps only n / 2^k elements remain — this reaches 1 element in about log₂(n) steps |
| Space | O(1) iterative, O(log n) recursive (call stack) | No extra data structure needed for the iterative version |
| Why efficient | Linear scan checks every element → O(n). Binary search discards half the search space every comparison → O(log n), which is dramatically faster for large n |

---

## 8. Common Mistakes

- Using `mid = (low + high) / 2` — this can **overflow** for large arrays; use `low + (high - low) / 2` instead
- Wrong loop condition: `low <= high` vs `low < high` — mixing these up causes infinite loops or missed elements
- Not updating `low`/`high` correctly (`mid` vs `mid + 1` vs `mid - 1`), which can cause infinite loops
- Applying binary search on an **unsorted** array (must sort first, or confirm it's monotonic in some way)
- Forgetting that "binary search on answer" problems need a **monotonic predicate function** (`isValid()` must be false-then-true, or true-then-false, not random)
- Off-by-one when searching for the **first/last occurrence** of a duplicate value

---

## 9. Real Interview Examples

1. **Binary Search** (Easy) — Why it works: textbook case, sorted array, find exact target. Key observation: use `low + (high-low)/2` to avoid overflow.

2. **Search in Rotated Sorted Array** (Medium) — Why it works: even though rotated, **one half** of the array around `mid` is always properly sorted — determine which half is sorted, then decide which side to search. Key observation: modify the standard binary search condition to check which half is sorted first.

3. **Find First and Last Position of Element in Sorted Array** (Medium) — Why it works: run binary search twice — once biased to find the leftmost match, once for the rightmost. Key observation: instead of stopping at first match, keep shrinking `high` (or `low`) to find the boundary.

4. **Koko Eating Bananas** (Medium) — Why it works: binary search on the "eating speed" (the answer), using a helper function to check if a given speed finishes in time. Key observation: classic "binary search on answer" pattern — the array itself isn't searched directly.

5. **Median of Two Sorted Arrays** (Hard) — Why it works: binary search on the smaller array to find the correct partition point such that left and right halves (combined from both arrays) are balanced. Key observation: advanced binary search where the "search space" is a partition index, not an array value.

---

## 10. LeetCode Problems

**Easy**
- [Binary Search](https://leetcode.com/problems/binary-search/)
- [Search Insert Position](https://leetcode.com/problems/search-insert-position/)
- [First Bad Version](https://leetcode.com/problems/first-bad-version/)

**Medium**
- [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
- [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
- [Find Peak Element](https://leetcode.com/problems/find-peak-element/)
- [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) done
- [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

**Hard**
- [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)
- [Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)

---

## 11. Interview Variations

Instead of: "Find the exact target"
They may ask:
- Find the **first/last** occurrence of a duplicate value
- Find the **insertion point** if the target isn't present
- Search in a **rotated** sorted array
- Find a **peak element** (binary search on a "shape", not sorted order)
- **Binary search on the answer** — minimize/maximize some value subject to a feasibility check (Koko Eating Bananas, Split Array Largest Sum, Capacity to Ship Packages)
- Search in a 2D sorted matrix

---

## 12. Pattern Recognition Checklist

```
Is the array sorted / monotonic?
        ↓ yes
Do I need O(log n) instead of O(n)?
        ↓ yes
Am I searching for a value, OR minimizing/maximizing 
an answer with a pass/fail check?
        ↓ either
Think Binary Search (on array, or on the answer space).
```

---

## 13. Similar Patterns

- **Binary Search vs Two Pointers:** Both need sorted data, but Two Pointers moves linearly from both ends comparing pairs (O(n)), while Binary Search jumps to the middle repeatedly to locate one target (O(log n)). Different goals: pairs vs single-value search.
- **Binary Search vs Linear Search:** Linear search checks every element (O(n)) and works on unsorted data; Binary Search needs sorted/monotonic data but is far faster (O(log n)).
- **Binary Search on Array vs Binary Search on Answer:** The first searches within given array indices for a value. The second searches within a **range of possible answers** (e.g., speeds, capacities, days) using a feasibility check function — the array itself may not even be sorted, but the "can this value work?" function must be monotonic.

---

## 14. Revision Summary (Cheat Sheet)

| Aspect | Details |
|---|---|
| **What it does** | Repeatedly halves the search space by comparing the middle element/value |
| **When to use** | Sorted/monotonic array, or minimizing/maximizing an answer with a feasibility check |
| **Recognition clues** | "sorted array", "O(log n)", "find minimum X such that", "rotated sorted array" |
| **Java template** | `while (low <= high) { mid = low + (high-low)/2; ... }` |
| **Time complexity** | O(log n) time, O(1) space (iterative) |
| **Common mistakes** | Overflow in mid calculation, wrong loop condition, infinite loops from bad pointer updates |
| **Top interview Qs** | Binary Search, Search in Rotated Sorted Array, Koko Eating Bananas, Median of Two Sorted Arrays |
