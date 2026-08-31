# Pattern: HashMap / HashSet

---

## 1. Pattern Name
**HashMap / HashSet (Hashing)**

---

## 2. Explain the Pattern in Very Simple English

Imagine you are standing at the entrance of a movie hall with a big notebook.
Every time someone walks in, you write down their name and their seat number in your notebook.

Now, if someone asks you "Is this person inside?" or "What seat does this person have?" — you don't have to search the whole hall. You just open your notebook and look up the name directly. It takes almost no time.

That notebook is a **HashMap**. It stores information as **key → value** pairs, and lets you look things up instantly instead of searching one by one.

A **HashSet** is the same idea, but simpler — it's a notebook where you only write down names, just to check "have I seen this before?" No value attached, just a yes/no memory.

The whole point of hashing: **trade memory for speed**. Instead of scanning an array again and again (slow), you remember what you've already seen (fast).

---

## 3. When Should I Think of This Pattern?

Think HashMap/HashSet when you hear or see:

- "find if a pair exists"
- "count frequency of elements"
- "check for duplicates"
- "have I seen this before?"
- "group items by some property" (anagrams, categories)
- "find the first non-repeating / most frequent element"
- You need **O(1) lookup** instead of scanning again and again
- The brute-force solution uses **nested loops** to compare every element with every other element

---

## 4. Mental Trigger

> "Whenever I catch myself writing a nested loop just to check 'does this exist?' or 'have I seen this before?' — I immediately think HashMap/HashSet."

---

## 5. Visual Explanation (ASCII)

```
Array: [2, 7, 11, 15],  target = 9

Step 1: See 2   -> map = {2:0}
Step 2: See 7   -> is (9-7=2) in map? YES! -> answer = [0,1]

map (notebook):
  key -> value
  2   -> index 0
  7   -> index 1
```

HashSet (just membership, no value):

```
seen = { }
for each num:
    if num in seen -> DUPLICATE FOUND
    else add num to seen
```

---

## 6. Generic Java Pseudocode Template

**HashMap template (store value/index, look up complement):**
```java
Map<Integer, Integer> map = new HashMap<>();

for (int i = 0; i < arr.length; i++) {
    int complement = target - arr[i];

    if (map.containsKey(complement)) {
        // found what we were looking for
        return new int[]{map.get(complement), i};
    }

    map.put(arr[i], i);
}
```

**HashSet template (track "seen" items):**
```java
Set<Integer> seen = new HashSet<>();

for (int num : arr) {
    if (seen.contains(num)) {
        // duplicate / condition met
    }
    seen.add(num);
}
```

**Frequency counting template:**
```java
Map<Character, Integer> freq = new HashMap<>();

for (char c : s.toCharArray()) {
    freq.put(c, freq.getOrDefault(c, 0) + 1);
}
```

---

## 7. Time Complexity

| Operation | Time | Why |
|---|---|---|
| put/get/contains (HashMap) | O(1) average | Hashing converts the key into an array index directly, no searching needed |
| add/contains (HashSet) | O(1) average | Same reason — internally backed by a hash table |
| Overall loop-based solution | O(n) | One pass through the array, O(1) work per element |

- **Space Complexity:** O(n) — in the worst case you store every element in the map/set.
- **Why it beats brute force:** Brute force checks every pair → O(n²). Hashing remembers what you've seen so you check each element only once → O(n).
- Note: worst case (many hash collisions) can degrade to O(n), but in practice and in interviews, treat it as O(1).

---

## 8. Common Mistakes

