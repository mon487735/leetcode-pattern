# Pattern: Stack

---

## 1. Pattern Name
**Stack (LIFO)**

---

## 2. Explain the Pattern in Very Simple English

Imagine a stack of plates in a kitchen. You can only add a new plate to the **top** of the stack, and you can only remove a plate from the **top** as well. You can never pull a plate from the middle or bottom without first removing everything above it.

This is called **LIFO** — Last In, First Out. The last plate you put on is the first one you take off.

A Stack is useful whenever you need to **remember things temporarily** and undo/process them in reverse order — like keeping track of open brackets, or "going back" in a browser, or remembering which function called which (that's literally how your computer's call stack works).

---

## 3. When Should I Think of This Pattern?

- "Balanced parentheses / brackets" problems
- Need to **reverse** the order of processing (undo, backtrack)
- "Next greater/smaller element" type problems
- Evaluating **expressions** (postfix, infix, calculator problems)
- Matching **nested structures** (HTML tags, nested brackets)
- Anything where you process elements and sometimes need to **look back** at the most recently seen unprocessed item
- Simulating **recursion iteratively** (DFS without recursion)

---

## 4. Mental Trigger

> "Whenever I see 'matching pairs', 'nested structure', or I need to remember the most recent unfinished item to process it later, I first think of Stack."

---

## 5. Visual Explanation (ASCII)

```
push(1)   push(2)   push(3)     pop() -> 3
   
[1]       [1]        [1]           [1]
          [2]         [2]           [2]
                       [3]  ← top

Top of stack = last element pushed = first element popped
```

**Balanced brackets example:**
```
Input: "{[()]}"

'{' -> push       stack: {
'[' -> push       stack: {[
'(' -> push       stack: {[(
')' -> pop, must match '(' ✓   stack: {[
']' -> pop, must match '[' ✓   stack: {
'}' -> pop, must match '{' ✓   stack: (empty)

Empty at the end -> Balanced!
```

---

## 6. Generic Java Pseudocode Template

