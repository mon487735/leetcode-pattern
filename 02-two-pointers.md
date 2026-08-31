# Pattern: Two Pointers

---

## 1. Pattern Name
**Two Pointers**

---

## 2. Explain the Pattern in Very Simple English

Imagine two people standing at opposite ends of a straight road.

One person starts from the left end. Another starts from the right end.

Depending on what you're looking for, one or both of them start walking toward each other, and they stop when they meet or find what they need.

Instead of one person walking the *entire* road checking every combination with every other point (which takes forever), two people moving smartly from both ends can solve the problem much faster — because the road (array) is already **sorted** or has some structure we can exploit.

---

## 3. When Should I Think of This Pattern?

- Array (or string) is **sorted**, or can be sorted
- Need to **find a pair** (sum, difference, product) matching a condition
- Need to **remove duplicates in place**
- Checking for a **palindrome**
- **Merging** two sorted arrays
- Comparing values from **both ends** toward the middle
- Need to do it in O(n) time and O(1) space (no extra data structure)

---

## 4. Mental Trigger

> "Whenever I see a sorted array (or a palindrome-style problem) and need to compare two values without extra space, I first think of Two Pointers."

---

## 5. Visual Explanation (ASCII)

```
left                      right
 ↓                          ↓
[1] [2] [3] [5] [7] [9] [10]

If arr[left] + arr[right] > target -> move right pointer left (right--)
If arr[left] + arr[right] < target -> move left pointer right (left++)
If equal -> found it!
```

Same-direction version (fast/slow within array, e.g. remove duplicates):
```
slow
 ↓
[1][1][2][2][3]
    ↑
   fast (scans ahead, slow only moves when a new unique value is found)
```

---

## 6. Generic Java Pseudocode Template

**Opposite-direction (converging) pointers:**
```java
int left = 0;
int right = arr.length - 1;

while (left < right) {
    int sum = arr[left] + arr[right];

    if (sum == target) {
        return new int[]{left, right};
    } else if (sum < target) {
        left++;   // need a bigger sum, move left pointer right
    } else {
        right--;  // need a smaller sum, move right pointer left
    }
}
```

**Same-direction pointers (in-place modification, e.g. remove duplicates):**
```java
int slow = 0;

for (int fast = 1; fast < arr.length; fast++) {
    if (arr[fast] != arr[slow]) {
        slow++;
        arr[slow] = arr[fast];
    }
}
// slow + 1 is the new length
```

---

## 7. Time Complexity

| Aspect | Value | Why |
|---|---|---|
| Time | O(n) | Each pointer moves at most n times total, so the total work is linear, not n² |
| Space | O(1) | No extra array/map used — just two index variables |
| Why efficient | Brute force checks all pairs → O(n²). Because the array is sorted, moving a pointer in the right direction always eliminates a whole set of impossible pairs at once |

---

## 8. Common Mistakes

- Moving the **wrong pointer** (e.g., moving `left` when you actually needed to shrink from the right)
- Off-by-one errors in the **while condition** (`left < right` vs `left <= right`)
- Forgetting to handle **duplicate values**, causing repeated results (common in 3Sum)
- Infinite loop because neither pointer is moved in some branch
- Not sorting the array first when the problem needs it and doesn't state it's already sorted
- Confusing this with sliding window, and building a "window size" when you actually just need two converging indices

---

## 9. Real Interview Examples

1. **Two Sum II – Input Array Is Sorted** (Easy) — Why it works: sorted array lets you eliminate half the search space each move. Key observation: no need for a HashMap here since the array is already sorted.

2. **Valid Palindrome** (Easy) — Why it works: compare characters from both ends moving inward; mismatch means not a palindrome. Key observation: skip non-alphanumeric characters carefully at both ends.

3. **Container With Most Water** (Medium) — Why it works: start with the widest container and always move the pointer at the **shorter** line inward (since that's the limiting factor). Key observation: moving the taller line can never improve the result.

4. **3Sum** (Medium) — Why it works: fix one number, then use two pointers on the rest of the sorted array to find pairs. Key observation: must skip duplicate values to avoid duplicate triplets.

5. **Trapping Rain Water** (Hard) — Why it works: two pointers track the max height seen from left and right; water trapped at each position depends on the smaller of the two maxes. Key observation: a variation that combines two pointers with running max tracking.

---

## 10. LeetCode Problems

**Easy**
- [Two Sum II – Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
- [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
- [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
- [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

**Medium**
- [3Sum](https://leetcode.com/problems/3sum/)
- [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
- [Sort Colors](https://leetcode.com/problems/sort-colors/)

**Hard**
- [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

---

## 11. Interview Variations

Instead of: "Find a pair"
They may ask:
- Count all pairs
- Print all pairs (not just first)
- Return indices vs return values
- Return minimum-sum pair, or maximum-sum pair
- Return the pair closest to a target (instead of exact match)
- Extend to triplets (3Sum) or quadruplets (4Sum) using two pointers inside a loop

---

## 12. Pattern Recognition Checklist

```
Is the array sorted (or sortable)?
        ↓ yes
Need to find pair/triplet/palindrome/merge?
        ↓ yes
Can I solve it in O(1) space?
        ↓ yes
Think Two Pointers.
```

---

## 13. Similar Patterns

- **Two Pointers vs Sliding Window:** Two Pointers usually converge from both ends toward the middle (or move at different speeds in the same direction for in-place edits). Sliding Window maintains a **contiguous range** and expands/shrinks it — used for subarray/substring problems with a size or sum condition.
- **Two Pointers vs Fast & Slow Pointers:** Fast & Slow is a special same-direction case used mainly for **cycle detection** or finding the middle of a linked list — not for pair-finding in arrays.
- **Two Pointers vs Binary Search:** Both exploit sorted order, but Binary Search eliminates half the space per step to find *one* target; Two Pointers scans linearly from both ends to find *pairs/relationships*.

---

## 14. Revision Summary (Cheat Sheet)

| Aspect | Details |
|---|---|
| **What it does** | Uses two indices moving toward each other (or at different speeds) to avoid nested loops |
| **When to use** | Sorted array, pair-finding, palindrome check, merging, in-place duplicate removal |
| **Recognition clues** | "sorted array", "find pair", "palindrome", "merge", "remove duplicates in place" |
| **Java template** | `while (left < right) { ... left++ or right-- }` |
| **Time complexity** | O(n) time, O(1) space |
| **Common mistakes** | Moving wrong pointer, off-by-one, not handling duplicates, infinite loops |
| **Top interview Qs** | Two Sum II, Valid Palindrome, Container With Most Water, 3Sum, Trapping Rain Water |
