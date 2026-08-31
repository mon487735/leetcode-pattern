# Pattern: DFS (Depth-First Search)

---

## 1. Pattern Name
**DFS (Depth-First Search)**

---

## 2. Explain the Pattern in Very Simple English

Imagine you're exploring a maze. At every junction, you pick **one direction** and keep walking as far as you can — all the way to a dead end — before turning back and trying the next unexplored direction from the last junction.

You don't explore all directions "a little bit" at once (that would be BFS). Instead, you commit fully to one path, go as deep as possible, and only backtrack when you truly can't go further.

That's **DFS** — Depth-First Search. You dive as deep as possible into one branch before backtracking to try another. It's usually implemented with **recursion** (the call stack handles the "backtracking" for you) or an explicit Stack.

---

## 3. When Should I Think of This Pattern?

- Exploring **all paths** or **all possibilities** (not just the shortest one)
- Tree/graph problems where you need to go deep before backtracking
- "Connected components" / "number of islands" style problems
- Problems involving **backtracking** (permutations, combinations, subsets — DFS is the engine behind backtracking)
- Detecting **cycles** in a graph
- When the problem doesn't specifically need "shortest path" (if it does, prefer BFS)
- Post-order/pre-order/in-order type processing on trees

---

## 4. Mental Trigger

> "Whenever I need to explore every possible path, go deep into a structure before backtracking, or the problem doesn't specifically require 'shortest,' I first think of DFS."

---

## 5. Visual Explanation (ASCII)

```
        1
      / | \
     2  3  4
    /|      \
   5 6       7

DFS order (going deep first): 1 -> 2 -> 5 -> 6 -> 3 -> 4 -> 7

Call stack visualization (recursive DFS):
visit(1)
  visit(2)
    visit(5)   <- goes all the way down first
    visit(6)
  visit(3)
  visit(4)
    visit(7)
```

**Backtracking flavor (DFS with undo):**
```
choose -> explore -> un-choose (backtrack)

[ ]  -> choose 1 -> [1]
                      -> choose 2 -> [1,2]  (dead end / found solution)
                      <- backtrack (undo 2) -> [1]
                      -> choose 3 -> [1,3]
                      <- backtrack (undo 3) -> [1]
       <- backtrack (undo 1) -> [ ]
```

---

## 6. Generic Java Pseudocode Template

**Recursive DFS (graph/grid):**
```java
void dfs(Node node, Set<Node> visited) {
    if (node == null || visited.contains(node)) return;

    visited.add(node);
    // process node

    for (Node neighbor : getNeighbors(node)) {
        dfs(neighbor, visited);
    }
}
```

**Iterative DFS (using explicit Stack):**
```java
Deque<Node> stack = new ArrayDeque<>();
Set<Node> visited = new HashSet<>();

stack.push(start);

while (!stack.isEmpty()) {
    Node current = stack.pop();

    if (visited.contains(current)) continue;
    visited.add(current);
    // process current

    for (Node neighbor : getNeighbors(current)) {
        if (!visited.contains(neighbor)) {
            stack.push(neighbor);
        }
    }
}
```

**DFS with backtracking (used for permutations/combinations/subsets):**
```java
void backtrack(List<Integer> current, boolean[] used, int[] nums, List<List<Integer>> result) {
    if (current.size() == nums.length) {
        result.add(new ArrayList<>(current));  // found a full solution
        return;
    }

    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;

        used[i] = true;
        current.add(nums[i]);          // choose

        backtrack(current, used, nums, result);  // explore

        current.remove(current.size() - 1);  // un-choose (backtrack)
        used[i] = false;
    }
}
```

---

## 7. Time Complexity

| Aspect | Value | Why |
|---|---|---|
| Graph/Tree traversal DFS | O(V + E) | Every vertex is visited once, every edge examined once |
| Backtracking DFS (permutations, subsets) | O(2^n) or O(n!) depending on the problem | You're exploring the full search tree of choices — exponential by nature, since you're generating all valid combinations |
| Space | O(h) for recursion stack | h = height of the recursion tree (tree height, or path length in a graph) |
| Why it's the right tool | For "explore everything" problems, there's no shortcut — you genuinely must visit every valid branch. DFS does this with minimal extra memory (just the call stack) |

---

## 8. Common Mistakes

