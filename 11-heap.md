# Pattern: Heap (Priority Queue)

---

## 1. Pattern Name
**Heap / Priority Queue**

---

## 2. Explain the Pattern in Very Simple English

Imagine a hospital emergency room. Patients don't get treated in the order they arrived (that would be a plain Queue). Instead, the **most critical** patient always gets treated **next**, regardless of when they walked in.

A **Heap** (used through Java's `PriorityQueue`) is exactly this: a structure that always gives you the **smallest** (Min-Heap) or **largest** (Max-Heap) item first, in O(log n) time, without needing to fully sort everything.

Think of it as a "priority list" — you can keep adding items in any order, but whenever you ask "give me the top priority item," it hands it to you instantly (well, in O(log n), which is close enough to instant).

---

## 3. When Should I Think of This Pattern?

- "Kth largest/smallest element"
- "Top K frequent elements"
- Need to repeatedly get the **min or max** from a changing collection
- **Merge K sorted lists/arrays**
- "Median" of a stream of numbers
- Scheduling problems (meeting rooms, task scheduling by priority)
- You'd otherwise need to **re-sort** the whole collection every time something changes (wasteful) — a heap avoids that

---

## 4. Mental Trigger

> "Whenever I need the top/bottom K elements, or I need repeated access to the min or max while the collection keeps changing, I first think of Heap / Priority Queue."

---

## 5. Visual Explanation (ASCII)

**Min-Heap (smallest always at top):**
```
          2
        /   \
       5     3
      / \   /
     8   9 7

Top of heap = smallest value = 2
Removing it "bubbles up" the next smallest to the top automatically.
```

**Using a heap to find K largest elements:**
```
Numbers: [3, 1, 5, 12, 2, 8]   K = 3

Keep a MIN-heap of size K:
add 3        -> [3]
add 1        -> [1,3]
add 5        -> [1,3,5]
add 12       -> heap full, 12 > min(1) -> pop 1, push 12 -> [3,5,12]
add 2        -> 2 < min(3) -> skip
add 8        -> 8 > min(3) -> pop 3, push 8 -> [5,8,12]

Final heap = the 3 largest elements: {5, 8, 12}
```

---

## 6. Generic Java Pseudocode Template

**Min-Heap (default in Java):**
```java
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.offer(x);
int smallest = minHeap.poll();   // removes and returns smallest
int peek = minHeap.peek();       // just looks at smallest
```

**Max-Heap (reverse comparator):**
```java
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
```

**Custom comparator (heap of objects/arrays):**
```java
PriorityQueue<int[]> heap = new PriorityQueue<>((a, b) -> a[0] - b[0]); // min-heap by a[0]
```

**"Top K" template using a fixed-size min-heap:**
```java
PriorityQueue<Integer> heap = new PriorityQueue<>(); // min-heap

for (int num : nums) {
    heap.offer(num);
    if (heap.size() > k) {
        heap.poll();   // remove the smallest, keeping only the K largest
    }
}
// heap now contains the K largest elements; heap.peek() = Kth largest
```

---

## 7. Time Complexity

| Aspect | Value | Why |
|---|---|---|
| offer (insert) | O(log n) | New element "bubbles up" to its correct position through at most log n levels |
| poll (remove top) | O(log n) | Removing the root requires "bubbling down" the replacement through at most log n levels |
| peek | O(1) | The top element is always directly accessible at the root |
| Building a "Top K" heap over n elements | O(n log k) | Each of n elements does an O(log k) insert/removal against a heap capped at size k |
| Why efficient | Full sorting is O(n log n) and gives you everything sorted, even if you only needed the top K. A size-K heap does only O(n log k) work — much cheaper when k << n |

---

## 8. Common Mistakes

- Forgetting Java's `PriorityQueue` is a **Min-Heap by default** — need `Collections.reverseOrder()` or a custom comparator for Max-Heap behavior
- Confusing "keep the K largest using a **min**-heap" — this feels backwards at first but is correct: the smallest of the "top K so far" sits at the top, ready to be evicted when a bigger number comes along
- Not capping the heap size in "Top K" problems — leads to O(n log n) instead of the optimal O(n log k)
- Using a heap when a **simple sort** would be just as easy and the input size is small (heap has more overhead for tiny inputs)
- Forgetting that `PriorityQueue` iteration order is **not sorted** — only `poll()` guarantees sorted removal order
- Off-by-one when deciding whether "Kth largest" means index `k-1` after sorting, or literally the `k`th pop from a max-heap

---

## 9. Real Interview Examples

1. **Kth Largest Element in an Array** (Medium) — Why it works: maintain a min-heap of size K; after processing all elements, the top of the heap is the Kth largest. Key observation: avoids full O(n log n) sort, achieving O(n log k).

2. **Top K Frequent Elements** (Medium) — Why it works: count frequencies with a HashMap, then use a heap to extract the K most frequent. Key observation: combines HashMap (Pattern 1) with Heap — a very common combo.

3. **Merge K Sorted Lists** (Hard) — Why it works: put the head of each list into a min-heap; repeatedly pop the smallest, advance that list, and push its next node. Key observation: heap efficiently manages "which of K candidates is currently smallest" without comparing all K every single time.

4. **Find Median from Data Stream** (Hard) — Why it works: maintain **two heaps** — a max-heap for the smaller half of numbers, a min-heap for the larger half — keeping them balanced in size so the median is always at the top of one (or the average of both tops). Key observation: the classic "two-heap" trick for streaming median.

5. **Task Scheduler** (Medium) — Why it works: a max-heap of task frequencies lets you always schedule the currently most frequent task first, minimizing idle time. Key observation: heap-based greedy scheduling.

---

## 10. LeetCode Problems

**Easy**
- [Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)
- [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)  done

**Medium**
- [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
- [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
- [Task Scheduler](https://leetcode.com/problems/task-scheduler/) done, also ck formula meth
- [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

**Hard**
- [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
- [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

---

## 11. Interview Variations

Instead of: "Find the Kth largest element"
They may ask:
- Find the Kth largest in a **continuously updating stream** (not a fixed array)
- Find **K closest points** to a given origin (custom distance comparator)
- Merge **K** sorted structures efficiently (lists, arrays, files)
- Maintain a **running median** of a data stream (two-heap trick)
- Schedule tasks/jobs by **priority** with additional constraints (cooldown periods, deadlines)
- Build a **max-heap from scratch** (heapify) instead of using the built-in `PriorityQueue`

---

## 12. Pattern Recognition Checklist

```
Do I repeatedly need the min or max from a changing collection?
        ↓ yes
Do I need only the "top K" or "bottom K", not a full sort?
        ↓ yes
Would repeated full sorting be wasteful?
        ↓ yes
Think Heap / Priority Queue.
```

---

## 13. Similar Patterns

- **Heap vs Sorting:** Sorting gives you everything in order at O(n log n) cost, all at once. A heap is better when you need **incremental** access to the min/max as data keeps arriving, or when you only need the top K (not everything sorted).
- **Heap vs BFS:** Both use a queue-like structure, but BFS's queue is FIFO (plain order), while a heap's "queue" is priority-based (smallest/largest first). When BFS needs to consider **weighted** edges (not equal cost), it becomes Dijkstra's algorithm — BFS + a min-heap.
- **Two-Heap Trick vs Single Heap:** A single heap only gives you one extreme (min or max) fast. When you need something in the **middle** (like a median), you need two heaps working together, split at the midpoint.

---

## 14. Revision Summary (Cheat Sheet)

| Aspect | Details |
|---|---|
| **What it does** | Always gives instant access to the min or max of a changing collection |
| **When to use** | Top K / Kth largest-smallest, merge K sorted structures, running median, priority-based scheduling |
| **Recognition clues** | "Kth largest/smallest", "top K", "merge K sorted", "median of stream", "priority" |
| **Java template** | `PriorityQueue<Integer> pq = new PriorityQueue<>(); pq.offer(); pq.poll(); pq.peek();` |
| **Time complexity** | O(log n) insert/remove, O(1) peek, O(n log k) for Top-K style problems |
| **Common mistakes** | Forgetting Java defaults to min-heap, not capping heap size for Top-K, confusing min-heap-for-largest logic |
| **Top interview Qs** | Kth Largest Element in an Array, Top K Frequent Elements, Merge K Sorted Lists, Find Median from Data Stream |
