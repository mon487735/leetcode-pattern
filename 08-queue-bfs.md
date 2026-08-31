# Pattern: Queue / BFS (Breadth-First Search)

---

## 1. Pattern Name
**Queue / BFS (Breadth-First Search)**

---

## 2. Explain the Pattern in Very Simple English

Imagine you're standing in a ticket line. The first person to join the line is the first person to get served. Nobody can cut in front — new people always join at the **back**, and service always happens from the **front**.

That's a **Queue** — FIFO (First In, First Out).

Now imagine you drop a pebble in a pond. Ripples spread outward in **rings** — first the closest ring, then the next ring, then the next. You never reach a far ring before covering all the closer ones.

That's **BFS** — Breadth-First Search. Starting from a point, you explore everything **one step away** first, then everything **two steps away**, then three, and so on — always level by level, using a Queue to keep track of what to visit next.

---

## 3. When Should I Think of This Pattern?

- "Shortest path" in an **unweighted** graph or grid
- "Level order traversal" of a tree
- "Minimum number of steps/moves" to reach a target
- Explore neighbors **level by level** rather than going deep first
- Multi-source spreading problems ("rotting oranges", "fire spreading")
- Anything about **closest/nearest** in a graph-like structure

---

## 4. Mental Trigger

> "Whenever I need the shortest path in an unweighted graph, or I need to process things level by level, I first think of Queue / BFS."

---

## 5. Visual Explanation (ASCII)

```
        1
      / | \
     2  3  4
    /|      \
   5 6       7

BFS order starting from 1:
Level 0: 1
Level 1: 2, 3, 4
Level 2: 5, 6, 7

Queue trace:
[1]
[2,3,4]        (dequeue 1, enqueue its children)
[3,4,5,6]      (dequeue 2, enqueue its children)
[4,5,6]        (dequeue 3, no children)
...and so on
```

---

## 6. Generic Java Pseudocode Template

**Basic BFS (graph/grid):**
```java
Queue<Node> queue = new LinkedList<>();
Set<Node> visited = new HashSet<>();

queue.offer(start);
visited.add(start);

while (!queue.isEmpty()) {
    Node current = queue.poll();

    // process current

    for (Node neighbor : getNeighbors(current)) {
        if (!visited.contains(neighbor)) {
            visited.add(neighbor);
            queue.offer(neighbor);
        }
    }
}
```

**Level-order traversal (tracking levels explicitly):**
```java
Queue<TreeNode> queue = new LinkedList<>();
queue.offer(root);

while (!queue.isEmpty()) {
    int levelSize = queue.size();   // number of nodes at this level

    for (int i = 0; i < levelSize; i++) {
        TreeNode node = queue.poll();
        // process node

        if (node.left != null) queue.offer(node.left);
        if (node.right != null) queue.offer(node.right);
    }
    // one full level has been processed here
}
```

---

## 7. Time Complexity

| Aspect | Value | Why |
|---|---|---|
| Time | O(V + E) | Each vertex is visited once (V), and each edge is examined once when checking neighbors (E) |
| Space | O(V) | Queue and visited set can hold up to all vertices in the worst case |
| Why efficient | BFS guarantees the **shortest path** in unweighted graphs because it explores in expanding rings — the first time you reach a node is guaranteed to be via the shortest route |

---

## 8. Common Mistakes

- Forgetting to mark nodes as **visited** — causes infinite loops or reprocessing in graphs with cycles
- Marking a node visited **after** dequeuing instead of when **enqueuing** — can cause duplicates in the queue
- Using DFS (recursion/stack) when the problem actually needs the shortest path — DFS does **not** guarantee shortest path in general graphs
- Forgetting to capture `queue.size()` **before** the inner loop when doing level-order traversal (the size changes as you enqueue children mid-loop)
- Not handling **multi-source BFS** correctly (e.g., "rotting oranges" — you must enqueue *all* starting sources at once, not one at a time)

---

## 9. Real Interview Examples

