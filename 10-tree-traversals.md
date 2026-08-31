# Pattern: Tree Traversals

---

## 1. Pattern Name
**Tree Traversals (Pre-order, In-order, Post-order, Level-order)**

---

## 2. Explain the Pattern in Very Simple English

Imagine a family tree drawn on paper, and you need to read out everyone's name — but the **order** you read them in matters for what you're trying to figure out.

- **Pre-order**: Say the parent's name first, then visit the left branch, then the right branch. (Root → Left → Right)
- **In-order**: Visit the left branch first, then say the parent's name, then the right branch. (Left → Root → Right) — for Binary **Search** Trees, this naturally gives you names in **sorted order**!
- **Post-order**: Visit both branches first, and only say the parent's name **last**. (Left → Right → Root) — useful when children must be processed before the parent (like deleting a tree, or evaluating an expression tree).
- **Level-order**: Read everyone row by row, like reading a family tree top-to-bottom, left-to-right on each generation. (This is just BFS on a tree.)

Each traversal order exists because different problems need to know about the parent **before**, **between**, or **after** its children.

---

## 3. When Should I Think of This Pattern?

- Any problem explicitly involving a **binary tree** or **binary search tree (BST)**
- "Serialize/deserialize a tree"
- Need nodes in **sorted order** → In-order (works only for BSTs)
- Need to **copy/clone/build** a tree top-down → Pre-order
- Need to **delete/evaluate/process children before parent** → Post-order
- Need row-by-row / **level-by-level** processing → Level-order (BFS)
- "Reconstruct a tree from traversal arrays"

---

## 4. Mental Trigger

> "Whenever I see 'binary tree' and need a specific order (sorted, top-down, bottom-up, or level-by-level), I first think about which of the four traversals fits — root-first (pre), root-middle (in), root-last (post), or row-by-row (level)."

---

## 5. Visual Explanation (ASCII)

```
          1
        /   \
       2     3
      / \
     4   5

Pre-order  (Root, Left, Right): 1, 2, 4, 5, 3
In-order   (Left, Root, Right): 4, 2, 5, 1, 3
Post-order (Left, Right, Root): 4, 5, 2, 3, 1
Level-order (row by row):       1, 2, 3, 4, 5
```

Visualizing where "Root" (the node itself) gets processed relative to its children:
```
Pre-order:   [ROOT] [left subtree] [right subtree]
In-order:    [left subtree] [ROOT] [right subtree]
Post-order:  [left subtree] [right subtree] [ROOT]
```

---

## 6. Generic Java Pseudocode Template

**Pre-order (recursive):**
```java
void preOrder(TreeNode node) {
    if (node == null) return;
    // process node HERE (before children)
    preOrder(node.left);
    preOrder(node.right);
}
```

**In-order (recursive):**
```java
void inOrder(TreeNode node) {
    if (node == null) return;
    inOrder(node.left);
    // process node HERE (between children)
    inOrder(node.right);
}
```

**Post-order (recursive):**
```java
void postOrder(TreeNode node) {
    if (node == null) return;
    postOrder(node.left);
    postOrder(node.right);
    // process node HERE (after both children)
}
```

**Level-order (iterative, using Queue — this is BFS):**
```java
Queue<TreeNode> queue = new LinkedList<>();
queue.offer(root);

while (!queue.isEmpty()) {
    int size = queue.size();
    for (int i = 0; i < size; i++) {
        TreeNode node = queue.poll();
        // process node

        if (node.left != null) queue.offer(node.left);
        if (node.right != null) queue.offer(node.right);
    }
}
```

