# Pattern: Intervals

---

## 1. Pattern Name
**Intervals (Merge / Overlap)**

---

## 2. Explain the Pattern in Very Simple English

Imagine you have a list of meetings on your calendar, each with a start time and an end time. Some meetings **overlap** (you're double-booked), and some don't.

If you want to know your **free time**, or you want to **combine overlapping meetings** into single blocks, or you want to know **how many meeting rooms** you'd need at once — you first **sort the meetings by start time**. Once sorted, you can walk through them one by one, and any overlap will always be between **neighboring** meetings in that sorted order — you never need to compare far-apart meetings.

That's the Intervals pattern: **sort by start time**, then scan through, merging or comparing only with the most recently processed interval.

---

## 3. When Should I Think of This Pattern?

- Problem gives you a list of **[start, end]** pairs
- "Merge overlapping intervals"
- "Insert a new interval"
- "Meeting rooms" — can a person attend all meetings, or how many rooms are needed
- "Find free time" / "find gaps"
- Any scheduling, booking, or range-overlap problem

---

## 4. Mental Trigger

> "Whenever I see a list of [start, end] pairs and need to merge, count overlaps, or find gaps, I first think: sort by start time, then scan once."

---

## 5. Visual Explanation (ASCII)

```
Intervals (unsorted): [8,10], [2,4], [6,9], [1,3]

After sorting by start:
[1,3]  [2,4]  [6,9]  [8,10]

Scan and merge:
[1,3] and [2,4] overlap (2 <= 3) -> merge into [1,4]
[1,4] and [6,9]: 6 > 4 -> no overlap, keep [6,9] as new interval
[6,9] and [8,10] overlap (8 <= 9) -> merge into [6,10]

Result: [1,4], [6,10]
```

```
Timeline view:
[1........4]
     [2..4]        <- absorbed into [1,4]
             [6..........10]
                 [8....10]  <- absorbed into [6,10]
```

---

## 6. Generic Java Pseudocode Template

**Merge overlapping intervals:**
```java
// Step 1: sort by start time
Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

List<int[]> merged = new ArrayList<>();

for (int[] interval : intervals) {
    if (merged.isEmpty() || merged.get(merged.size() - 1)[1] < interval[0]) {
        // no overlap with the last merged interval -> add as new
        merged.add(interval);
    } else {
        // overlap -> merge by extending the end time
        merged.get(merged.size() - 1)[1] =
            Math.max(merged.get(merged.size() - 1)[1], interval[1]);
    }
}
```

**Check if two intervals overlap:**
```java
boolean overlaps(int[] a, int[] b) {
    return a[0] < b[1] && b[0] < a[1];
}
```

**Meeting rooms II (minimum rooms needed) using a min-heap:**
```java
Arrays.sort(intervals, (a, b) -> a[0] - b[0]);   // sort by start time

PriorityQueue<Integer> endTimes = new PriorityQueue<>(); // min-heap of end times

for (int[] interval : intervals) {
    if (!endTimes.isEmpty() && endTimes.peek() <= interval[0]) {
        endTimes.poll();  // a room freed up before this meeting starts, reuse it
    }
    endTimes.offer(interval[1]);
}
// endTimes.size() = minimum number of rooms needed
```

---

## 7. Time Complexity

| Aspect | Value | Why |
|---|---|---|
| Sorting | O(n log n) | Standard comparison-based sort on start times |
| Scanning/merging | O(n) | One linear pass after sorting — each interval compared only to the last merged one |
| Overall | O(n log n) | Dominated by the sort step |
| Space | O(n) | For the result list (or O(log n) to O(n) for sort's internal usage, depending on implementation) |
| Why sorting first matters | Without sorting, you'd have to compare every interval against every other interval → O(n²). Sorting guarantees overlaps only ever occur between adjacent intervals in the sorted list |

---

## 8. Common Mistakes

- Forgetting to **sort first** — without sorting, overlaps can appear anywhere, not just adjacently, breaking the linear scan
- Using the wrong overlap condition — strict `<` vs `<=` matters depending on whether touching endpoints (e.g., `[1,3]` and `[3,5]`) count as overlapping in that specific problem
- Comparing the new interval against **all** merged intervals instead of just the **last** one (unnecessary — sorting guarantees you only need the last)
- Not updating the end time with `Math.max()` when merging (an interval fully contained inside another can have a smaller end time — always take the max)
- Forgetting to sort by **end time** instead of start time for certain problems (e.g., "maximum non-overlapping intervals" / activity selection uses end-time sorting)

---

## 9. Real Interview Examples

1. **Merge Intervals** (Medium) — Why it works: sort by start, then merge whenever the current interval's start is within the last merged interval's range. Key observation: the textbook example of this entire pattern.

2. **Insert Interval** (Medium) — Why it works: split into three phases — add all intervals ending before the new one starts, merge all overlapping intervals with the new one, then add all intervals starting after the new one ends. Key observation: a variation where the list is already sorted, so you don't need to re-sort.

3. **Meeting Rooms** (Easy) — Why it works: sort by start time; if any interval starts before the previous one ends, the person can't attend all meetings. Key observation: simplest version — just detects if *any* overlap exists.

4. **Meeting Rooms II** (Medium) — Why it works: use a min-heap of end times; if the earliest-ending meeting has already finished before the current meeting starts, reuse that room. Key observation: heap tracks "how many meetings are simultaneously active."

5. **Non-overlapping Intervals** (Medium) — Why it works: sort by **end time** (not start); greedily keep an interval if it doesn't overlap with the last kept one, otherwise remove it. Key observation: this is the classic "activity selection" greedy problem — sorting criterion changes based on the goal.

---

## 10. LeetCode Problems

**Easy**
- [Meeting Rooms](https://leetcode.com/problems/meeting-rooms/) (Premium — may need alternate source)
- [Summary Ranges](https://leetcode.com/problems/summary-ranges/)

**Medium**
- [Merge Intervals](https://leetcode.com/problems/merge-intervals/)
- [Insert Interval](https://leetcode.com/problems/insert-interval/)
- [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) (Premium — may need alternate source)
- [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)
- [Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

**Hard**
- [Employee Free Time](https://leetcode.com/problems/employee-free-time/) (Premium — may need alternate source)

---

## 11. Interview Variations

Instead of: "Merge overlapping intervals"
They may ask:
- **Insert** a new interval into an already-sorted list
- Count the **minimum number of resources** (rooms, arrows, etc.) needed for overlapping intervals
- Find the **maximum number of non-overlapping** intervals you can keep (activity selection — sort by end time)
- Find **free/gap** time between intervals (Employee Free Time)
- Determine if a **single new interval** conflicts with any existing ones (simpler existence check, no merging)
- Combine intervals from **multiple people/sources** before merging (Employee Free Time)

---

## 12. Pattern Recognition Checklist

```
Does the input consist of [start, end] pairs?
        ↓ yes
Do I need to merge, count overlaps, or find gaps?
        ↓ yes
Have I sorted by start (or end, depending on the goal) first?
        ↓ yes
Think Intervals — sort, then single linear scan.
```

---

## 13. Similar Patterns

- **Intervals vs Sorting:** Intervals problems almost always *start* with a sort — the pattern itself is really "sort + single-pass greedy scan," so recognizing when a problem is interval-shaped is really recognizing when a sort will simplify everything.
- **Intervals vs Heap:** For counting **simultaneous overlaps** (Meeting Rooms II), a heap tracks active end times efficiently — a natural pairing of the two patterns.
- **Intervals vs Two Pointers:** Some interval problems (like finding intersections between two separate lists of intervals) use two pointers — one per list — advancing whichever interval ends first. This is a different flavor from single-list merging.

---

## 14. Revision Summary (Cheat Sheet)

| Aspect | Details |
|---|---|
| **What it does** | Sorts intervals by start (or end) time, then scans once to merge, count, or detect overlaps |
| **When to use** | Merging ranges, scheduling/meeting-room problems, finding gaps, overlap detection |
| **Recognition clues** | "[start, end] pairs", "merge", "meeting rooms", "overlap", "free time" |
| **Java template** | `sort by start; for each interval: if overlaps last merged, extend it; else add new` |
| **Time complexity** | O(n log n) time (dominated by sort), O(n) space |
| **Common mistakes** | Forgetting to sort first, wrong overlap condition (`<` vs `<=`), not using `Math.max` when merging end times |
| **Top interview Qs** | Merge Intervals, Insert Interval, Meeting Rooms II, Non-overlapping Intervals |