- Forgetting to mark nodes as **visited**, causing infinite recursion on graphs with cycles
- Forgetting to **un-mark** (backtrack) visited/used state when doing combinatorial DFS (permutations/subsets) — this corrupts future branches
- Stack overflow on very deep/large graphs when using pure recursion — consider the iterative stack-based version for large inputs
- Confusing pre-order, in-order, and post-order traversal order on trees (each has a specific place to "process" the node relative to its children)
- Using DFS when the problem explicitly needs the **shortest path** in an unweighted graph (BFS is correct there, not DFS)
- Not handling the **base case** properly in recursive DFS (missing null check causes NPE)

---

## 9. Real Interview Examples

1. **Number of Islands** (Medium) — Why it works: DFS from every unvisited land cell marks the whole connected island, "sinking" it as visited. Key observation: DFS/BFS are interchangeable here since we only care about connectivity, not path length.

2. **Path Sum** (Easy) — Why it works: DFS down each root-to-leaf path, subtracting node values, checking if remaining sum reaches 0 exactly at a leaf. Key observation: classic tree DFS with a running accumulator passed down the recursion.

3. **Subsets** (Medium) — Why it works: DFS/backtracking explores "include this element" vs "exclude this element" at every step, generating every possible subset. Key observation: backtracking DFS naturally maps to a binary decision tree.

4. **Course Schedule** (Medium) — Why it works: DFS detects a **cycle** in the course dependency graph (using a "currently visiting" state, not just "visited") — a cycle means the courses can't all be completed. Key observation: cycle detection needs 3 states (unvisited, visiting, visited), not just 2.

5. **Word Search** (Medium) — Why it works: DFS explores each possible path letter-by-letter through the grid, backtracking (un-marking the cell) when a path doesn't lead to the full word. Key observation: classic grid DFS + backtracking combo.

---

## 10. LeetCode Problems

**Easy**
- [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
- [Path Sum](https://leetcode.com/problems/path-sum/)
- [Flood Fill](https://leetcode.com/problems/flood-fill/)

**Medium**
- [Number of Islands](https://leetcode.com/problems/number-of-islands/)
- [Subsets](https://leetcode.com/problems/subsets/)
- [Course Schedule](https://leetcode.com/problems/course-schedule/)
- [Word Search](https://leetcode.com/problems/word-search/)
- [Clone Graph](https://leetcode.com/problems/clone-graph/)

**Hard**
- [Word Search II](https://leetcode.com/problems/word-search-ii/)
- [N-Queens](https://leetcode.com/problems/n-queens/)

---

## 11. Interview Variations

Instead of: "Visit every node"
They may ask:
- Find **all paths** from root to leaf (not just check existence)
- **Count** connected components / islands
- Detect a **cycle** in the graph
- Generate all **permutations/combinations/subsets** (backtracking DFS)
- Find **connected components** and label them separately
- Combine DFS with **memoization** to avoid recomputation (turns exponential DFS into polynomial DP — very common Medium/Hard combo)

---

## 12. Pattern Recognition Checklist

```
Do I need to explore ALL paths/possibilities, not just the shortest?
        ↓ yes
Is it a tree, graph, or grid structure? 
        ↓ yes
Do I need to go deep before backtracking (e.g. generate combinations)?
        ↓ yes
Think DFS (recursive or with an explicit stack).
```

---

## 13. Similar Patterns

- **DFS vs BFS:** DFS goes deep first (Stack/recursion) — good for exploring all paths, connectivity, cycle detection. BFS goes level by level (Queue) — good for shortest path in unweighted graphs. If the question says "shortest," lean BFS; if it says "all paths" or "any path," lean DFS.
- **DFS vs Backtracking:** Backtracking *is* DFS, but with an explicit "undo" step after exploring each choice — used specifically for generating combinatorial results (permutations, subsets, N-Queens) rather than just traversing a fixed structure.
- **DFS vs Heap/Sorting:** Unrelated in general, but sometimes combined — e.g., DFS on a graph while using a heap to always expand the cheapest edge (this becomes more like Dijkstra/Prim's, a different family of algorithms).

---

## 14. Revision Summary (Cheat Sheet)

| Aspect | Details |
|---|---|
| **What it does** | Explores as deep as possible along each branch before backtracking |
| **When to use** | Explore all paths, connectivity, cycle detection, backtracking (permutations/subsets) |
| **Recognition clues** | "all paths", "connected components", "generate all combinations", "explore/backtrack" |
| **Java template** | Recursive: `dfs(node) { mark visited; process; for neighbor: dfs(neighbor); }` |
| **Time complexity** | O(V+E) for traversal; O(2^n)/O(n!) for backtracking; O(h) space |
| **Common mistakes** | Missing visited marks, forgetting to backtrack/un-mark, stack overflow on deep recursion |
| **Top interview Qs** | Number of Islands, Subsets, Course Schedule, Word Search, N-Queens |