**Iterative in-order (using explicit Stack, if recursion isn't allowed):**
```java
Deque<TreeNode> stack = new ArrayDeque<>();
TreeNode current = root;

while (current != null || !stack.isEmpty()) {
    while (current != null) {
        stack.push(current);
        current = current.left;
    }
    current = stack.pop();
    // process current
    current = current.right;
}
```

---

## 7. Time Complexity

| Aspect | Value | Why |
|---|---|---|
| Time | O(n) | Every node is visited exactly once, regardless of traversal order |
| Space (recursive) | O(h) | h = height of the tree, due to the recursion call stack (O(log n) balanced, O(n) worst-case skewed tree) |
| Space (level-order) | O(w) | w = maximum width of the tree (number of nodes at the widest level) |
| Why it matters | Traversal order doesn't change time complexity — it changes **what information is available when**, which is the real reason to pick one over another |

---

## 8. Common Mistakes

- Mixing up **where** to "process" the node relative to the recursive calls (before / between / after)
- Forgetting the **base case** (`if node == null return`) — causes NullPointerException
- Using in-order traversal on a tree that **isn't** a BST and expecting sorted output (only works for BSTs)
- In iterative traversals, forgetting to push/pop in the right order, especially for in-order and post-order (post-order iteratively is trickier — often done via a modified pre-order + reversal)
- Confusing level-order (BFS, uses Queue) with the other three (DFS-style, use recursion/Stack)
- Not handling **null children** correctly when reconstructing a tree from traversal arrays

---

## 9. Real Interview Examples

1. **Binary Tree Inorder Traversal** (Easy) — Why it works: direct application of in-order recursion; for a BST, this also validates/produces sorted order. Key observation: also commonly asked as an *iterative* version using a stack.

2. **Validate Binary Search Tree** (Medium) — Why it works: an in-order traversal of a valid BST must produce strictly increasing values — check this while traversing. Key observation: reframes "is this a BST" into "is the in-order sequence sorted."

3. **Construct Binary Tree from Preorder and Inorder Traversal** (Medium) — Why it works: pre-order gives you the root first; in-order tells you which nodes are in the left vs right subtree relative to that root. Key observation: combining two traversal orders lets you rebuild the unique original tree.

4. **Binary Tree Level Order Traversal** (Medium) — Why it works: BFS with a queue, using the `queue.size()` trick to separate levels. Key observation: technically a traversal pattern but relies on BFS, not recursion.

5. **Serialize and Deserialize Binary Tree** (Hard) — Why it works: pre-order traversal (including `null` markers) captures a tree's exact shape and values, letting you rebuild it exactly during deserialization. Key observation: shows how traversal order directly determines whether reconstruction is even possible.

---

## 10. LeetCode Problems

**Easy**
- [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)
- [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)
- [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)
- [Same Tree](https://leetcode.com/problems/same-tree/)

**Medium**
- [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
- [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
- [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
- [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

**Hard**
- [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)
- [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

---

## 11. Interview Variations

Instead of: "Traverse the tree and print values"
They may ask:
- Return traversal results **iteratively** (no recursion allowed)
- **Reconstruct** the tree given two traversal orders
- **Validate** a property using traversal (BST validity, symmetry, balance)
- Traverse in **zig-zag** order (alternate left-to-right and right-to-left per level — a level-order variation)
- Find the **Kth smallest/largest** using in-order traversal on a BST
- **Serialize/deserialize** using a chosen traversal order plus null markers

---

## 12. Pattern Recognition Checklist

```
Is this a binary tree/BST problem?
        ↓ yes
Do I need sorted order? -> In-order (BST only)
Do I need to process parent before children (build/copy)? -> Pre-order
Do I need to process children before parent (delete/evaluate)? -> Post-order
Do I need row-by-row order? -> Level-order (BFS)
```

---

## 13. Similar Patterns

- **Pre/In/Post-order vs Level-order:** The first three are DFS-based (go deep first, use recursion/stack). Level-order is BFS-based (go wide first, use a queue). Pick based on whether you need "depth-first" info (ancestor/descendant relationships) or "row-by-row" info.
- **In-order Traversal vs Sorting:** For a BST specifically, in-order traversal *is* an O(n) way to get sorted output — no need for an O(n log n) sort.
- **Tree Traversal vs Graph DFS/BFS:** Tree traversal is just DFS/BFS applied to the special case of a tree (no cycles, one path between any two nodes) — so trees don't need a "visited" set like general graphs do.

---

## 14. Revision Summary (Cheat Sheet)

| Aspect | Details |
|---|---|
| **What it does** | Visits every node in a tree in a specific order (root-first, root-middle, root-last, or row-by-row) |
| **When to use** | Sorted BST output (in-order), copy/build (pre-order), delete/evaluate (post-order), row-by-row (level-order) |
| **Recognition clues** | "binary tree", "BST", "sorted order", "level by level", "reconstruct tree" |
| **Java template** | Recursive: `process before/between/after two recursive calls`. Level-order: Queue + `size()` trick |
| **Time complexity** | O(n) time; O(h) space (recursive), O(w) space (level-order) |
| **Common mistakes** | Wrong processing position, missing base case, using in-order on non-BST expecting sorted output |
| **Top interview Qs** | Validate Binary Search Tree, Construct Binary Tree from Traversals, Level Order Traversal, Serialize/Deserialize Binary Tree |
