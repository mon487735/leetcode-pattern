# Pattern 5: Backtracking

## 1. Pattern Name
**Backtracking**

## 2. Explain the Pattern in Very Simple English
Imagine you're walking through a maze. At every junction, you pick a path and keep walking. If you hit a dead end, you **walk back** to the last junction and try a different path.

Backtracking is exactly this: try a choice, explore further, and if it doesn't work out, **undo the choice** (go back) and try the next option. It's how we explore all possible combinations/permutations without missing any, while avoiding paths that are clearly wrong.

## 3. When Should I Think of This Pattern?
- Problem asks for **all possible** combinations, permutations, or subsets
- Problem mentions **"generate all"**, **"find all solutions"**
- Involves placing items with **constraints** (N-Queens, Sudoku)
- Problem is about making a series of **choices** where some choices may need to be undone
- Words like "partition", "combination", "permutation", "subset"

## 4. Mental Trigger
> "Whenever I need to generate ALL possible ways to do something, and some paths might dead-end, I think of Backtracking."

## 5. Visual Explanation (ASCII)
```
                     []
          /            |            \
        [1]           [2]           [3]
       /    \         /   \
   [1,2]  [1,3]   [2,1]  [2,3]
     |                     |
  [1,2,3]               [2,3,1]... 

Each branch = a choice.
Going back up = "backtrack" (undo the choice).
```

## 6. Generic Java Pseudocode Template
```java
void backtrack(List<Integer> current, /* other state */) {
    if (isSolution(current)) {
        result.add(new ArrayList<>(current)); // save a copy!
        return;
    }

    for (int choice : getChoices()) {
        if (!isValid(choice, current)) continue; // pruning

        current.add(choice);          // 1. Make choice
        backtrack(current);           // 2. Explore
        current.remove(current.size() - 1); // 3. Undo choice (backtrack)
    }
}
```

## 7. Time Complexity
- **Time:** Usually exponential — O(2^n) for subsets, O(n!) for permutations, O(k^n) for combinations with k choices per step — because we're exploring a tree of all possible choice sequences.
- **Space:** O(n) for the recursion depth/current path, plus O(result size) to store all solutions.
- **Why it's still needed despite being exponential:** Some problems fundamentally require exploring all valid combinations (there's no shortcut), so Backtracking with good **pruning** (skipping invalid branches early) is the best approach — it avoids wasting time on branches that can never lead to a valid answer.

## 8. Common Mistakes
- Forgetting to **undo** the choice after exploring (missing the "remove" step) — corrupts state for sibling branches
- Adding the **same reference** to the result list instead of a copy (`new ArrayList<>(current)`), causing all saved results to be identical/empty later
- Not pruning early — checking validity only at the end instead of skipping invalid branches early
- Forgetting to **skip duplicates** when the input has repeated elements (Subsets II, Permutations II)
- Off-by-one in loop start index causing repeated combinations

## 9. Real Interview Examples

| Problem | Difficulty | Why Backtracking Works | Key Observation |
|---|---|---|---|
| Subsets | Medium | Each element is either included or not — binary choice tree | 2^n total subsets |
| Permutations | Medium | Try every unused element at each position | Track "used" elements |
| Combination Sum | Medium | Try each candidate, allow reuse, backtrack when sum exceeds target | Prune when running sum > target |
| N-Queens | Hard | Place queens row by row, backtrack when a placement causes a conflict | Check column/diagonal conflicts before placing |
| Word Search | Medium | Explore grid in all 4 directions, backtrack when path is invalid | Mark visited cells, then unmark on backtrack |

## 10. LeetCode Problems
- Medium: [Subsets](https://leetcode.com/problems/subsets/)
- Medium: [Permutations](https://leetcode.com/problems/permutations/)
- Medium: [Combination Sum](https://leetcode.com/problems/combination-sum/)
- Medium: [Word Search](https://leetcode.com/problems/word-search/)
- Medium: [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)
- Hard: [N-Queens](https://leetcode.com/problems/n-queens/)
- Hard: [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)

## 11. Interview Variations
Instead of "generate all subsets," interviewers may ask you to:
- Generate subsets/permutations with **duplicate elements** (need dedup logic)
- Return **count only**, not the actual combinations (can sometimes switch to DP)
- Add **additional constraints** (e.g., subsets that sum to a target)
- Ask for the **first valid solution only** (stop early instead of exploring everything)
- Combine with **memoization** to avoid recomputation (Backtracking + DP hybrid)

## 12. Pattern Recognition Checklist
```
Does the problem ask for ALL combinations/permutations/subsets/arrangements?
        ↓ yes
Are there constraints that might make some choices invalid?
        ↓ yes
Can I make a choice, recurse, then undo it?
        ↓ yes
→ Think Backtracking
```

## 13. Similar Patterns (Don't Confuse These)
- **Backtracking vs DFS:** Backtracking IS a form of DFS, but with an explicit "undo" step after exploring — DFS on graphs/trees usually doesn't need undoing because nodes aren't being "chosen" into a path the same way.
- **Backtracking vs Dynamic Programming:** DP is used when subproblems overlap and can be cached; Backtracking explores all raw possibilities without reusing previous work (though sometimes combined with memoization).
- **Backtracking vs Greedy:** Greedy makes one choice and never looks back; Backtracking explores all choices and reverses when needed.

## 14. Revision Summary (Cheat Sheet)
- **What it does:** Explores all possible choice sequences using recursion, undoing choices that don't lead anywhere.
- **When to use:** "Generate all" combinations/permutations/subsets, constraint satisfaction (N-Queens, Sudoku).
- **Recognition clues:** "all possible", "combinations", "permutations", "subsets", "arrangements".
- **Java template:** choose → recurse → un-choose — see Section 6.
- **Time complexity:** Exponential (varies by problem), O(n) extra space for recursion path.
- **Common mistakes:** forgetting to undo choice, saving reference instead of copy, missing duplicate skip logic.
- **Top interview questions:** Subsets, Permutations, Combination Sum, N-Queens, Word Search.