- Forgetting to check `containsKey`/`contains` **before** inserting the current element (this can cause you to match an element with itself).
- Using the wrong data structure — using a HashMap when a HashSet would do (wastes clarity), or vice versa when you actually need the index/value.
- Not handling **duplicate keys** properly — `put()` overwrites, so if you need the *first* occurrence, don't overwrite blindly.
- Forgetting `getOrDefault()` and causing `NullPointerException` when incrementing counts.
- Assuming HashMap keeps insertion order (it doesn't — use `LinkedHashMap` if you need order).
- Not considering **hashCode/equals** contract issues when using custom objects as keys.

---

## 9. Real Interview Examples

1. **Two Sum** (Easy) — Why it works: instead of checking every pair (O(n²)), store each number's index as you go, and check if the complement was already seen. Key observation: you only need *one* pass if you check-then-insert.

2. **Group Anagrams** (Medium) — Why it works: anagrams share the same sorted-character signature; use that signature as a HashMap key to group words. Key observation: the "key" doesn't have to be a number — it can be any hashable representation.

3. **Longest Consecutive Sequence** (Medium) — Why it works: put all numbers in a HashSet, then only start counting a sequence from numbers that have no predecessor (`num-1` not in set). Key observation: this avoids sorting, achieving O(n) instead of O(n log n).

4. **Subarray Sum Equals K** (Medium) — Why it works: use a HashMap of prefix-sum frequencies; if `(currentSum - k)` was seen before, a valid subarray exists. Key observation: combines HashMap with the Prefix Sum idea.

5. **First Unique Character in a String** (Easy) — Why it works: count frequency with a HashMap, then scan again for the first char with count 1. Key observation: two-pass hashing is often simpler than one clever pass.

---

## 10. LeetCode Problems

**Easy**
- [Two Sum](https://leetcode.com/problems/two-sum/)
- [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)
- [Valid Anagram](https://leetcode.com/problems/valid-anagram/)
- [First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)

**Medium**
- [Group Anagrams](https://leetcode.com/problems/group-anagrams/)
- [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)
- [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)
- [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

**Hard**
- [Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

---

## 11. Interview Variations

Instead of: "Find if a pair exists"
They may ask you to:
- Count how many such pairs exist
- Return all such pairs (not just one)
- Return indices vs return values
- Find the pair with minimum/maximum sum satisfying a condition
- Extend to **three** numbers (Two Sum → Three Sum, often combined with sorting + two pointers)
- Ask for it **without extra space** (forces you to think of sorting + two pointers instead)

---

## 12. Pattern Recognition Checklist

```
Do I need to check existence/frequency of something fast?
        ↓ yes
Would brute force need nested loops to compare elements?
        ↓ yes
Do I care about order of elements? 
        ↓ no  → HashMap / HashSet
        ↓ yes → consider LinkedHashMap, or a different pattern (e.g. sliding window)
```

---

## 13. Similar Patterns

- **HashMap vs Sliding Window:** Both often solve substring/subarray problems. Use HashMap when you need to remember *all* seen items with no positional/contiguous constraint. Use Sliding Window when the problem cares about a **contiguous** range and you need to shrink/grow a window.
- **HashMap vs Prefix Sum:** Prefix Sum + HashMap is a common combo (e.g., Subarray Sum Equals K) — prefix sum gives you a running total, HashMap gives you O(1) lookup of previously seen totals.
- **HashSet vs Sorting:** Sorting also helps find duplicates/pairs, but costs O(n log n) and changes order. HashSet does it in O(n) but uses extra space.

---

## 14. Revision Summary (Cheat Sheet)

| Aspect | Details |
|---|---|
| **What it does** | Stores key→value (or just keys) for instant O(1) lookup |
| **When to use** | Need fast existence check, frequency count, or pairing without nested loops |
| **Recognition clues** | "pair exists", "seen before", "count frequency", "duplicates", "group by" |
| **Java template** | `map.containsKey()` before `map.put()`; `set.contains()` before `set.add()` |
| **Time complexity** | O(n) time, O(n) space |
| **Common mistakes** | Not checking before inserting, forgetting `getOrDefault`, assuming order is preserved |
| **Top interview Qs** | Two Sum, Group Anagrams, Longest Consecutive Sequence, Subarray Sum Equals K |
