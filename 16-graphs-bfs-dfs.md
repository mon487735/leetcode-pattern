# Pattern 8: Graphs (BFS & DFS)

## 1. Pattern Name
**Graphs — Breadth-First Search (BFS) & Depth-First Search (DFS)**

## 2. Explain the Pattern in Very Simple English
Imagine a city map with roads connecting places. If you want to visit every place reachable from your house, you have two strategies:

- **DFS (Depth-First Search):** Go as far as you can down one road, and only turn back when you hit a dead end (like exploring a maze by always taking the first available turn).
- **BFS (Breadth-First Search):** Check all places **one step away** first, then all places two steps away, then three, and so on (like ripples spreading out in water).

DFS uses a stack (or recursion); BFS uses a queue. Both visit every reachable node, but in a different order — and that order matters for different problems (e.g., BFS naturally finds the **shortest path** in an unweighted graph).

## 3. When Should I Think of This Pattern?
- Problem involves a **graph, grid, or tree** with connections
- Asks to find if a **path exists** between two nodes
- Asks for the **shortest path** in an unweighted graph → BFS
- Asks to explore **all connected components**
- Asks about **islands, regions, or flood fill** in a grid
- Words like "connected", "reachable", "shortest path", "levels"

## 4. Mental Trigger
> "Whenever I see a graph or grid and need to explore connections, I think: shortest path or level-by-level? → BFS. Just need to visit everything or find any path? → DFS."

## 5. Visual Explanation (ASCII)
```
Graph:      1
           / \
          2   3
         /     \
        4       5

DFS order: 1 → 2 → 4 → 3 → 5   (goes deep first)
BFS order: 1 → 2 → 3 → 4 → 5   (goes level by level)

BFS visual (ripple):
Level 0: [1]
Level 1: [2, 3]
Level 2: [4, 5]
```

## 6. Generic Java Pseudocode Template

**DFS (recursive):**
```java
void dfs(int node, boolean[] visited, List<List<Integer>> graph) {
    if (visited[node]) return;
    visited[node] = true;

    // process node here

    for (int neighbor : graph.get(node)) {
        dfs(neighbor, visited, graph);
    }
}
```

**BFS (iterative with queue):**
```java
Queue<Integer> queue = new LinkedList<>();
boolean[] visited = new boolean[n];

queue.offer(start);
visited[start] = true;

while (!queue.isEmpty()) {
    int node = queue.poll();
    // process node here

    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            visited[neighbor] = true;
            queue.offer(neighbor);
        }
    }
}
```

## 7. Time Complexity
- **Time:** O(V + E) — every vertex (V) is visited once, and every edge (E) is examined once when checking neighbors.
- **Space:** O(V) — for the visited array, plus the recursion stack (DFS) or queue (BFS), which can hold up to V nodes in the worst case.
- **Why it's efficient:** Without tracking visited nodes, you'd re-explore the same nodes infinitely (especially in graphs with cycles). Marking nodes visited ensures each node/edge is processed exactly once.

## 8. Common Mistakes
- Forgetting to **mark nodes as visited**, causing infinite loops on graphs with cycles
- Marking a node visited **too late** (after adding to queue vs before) — in BFS, mark as visited when adding to the queue, not when processing, to avoid adding the same node multiple times
- Using DFS when the problem specifically needs the **shortest path** (DFS doesn't guarantee shortest path in unweighted graphs; BFS does)
- Not handling **disconnected components** — need to loop over all nodes and start a fresh BFS/DFS for unvisited ones
- Stack overflow with recursive DFS on very deep/large graphs (convert to iterative DFS with an explicit stack if needed)

## 9. Real Interview Examples

| Problem | Difficulty | Why BFS/DFS Works | Key Observation |
|---|---|---|---|
| Number of Islands | Medium | DFS/BFS flood-fill marks all connected land cells | Each new unvisited land cell starts a new island |
| Clone Graph | Medium | Traverse graph while creating copies, using a map to avoid re-cloning | Track original-to-clone mapping |
| Course Schedule | Medium | Detect cycle in a directed graph using DFS with states | Combines with Topological Sort concepts |
| Rotting Oranges | Medium | BFS spreads rot level by level, naturally modeling time steps | Multi-source BFS starting from all rotten oranges at once |
| Word Ladder | Hard | BFS finds shortest transformation sequence | Each word is a node, one-letter difference is an edge |

## 10. LeetCode Problems
- Medium: [Number of Islands](https://leetcode.com/problems/number-of-islands/)
- Medium: [Clone Graph](https://leetcode.com/problems/clone-graph/)
- Medium: [Course Schedule](https://leetcode.com/problems/course-schedule/)
- Medium: [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)
- Medium: [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)
- Hard: [Word Ladder](https://leetcode.com/problems/word-ladder/)

## 11. Interview Variations
Instead of "is there a path," interviewers may ask you to:
- Find the **shortest path length** (BFS with level tracking)
- Count the **number of connected components/islands**
- Detect a **cycle** in directed or undirected graphs
- Perform a **multi-source BFS** (start from multiple nodes simultaneously — Rotting Oranges)
- Apply BFS/DFS on an **implicit graph** (like word transformations, not an explicit adjacency list)

## 12. Pattern Recognition Checklist
```
Is there a graph, grid, or tree with connections?
        ↓ yes
Do I need shortest path / level order?
        ↓ yes → BFS
Do I just need to explore all reachable nodes / find any path / count components?
        ↓ yes → DFS
```

## 13. Similar Patterns (Don't Confuse These)
- **BFS vs DFS:** BFS guarantees shortest path in unweighted graphs and explores level-by-level (uses queue); DFS goes deep first and is often simpler to write recursively but doesn't guarantee shortest path.
- **BFS vs Dijkstra's Algorithm:** BFS works for unweighted graphs; Dijkstra's (using a priority queue) is needed when edges have different weights.
- **Graph DFS vs Backtracking:** Graph DFS just visits/marks nodes; Backtracking (often tree-based) makes choices, explores, and explicitly undoes them — Graph DFS usually doesn't "undo" the visited marking (except in path-finding problems).

## 14. Revision Summary (Cheat Sheet)
- **What it does:** Explores nodes in a graph/grid either level-by-level (BFS) or as deep as possible first (DFS).
- **When to use:** Path existence, shortest path (BFS), connected components, flood fill.
- **Recognition clues:** "graph", "grid", "islands", "shortest path", "connected", "reachable".
- **Java template:** DFS recursive with visited array; BFS iterative with queue — see Section 6.
- **Time complexity:** O(V + E) time, O(V) space.
- **Common mistakes:** forgetting visited marking, using DFS for shortest-path problems, missing disconnected components.
- **Top interview questions:** Number of Islands, Course Schedule, Rotting Oranges, Word Ladder.
