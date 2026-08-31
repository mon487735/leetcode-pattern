# Pattern: Fast & Slow Pointer (Floyd's Cycle Detection)

---

## 1. Pattern Name
**Fast & Slow Pointer**

---

## 2. Explain the Pattern in Very Simple English

Imagine two runners on a circular track. One runs at normal speed (the "slow" runner, taking 1 step at a time). The other runs twice as fast (the "fast" runner, taking 2 steps at a time).

If the track is a **loop**, the fast runner will eventually **lap** the slow runner and they will meet again somewhere on the track — this proves there's a cycle.

If the track is **not** a loop (it's a straight line with an end), the fast runner will simply reach the end first — no meeting happens.

This idea — one pointer moving 1 step, another moving 2 steps — is also useful for finding the **middle** of a list: when the fast pointer reaches the end, the slow pointer is exactly at the middle (because it moved half as far).

---

## 3. When Should I Think of This Pattern?

- Working with a **Linked List** (most common context)
- "Detect a cycle / loop" in a linked list
- "Find the start of the cycle"
- "Find the middle of a linked list" in one pass
- "Find if a number is happy" (implicit cycle in a sequence, not a literal list)
- You want to avoid using extra space (like a HashSet to track visited nodes)

---

## 4. Mental Trigger

> "Whenever I'm dealing with a linked list and need to detect a cycle or find the middle without extra space, I first think of Fast & Slow Pointers."

---

## 5. Visual Explanation (ASCII)

**Finding the middle:**
```
slow
 ↓
[1] -> [2] -> [3] -> [4] -> [5] -> null
 ↑
fast

Step 1: slow moves 1, fast moves 2
     slow: [2]      fast: [3]
Step 2: slow moves 1, fast moves 2
     slow: [3]      fast: [5]
Step 3: fast can't move 2 more (reaches null) -> STOP
     slow is now at the middle: [3]
```

**Cycle detection:**
```
[1] -> [2] -> [3] -> [4]
              ↑       ↓
              [6] <- [5]

slow moves 1 step, fast moves 2 steps.
Eventually, fast "laps" slow inside the loop and they meet
-> cycle confirmed.
If fast ever reaches null -> no cycle.
```

---

## 6. Generic Java Pseudocode Template

**Cycle detection (Floyd's algorithm):**
```java
ListNode slow = head;
ListNode fast = head;

while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;

    if (slow == fast) {
        // cycle detected
        return true;
    }
}
return false; // fast reached the end -> no cycle
```

**Find middle of linked list:**
```java
ListNode slow = head;
ListNode fast = head;

while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}
// slow is now at the middle node
```

**Find the start of the cycle (after detecting one):**
```java
ListNode slow = head, fast = head;

while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow == fast) break;   // meeting point found
}

if (fast == null || fast.next == null) return null; // no cycle

slow = head;
while (slow != fast) {
    slow = slow.next;
    fast = fast.next;          // both move 1 step now
}
return slow; // start of the cycle
```

---

## 7. Time Complexity

| Aspect | Value | Why |
|---|---|---|
| Time | O(n) | Even though "fast" moves twice as far, the total distance covered is still bounded linearly by the list length; in a cycle, they meet within one loop traversal |
| Space | O(1) | Only two pointer variables used — no extra HashSet needed to track visited nodes |
| Why efficient | The naive approach uses a HashSet to track visited nodes → O(n) space. Fast & Slow achieves the same detection in O(1) space by using relative speed difference instead of memory |

---

## 8. Common Mistakes

- Not checking `fast != null && fast.next != null` in the loop condition — causes a `NullPointerException` when the list has no cycle
- Confusing which pointer moves how many steps (slow=1, fast=2 — mixing these up breaks the meeting guarantee)
- For finding the **cycle start**, forgetting to reset one pointer back to `head` and move both at speed 1 after they first meet
- Using this pattern on a problem that doesn't actually have a "sequence with possible repetition" structure (e.g., don't force it onto normal array pair-finding — that's Two Pointers, not Fast & Slow)
- Off-by-one when finding the middle of an **even-length** list (decide up front: do you want the first middle or second middle node?)

---

## 9. Real Interview Examples

1. **Linked List Cycle** (Easy) — Why it works: if there's a cycle, the fast pointer will eventually catch up to the slow pointer inside the loop. Key observation: no extra space needed, unlike the HashSet approach.

2. **Linked List Cycle II** (Medium) — Why it works: after detecting a meeting point, resetting one pointer to head and moving both at speed 1 mathematically guarantees they meet exactly at the cycle's start. Key observation: relies on a mathematical proof about distances (worth memorizing the two-phase approach, not just the code).

3. **Middle of the Linked List** (Easy) — Why it works: fast moves twice as fast, so when it finishes the list, slow is exactly halfway. Key observation: solves "find middle" in a single pass, no need to count length first.

4. **Happy Number** (Easy) — Why it works: repeatedly summing squared digits either reaches 1 or falls into a cycle — treat the "next number" function like linked-list `.next` and apply the same fast/slow trick. Key observation: this pattern isn't limited to literal linked lists — any sequence generated by a repeatable function can have "cycles."

5. **Palindrome Linked List** (Medium) — Why it works: use fast/slow to find the middle, reverse the second half, then compare both halves. Key observation: fast & slow is often a *building block* combined with other techniques (list reversal) for a bigger solution.

---

## 10. LeetCode Problems

**Easy**
- [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
- [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)
- [Happy Number](https://leetcode.com/problems/happy-number/)

**Medium**
- [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
- [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)
- [Reorder List](https://leetcode.com/problems/reorder-list/)
- [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)

**Hard**
- (Fast & Slow is rarely the core of Hard problems on its own — it's usually a helper step within harder linked-list manipulation problems)

---

## 11. Interview Variations

Instead of: "Detect if a cycle exists"
They may ask:
- Find the **starting node** of the cycle (not just whether it exists)
- Find the **length** of the cycle
- Find the **middle** node for further processing (e.g., before reversing half a list)
- Apply the concept to a **non-list sequence** (Happy Number, Find the Duplicate Number — treating array indices like `.next` pointers)
- Combine with **reversal** (Palindrome Linked List, Reorder List)

---

## 12. Pattern Recognition Checklist

```
Is this a Linked List problem?
        ↓ yes
Do I need to detect a cycle, find the middle, 
or find a repeating pattern in a sequence?
        ↓ yes
Do I want to avoid extra space (HashSet)?
        ↓ yes
Think Fast & Slow Pointers.
```

---

## 13. Similar Patterns

- **Fast & Slow vs Two Pointers:** Two Pointers usually works on **arrays** with pointers moving toward each other or comparing values. Fast & Slow works mainly on **linked lists/sequences** with pointers moving in the *same direction* at *different speeds* — no "meeting from both ends" concept since linked lists can't be indexed from the back easily.
- **Fast & Slow vs HashSet cycle detection:** A HashSet can also detect cycles (store visited nodes, check for repeats) in O(n) time but O(n) space. Fast & Slow does it in O(1) space — this space trade-off is a common interview follow-up question ("can you do it without extra space?").
- **Fast & Slow vs Sliding Window:** Unrelated in purpose — Sliding Window works over contiguous ranges in arrays/strings; Fast & Slow works over pointer chains (linked structures or repeatable functions).

---

## 14. Revision Summary (Cheat Sheet)

| Aspect | Details |
|---|---|
| **What it does** | Uses two pointers moving at different speeds (1 step vs 2 steps) to detect cycles or find the middle |
| **When to use** | Linked list cycle detection, finding the middle, or detecting cycles in a repeatable sequence |
| **Recognition clues** | "linked list", "detect cycle", "find middle", "repeating sequence" |
| **Java template** | `while (fast != null && fast.next != null) { slow = slow.next; fast = fast.next.next; }` |
| **Time complexity** | O(n) time, O(1) space |
| **Common mistakes** | Missing null checks, confusing pointer speeds, forgetting the two-phase approach for finding cycle start |
| **Top interview Qs** | Linked List Cycle, Linked List Cycle II, Middle of the Linked List, Happy Number, Find the Duplicate Number |