**Basic stack usage (Java's `Deque` is preferred over the legacy `Stack` class):**
```java
Deque<Integer> stack = new ArrayDeque<>();

stack.push(x);       // add to top
int top = stack.peek();  // look at top without removing
int val = stack.pop();   // remove and return top
boolean empty = stack.isEmpty();
```

**Balanced parentheses template:**
```java
Deque<Character> stack = new ArrayDeque<>();

for (char c : s.toCharArray()) {
    if (isOpeningBracket(c)) {
        stack.push(c);
    } else {
        if (stack.isEmpty() || !matches(stack.pop(), c)) {
            return false; // mismatch
        }
    }
}
return stack.isEmpty(); // must be empty if fully balanced
```

**Monotonic stack template (next greater element):**
```java
Deque<Integer> stack = new ArrayDeque<>(); // stores indices
int[] result = new int[arr.length];
Arrays.fill(result, -1);

for (int i = 0; i < arr.length; i++) {
    while (!stack.isEmpty() && arr[stack.peek()] < arr[i]) {
        int idx = stack.pop();
        result[idx] = arr[i]; // arr[i] is the "next greater" for idx
    }
    stack.push(i);
}
```

---

## 7. Time Complexity

| Aspect | Value | Why |
|---|---|---|
| push/pop/peek | O(1) | Direct access/modification at the top only — no shifting needed |
| Overall traversal (e.g. monotonic stack) | O(n) | Each element is pushed once and popped at most once, so total operations are bounded by 2n |
| Space | O(n) | Worst case, all elements sit on the stack at once |
| Why efficient | Brute force "next greater element" checks every pair → O(n²). A monotonic stack processes each element a constant number of times → O(n) |

---

## 8. Common Mistakes

- Forgetting to check `stack.isEmpty()` before calling `peek()`/`pop()` — causes exceptions
- Using the legacy `java.util.Stack` class (synchronized, slower) instead of `ArrayDeque` (preferred in interviews)
- Pushing/popping the wrong data — e.g., pushing values when indices are needed (common in monotonic stack problems)
- Forgetting that a truly balanced string must result in an **empty stack** at the end (not just "no mismatches during the loop")
- Confusing Stack (LIFO) with Queue (FIFO) — using the wrong one changes the entire algorithm's behavior
- Not clearing/reinitializing the stack between separate test cases or function calls

---

## 9. Real Interview Examples

1. **Valid Parentheses** (Easy) — Why it works: push opening brackets, pop and match on closing brackets; stack must be empty at the end. Key observation: mismatch or leftover items in stack = invalid.

2. **Min Stack** (Medium) — Why it works: maintain a secondary stack that tracks the minimum value at each point, so `getMin()` is O(1). Key observation: track auxiliary state alongside the main stack.

3. **Daily Temperatures** (Medium) — Why it works: classic monotonic stack — for each day, pop from the stack while the current temperature is higher, updating results. Key observation: this is "next greater element" in disguise.

4. **Evaluate Reverse Polish Notation** (Medium) — Why it works: numbers get pushed; operators pop the last two numbers, compute, and push the result back. Key observation: stack naturally models postfix expression evaluation.

5. **Largest Rectangle in Histogram** (Hard) — Why it works: monotonic increasing stack of indices; when a smaller bar is found, pop and calculate area using the popped bar as height. Key observation: an advanced monotonic stack problem — practice "Daily Temperatures" first to build intuition.

---

## 10. LeetCode Problems

**Easy**
- [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) done
- [Baseball Game](https://leetcode.com/problems/baseball-game/) done

**Medium**
- [Min Stack](https://leetcode.com/problems/min-stack/)
- [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)
- [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)
- [Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/)
- [Decode String](https://leetcode.com/problems/decode-string/)

**Hard**
- [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)
- [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) (stack-based alternative solution)

---

## 11. Interview Variations

Instead of: "Check if brackets are balanced"
They may ask:
- Return the **minimum number of insertions/removals** to make it balanced
- Support **multiple bracket types** simultaneously
- Find the **next greater/smaller** element instead of validating structure (monotonic stack)
- Evaluate a **mathematical expression** (calculator problems, with +, -, *, /, parentheses)
- Simulate **recursion iteratively** using an explicit stack (common in DFS/tree problems when recursion depth is a concern)
- Decode **nested encoded strings** (e.g., `3[a2[c]]`)

---

## 12. Pattern Recognition Checklist

```
Do I need to remember the most recent unprocessed item?
        ↓ yes
Is there a nested/matching structure, or do I need 
to look "backward" at recent elements?
        ↓ yes
Would a monotonic order (increasing/decreasing) help?
        ↓ maybe
Think Stack (plain or monotonic).
```

---

## 13. Similar Patterns

- **Stack vs Queue:** Stack is LIFO (last in, first out) — used for reversing order, matching, backtracking. Queue is FIFO (first in, first out) — used for processing things in the order they arrived, like BFS.
- **Stack vs Recursion:** Recursion implicitly uses the **call stack**. Any recursive solution can be rewritten iteratively using an explicit Stack — useful when recursion might cause a stack overflow on deep inputs.
- **Monotonic Stack vs Sliding Window:** Both can solve "next greater element"-style problems, but a monotonic stack handles it in one clean O(n) pass without needing to track window boundaries explicitly.

---

## 14. Revision Summary (Cheat Sheet)

| Aspect | Details |
|---|---|
| **What it does** | LIFO structure — last item added is the first removed |
| **When to use** | Matching/nested structures, expression evaluation, next greater/smaller element, iterative DFS |
| **Recognition clues** | "balanced brackets", "nested", "next greater element", "undo/backtrack" |
| **Java template** | `Deque<T> stack = new ArrayDeque<>(); stack.push(); stack.pop(); stack.peek();` |
| **Time complexity** | O(1) per operation, O(n) overall traversal, O(n) space |
| **Common mistakes** | Not checking `isEmpty()` first, using legacy `Stack` class, confusing with Queue |
| **Top interview Qs** | Valid Parentheses, Min Stack, Daily Temperatures, Evaluate RPN, Largest Rectangle in Histogram |
