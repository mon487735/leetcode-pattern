# Pattern 9: Union Find (Disjoint Set Union)

## 1. Pattern Name
**Union Find** (also called Disjoint Set Union / DSU)

## 2. Explain the Pattern in Very Simple English
Imagine a classroom where students form friend groups. Whenever two students become friends, their entire friend groups **merge into one big group**. You often want to quickly answer: "Are these two students in the same friend group?"

Union Find is a data structure that efficiently supports two operations:
- **Union:** merge two groups together
- **Find:** figure out which group a person belongs to (its "representative" or "leader")

It's much faster than re-checking group membership by searching through a graph every time.

## 3. When Should I Think of This Pattern?
- Problem involves grouping items into **connected components**
- Asks to **detect a cycle** in an undirected graph
- Asks if two elements are **connected** or belong to the same group
- Involves **merging** sets/accounts/friend circles incrementally
- Problem hints at "connected components" without giving you an explicit adjacency list upfront (edges arrive one at a time)

## 4. Mental Trigger
> "Whenever I need to repeatedly merge groups and quickly check 'are these two in the same group?', I think of Union Find."

## 5. Visual Explanation (ASCII)
```
Initially, everyone is their own leader:
1   2   3   4   5
↑   ↑   ↑   ↑   ↑
1   2   3   4   5   (each is its own parent)

union(1, 2):
1 ← 2   (2's parent becomes 1, or vice versa)

union(2, 3):
1 ← 2 ← 3   (path compression later flattens this)

find(3) → follows parent chain up to leader (1)
```

## 6. Generic Java Pseudocode Template
```java
int[] parent;
int[] rank;

void initialize(int n) {
    parent = new int[n];
    rank = new int[n];
    for (int i = 0; i < n; i++) parent[i] = i; // everyone is their own leader initially
}

int find(int x) {
    if (parent[x] != x) {
        parent[x] = find(parent[x]); // path compression: flatten the tree
    }
    return parent[x];
}

void union(int x, int y) {
    int rootX = find(x);
    int rootY = find(y);

    if (rootX == rootY) return; // already in the same group

    // union by rank: attach smaller tree under bigger tree
    if (rank[rootX] < rank[rootY]) {
        parent[rootX] = rootY;
    } else if (rank[rootX] > rank[rootY]) {
        parent[rootY] = rootX;
    } else {
        parent[rootY] = rootX;
        rank[rootX]++;
    }
}

boolean isConnected(int x, int y) {
    return find(x) == find(y);
}
```

## 7. Time Complexity
- **Time:** Nearly O(1) per operation (technically O(α(n)), where α is the inverse Ackermann function — grows so slowly it's effectively constant for all practical input sizes) when using **both** path compression and union by rank.
- **Space:** O(n) for the parent and rank arrays.
- **Why it's efficient:** Without path compression, `find` could take O(n) per call in a skewed tree. Path compression flattens the tree structure every time `find` is called, and union by rank keeps trees balanced, so both together make operations nearly constant time.

## 8. Common Mistakes
- Forgetting **path compression** in `find`, leading to slow, deeply nested chains
- Forgetting **union by rank/size**, which can create a long skewed chain (worst case O(n) per find)
- Not initializing `parent[i] = i` for every element before starting
- Confusing "is connected" check — must compare `find(x) == find(y)`, not `x == y`
- Using Union Find on a problem that actually needs **directed** graph reasoning (Union Find is inherently for undirected relationships)

## 9. Real Interview Examples

| Problem | Difficulty | Why Union Find Works | Key Observation |
|---|---|---|---|
| Number of Provinces | Medium | Union connected cities, count remaining distinct groups | Count of unique roots = number of provinces |
| Redundant Connection | Medium | Detect the edge that creates a cycle in an undirected graph | If union(a,b) finds a==b already connected, that edge is redundant |
| Accounts Merge | Medium | Union accounts sharing at least one email | Group emails, then map back to account owners |
| Number of Islands (Union Find variant) | Medium | Alternative to BFS/DFS: union adjacent land cells | Shows Union Find can replace some BFS/DFS problems |
| Graph Valid Tree | Medium | A valid tree has exactly n-1 edges and no cycles — Union Find detects cycles | If any union finds already-connected nodes, it's not a tree |

## 10. LeetCode Problems
- Medium: [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)
- Medium: [Redundant Connection](https://leetcode.com/problems/redundant-connection/)
- Medium: [Accounts Merge](https://leetcode.com/problems/accounts-merge/)
- Medium: [Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/)
- Medium: [Satisfiability of Equality Equations](https://leetcode.com/problems/satisfiability-of-equality-equations/)
- Hard: [Number of Islands II](https://leetcode.com/problems/number-of-islands-ii/)

## 11. Interview Variations
Instead of "are these connected," interviewers may ask you to:
- **Count** the number of distinct connected components
- Detect the **specific edge** that causes a cycle
- Track **group size** along with connectivity (union by size)
- Handle **dynamic connectivity** where edges are added over time and you need to answer queries in between (Number of Islands II)
- Combine Union Find with **Kruskal's Algorithm** for Minimum Spanning Tree problems

## 12. Pattern Recognition Checklist
```
Do I need to group items and merge groups incrementally?
        ↓ yes
Do I need to quickly check "are these two in the same group"?
        ↓ yes
Is the relationship undirected (mutual connection)?
        ↓ yes
→ Think Union Find
```

## 13. Similar Patterns (Don't Confuse These)
- **Union Find vs BFS/DFS for connected components:** Both can find connected components; Union Find is better when edges arrive **incrementally** (online) or you need frequent "are these connected" queries; BFS/DFS is simpler when you have the full graph upfront and just need one pass.
- **Union Find vs Topological Sort:** Union Find handles **undirected** connectivity/cycle detection; Topological Sort handles **directed** graphs and ordering based on dependencies.

## 14. Revision Summary (Cheat Sheet)
- **What it does:** Efficiently merges groups and checks if two elements belong to the same group.
- **When to use:** Connected components, cycle detection in undirected graphs, incremental grouping/merging.
- **Recognition clues:** "connected", "merge", "same group", "provinces", "accounts merge".
- **Java template:** `find` with path compression + `union` with rank — see Section 6.
- **Time complexity:** Nearly O(1) per operation (O(α(n))), O(n) space.
- **Common mistakes:** missing path compression, missing union by rank, wrong initialization.
- **Top interview questions:** Number of Provinces, Redundant Connection, Accounts Merge, Graph Valid Tree.
