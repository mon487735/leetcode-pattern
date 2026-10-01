# Pattern 10: Topological Sort

## 1. Pattern Name
**Topological Sort**

## 2. Explain the Pattern in Very Simple English
Imagine you're getting dressed. You must put on **socks before shoes**, and **shirt before jacket**. There's an order — some things must come before others.

Topological Sort finds a valid ordering of tasks/items such that every "must come before" rule (a directed edge) is respected. It only works when there's **no circular dependency** (you can't need to wear shoes before socks AND socks before shoes at the same time — that would be a cycle, and no valid order would exist).

## 3. When Should I Think of This Pattern?
- Problem mentions **prerequisites**, **dependencies**, or **order**
- Involves a **Directed Acyclic Graph (DAG)**
- Asks "can all tasks be completed" (cycle detection in directed graph)
- Asks for a **valid sequence/order** to do things
- Words like "course schedule", "build order", "compilation order", "task dependencies"

## 4. Mental Trigger
> "Whenever I see 'must be done before' or 'prerequisites' in a directed graph, I think of Topological Sort."

## 5. Visual Explanation (ASCII)
```
Socks → Shoes
Shirt → Jacket
Underwear → Pants → Shoes

Valid orders (multiple may exist):
Underwear → Socks → Shirt → Pants → Jacket → Shoes
Socks → Underwear → Pants → Shirt → Shoes → Jacket

In-degree of a node = number of arrows pointing INTO it.
Start with nodes that have in-degree 0 (no prerequisites).
```

## 6. Generic Java Pseudocode Template

**Kahn's Algorithm (BFS-based):**
```java
int[] inDegree = new int[n];
List<List<Integer>> graph = buildGraph();

for (List<Integer> neighbors : graph) {
    for (int neighbor : neighbors) inDegree[neighbor]++;
}

Queue<Integer> queue = new LinkedList<>();
for (int i = 0; i < n; i++) {
    if (inDegree[i] == 0) queue.offer(i); // no prerequisites, can start here
}

List<Integer> order = new ArrayList<>();
while (!queue.isEmpty()) {
    int node = queue.poll();
    order.add(node);

    for (int neighbor : graph.get(node)) {
        inDegree[neighbor]--;
        if (inDegree[neighbor] == 0) {
            queue.offer(neighbor);
        }
    }
}

// If order.size() != n, there's a cycle — no valid topological order exists
```

**DFS-based approach:**
```java
void dfs(
        int node,
        boolean[] visited,
        boolean[] inPath,
        Deque<Integer> stack) {

    visited[node] = true;
    inPath[node] = true;

    for (int neighbor : graph.get(node)) {

        // Neighbor is already in the current DFS path → cycle
        if (inPath[neighbor]) {
            throw new RuntimeException("Cycle detected!");
        }

        if (!visited[neighbor]) {
            dfs(neighbor, visited, inPath, stack);
        }
    }

    // Leaving this node's DFS path
    inPath[node] = false;

    // All descendants are processed
    stack.push(node);
}
// Final order = pop everything off the stack
graph[0] = [1, 2]
graph[1] = [3]
graph[2] = [3]
graph[3] = []
```
```text
        0
       / \
      ↓   ↓
      1   2
       \ /
        ↓
        3
```

## 7. Time Complexity
- **Time:** O(V + E) — every node and edge is processed exactly once (building in-degrees, then processing the queue).
- **Space:** O(V + E) for the adjacency list and in-degree array, plus O(V) for the queue/stack.
- **Why it's efficient:** By tracking in-degree counts, we know exactly which nodes are "ready" (no pending prerequisites) without repeatedly re-scanning the whole graph.

## 8. Common Mistakes
- Forgetting to check if `order.size() != n` at the end (missing cycle detection)
- Confusing **in-degree** (incoming edges) with **out-degree** (outgoing edges)
- In DFS-based approach, forgetting to track the **recursion stack** separately from "visited" (needed to detect back-edges/cycles)
- Not handling **multiple valid orderings** — topological sort is often not unique, and that's fine
- Building the graph in the **wrong direction** (edge should go from prerequisite → dependent task)

## 9. Real Interview Examples

| Problem | Difficulty | Why Topological Sort Works | Key Observation |
|---|---|---|---|
| Course Schedule | Medium | Detect if prerequisites form a cycle (impossible to complete all courses) | If topological order can't include all nodes, there's a cycle |
| Course Schedule II | Medium | Return the actual valid order of courses | Same as Course Schedule but output the order array |
| Alien Dictionary | Hard | Build edges from letter ordering rules in the sorted word list | Extract character order constraints, then topological sort |
| Minimum Height Trees | Medium | Trim leaf nodes layer by layer (reverse topological idea) | Not standard topo sort, but same "in-degree" trimming idea |
| Sequence Reconstruction | Medium | Check if a unique topological order matches a given sequence | Ensure only one valid path exists at each step |

## 10. LeetCode Problems
- Medium: [Course Schedule](https://leetcode.com/problems/course-schedule/)
- Medium: [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)
- Medium: [Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)
- Hard: [Alien Dictionary](https://leetcode.com/problems/alien-dictionary/) (Premium)
- Medium: [Sequence Reconstruction](https://leetcode.com/problems/sequence-reconstruction/) (Premium)

## 11. Interview Variations
Instead of "can all tasks be completed," interviewers may ask you to:
- Return the **actual valid order**, not just true/false
- Find **all possible valid orderings** (combine with backtracking)
- Detect the **specific edge** causing a cycle
- Build the graph from **implicit rules** (like character ordering in Alien Dictionary)
- Combine with **BFS layering** to find "levels" of tasks that can run in parallel

## 12. Pattern Recognition Checklist
```
Is there a directed graph with "must come before" relationships?
        ↓ yes
Do I need a valid order, or to check if a valid order is possible?
        ↓ yes
Is the graph potentially a DAG (Directed Acyclic Graph)?
        ↓ yes
→ Think Topological Sort
```

## 13. Similar Patterns (Don't Confuse These)
- **Topological Sort vs Union Find:** Topological Sort handles **directed** dependency graphs; Union Find handles **undirected** connectivity/grouping.
- **Topological Sort vs BFS:** Kahn's Algorithm topological sort IS a variant of BFS (using in-degree instead of a simple visited array), so they look similar but track different state.
- **Topological Sort vs DFS post-order:** The DFS-based approach uses post-order traversal (add to result after visiting all children) — easy to confuse with regular DFS if you forget the ordering detail.

## 14. Revision Summary (Cheat Sheet)
- **What it does:** Produces a valid linear ordering of nodes in a DAG respecting all "must come before" edges.
- **When to use:** Prerequisites, dependencies, build/compile order, scheduling with constraints.
- **Recognition clues:** "prerequisites", "dependencies", "order", "course schedule".
- **Java template:** Kahn's Algorithm (BFS + in-degree) or DFS post-order with a stack — see Section 6.
- **Time complexity:** O(V + E) time, O(V + E) space.
- **Common mistakes:** missing cycle check, confusing in-degree/out-degree, missing recursion-stack tracking in DFS.
- **Top interview questions:** Course Schedule, Course Schedule II, Alien Dictionary.