1. **Binary Tree Level Order Traversal** (Medium) — Why it works: process nodes level by level using the queue-size trick. Key observation: capture `queue.size()` before the loop to know exactly how many nodes belong to the current level.

2. **Rotting Oranges** (Medium) — Why it works: multi-source BFS — enqueue *all* initially rotten oranges together, then spread level by level (minutes). Key observation: BFS naturally computes "minimum time for the spread to reach everywhere."

3. **Word Ladder** (Hard) — Why it works: treat each word as a graph node; BFS finds the shortest transformation sequence since each step (word change) has equal "cost." Key observation: BFS is the right choice whenever all edges have equal weight and you want the shortest path.

4. **01 Matrix** (Medium) — Why it works: multi-source BFS starting from all `0`s simultaneously finds the shortest distance to a `0` for every cell in one pass. Key observation: reframe "distance to nearest X" problems as multi-source BFS.

5. **Number of Islands** (Medium) — Why it works: BFS (or DFS) from each unvisited land cell to mark the entire connected island as visited. Key observation: either BFS or DFS works here since we just need connectivity, not shortest path — but BFS with a queue avoids recursion depth issues on large grids.

---

## 10. LeetCode Problems

**Easy**
- [Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/)

**Medium**
- [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
- [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)
- [01 Matrix](https://leetcode.com/problems/01-matrix/)
- [Number of Islands](https://leetcode.com/problems/number-of-islands/)
- [Course Schedule](https://leetcode.com/problems/course-schedule/) (BFS-based topological sort)

**Hard**
- [Word Ladder](https://leetcode.com/problems/word-ladder/)
- [Sliding Puzzle](https://leetcode.com/problems/sliding-puzzle/)

---

## 11. Interview Variations

Instead of: "Find if a path exists"
They may ask:
- Find the **shortest path length** (BFS naturally gives this in unweighted graphs)
- Process **multiple sources** simultaneously (Rotting Oranges, 01 Matrix)
- Return the **actual path**, not just its length (track parent pointers while doing BFS)
- Combine BFS with **state tracking** (e.g., Sliding Puzzle — each "board configuration" is a node)
- Detect **bipartiteness** or perform **topological sort** using BFS (Kahn's algorithm)

---

## 12. Pattern Recognition Checklist

```
Is this graph/tree/grid based?
        ↓ yes
Do I need the SHORTEST path or level-by-level processing?
        ↓ yes
Are all edges/steps equal "cost"?
        ↓ yes
Think Queue / BFS.
```

---

## 13. Similar Patterns

- **BFS vs DFS:** BFS explores level by level (guarantees shortest path in unweighted graphs, uses a Queue). DFS explores as deep as possible before backtracking (uses a Stack or recursion, better for exploring all paths, connectivity, or when memory for BFS's queue would be too large).
- **BFS vs Dijkstra's Algorithm:** BFS finds shortest paths only when all edges have **equal weight**. When edges have different weights, you need Dijkstra's algorithm (BFS + a priority queue/min-heap instead of a plain queue).
- **Queue vs Stack:** Queue = FIFO (BFS). Stack = LIFO (DFS, backtracking). Picking the wrong one completely changes traversal order.

---

## 14. Revision Summary (Cheat Sheet)

| Aspect | Details |
|---|---|
| **What it does** | Explores level by level using a Queue, guaranteeing shortest path in unweighted graphs |
| **When to use** | Shortest path (unweighted), level order traversal, multi-source spreading problems |
| **Recognition clues** | "shortest path", "level order", "minimum steps", "spread/rot/infect" |
| **Java template** | `Queue<Node> q = new LinkedList<>(); q.offer(start); while(!q.isEmpty()) { poll, process, enqueue neighbors }` |
| **Time complexity** | O(V + E) time, O(V) space |
| **Common mistakes** | Not marking visited on enqueue, using DFS when shortest path is needed, wrong level-size capture |
| **Top interview Qs** | Binary Tree Level Order Traversal, Rotting Oranges, 01 Matrix, Word Ladder, Number of Islands |
