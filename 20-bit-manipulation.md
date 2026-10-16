# Pattern 12: Bit Manipulation

## 1. Pattern Name
**Bit Manipulation**

## 2. Explain the Pattern in Very Simple English
Every number in a computer is stored as a sequence of **0s and 1s** (bits). Bit Manipulation means directly working with these 0s and 1s using special operators — much faster than doing math the normal way, because computers process bits natively.

Think of it like flipping light switches: each bit is a switch that's either OFF (0) or ON (1), and we have special tools (AND, OR, XOR, shifts) to flip, check, or combine these switches very quickly.

## 3. When Should I Think of This Pattern?
- Problem asks about **finding a unique/missing number** among duplicates
- Involves checking if a number is a **power of 2**
- Asks to **count set bits** (1s in binary representation)
- Mentions **XOR** properties (pairs cancel out)
- Needs O(1) extra space where a HashSet would normally be used
- Words like "without extra space", "single number", "power of two", "binary representation"

## 4. Mental Trigger
> "Whenever I see 'find the one that doesn't repeat' or need O(1) space instead of a HashSet, I think of Bit Manipulation (especially XOR)."

## 5. Visual Explanation (ASCII)
```
XOR truth table:
0 ^ 0 = 0
1 ^ 1 = 0
0 ^ 1 = 1
1 ^ 0 = 1

Key property: a ^ a = 0, and a ^ 0 = a

Single Number example: [4, 1, 2, 1, 2]
4 ^ 1 ^ 2 ^ 1 ^ 2
= 4 ^ (1 ^ 1) ^ (2 ^ 2)   ← pairs cancel to 0
= 4 ^ 0 ^ 0
= 4   ← the unique number!

Check power of 2:  n & (n-1) == 0
   8 = 1000
   7 = 0111
   8 & 7 = 0000  → 8 is a power of 2
```

## 6. Generic Java Pseudocode Template

**XOR to find unique element:**
```java
int result = 0;
for (int num : nums) {
    result ^= num; // duplicates cancel out
}
return result;
```

**Check if a number is a power of two:**
```java
boolean isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}
```

**Count set bits (Brian Kernighan's algorithm):**
```java
int countSetBits(int n) {
    int count = 0;
    while (n != 0) {
        n = n & (n - 1); // removes the lowest set bit each time
        count++;
    }
    return count;
}
```

**Common bit tricks:**
```java
//position 3 2 1 0
// n = 10  means 1 0 1 0
//               3 2 1 0
// Check if bit i is set
boolean isSet = (n & (1 << i)) != 0;

// Set bit i
n = n | (1 << i);

// Clear bit i
n = n & ~(1 << i);

// Toggle bit i
n = n ^ (1 << i);
```

## 7. Time Complexity
- **Time:** O(n) for scanning through numbers (like XOR-ing an array), or O(log n)/O(number of bits) for operations on a single number (like counting set bits).
- **Space:** O(1) — bit operations work directly on integers without extra data structures.
- **Why it beats brute force:** A HashSet-based "find the unique number" solution uses O(n) space; XOR achieves the same O(n) time with O(1) space by exploiting the mathematical property that `a ^ a = 0`.

## 8. Common Mistakes
- Forgetting operator **precedence** (bitwise operators have lower precedence than comparisons in Java — always use parentheses)
- Confusing `&` (bitwise AND) with `&&` (logical AND), or `|` with `||`
- Off-by-one errors with shift amounts (`1 << i` vs `1 << (i-1)`)
- Not handling **negative numbers** correctly (two's complement representation can behave unexpectedly with shifts)
- Overusing bit tricks where a simpler, more readable solution would do (don't over-engineer if not needed for the constraints)

## 9. Real Interview Examples

| Problem | Difficulty | Why Bit Manipulation Works | Key Observation |
|---|---|---|---|
| Single Number | Easy | XOR-ing all numbers cancels out pairs, leaving the unique one | a ^ a = 0, a ^ 0 = a |
| Number of 1 Bits | Easy | Count set bits using Brian Kernighan's trick | n & (n-1) removes lowest set bit |
| Counting Bits | Easy | DP + bit trick: bits(n) = bits(n >> 1) + (n & 1) | Reuses previously computed bit counts |
| Power of Two | Easy | A power of two has exactly one set bit | n & (n-1) == 0 confirms only one bit is set |
| Single Number II (each number appears 3 times except one) | Medium | Track bits appearing count mod 3 using two variables | More advanced XOR-style bit counting |

## 10. LeetCode Problems
- Easy: [Single Number](https://leetcode.com/problems/single-number/)
- Easy: [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)
- Easy: [Counting Bits](https://leetcode.com/problems/counting-bits/)
- Easy: [Power of Two](https://leetcode.com/problems/power-of-two/)
- Medium: [Single Number II](https://leetcode.com/problems/single-number-ii/)
- Medium: [Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)
- Medium: [Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/)

## 11. Interview Variations
Instead of "find the single number," interviewers may ask you to:
- Handle when **every other number appears three times** instead of twice (Single Number II)
- Find **two unique numbers** among duplicates (Single Number III — split by a differing bit)
- Perform **addition without using + or -** (Sum of Two Integers, using bitwise AND/XOR/shift)
- Find the **maximum XOR pair** in an array (combine with Trie — binary Trie)
- Convert between **binary/decimal/hex representations** manually

## 12. Pattern Recognition Checklist
```
Does the problem mention finding a "single"/"unique" element among duplicates?
        ↓ yes
Is O(1) space required (no HashSet allowed)?
        ↓ yes
Does the problem talk about binary representation, powers of two, or set bits?
        ↓ yes
→ Think Bit Manipulation
```

## 13. Similar Patterns (Don't Confuse These)
- **Bit Manipulation vs HashSet approach:** HashSet is easier to write and understand but uses O(n) space; Bit Manipulation (especially XOR) achieves O(1) space for specific problems with the right mathematical property.
- **Bit Manipulation vs Math tricks:** Some problems solvable via math (sum formulas) can also be solved via bits — usually math is more intuitive unless the bit trick is significantly faster or the problem explicitly hints at binary representation.

## 14. Revision Summary (Cheat Sheet)
- **What it does:** Directly manipulates the binary representation of numbers using AND, OR, XOR, NOT, and shift operators.
- **When to use:** Find unique/missing numbers, power-of-two checks, counting set bits, O(1) space requirements.
- **Recognition clues:** "single number", "power of two", "without extra space", "binary representation".
- **Java template:** XOR accumulation, `n & (n-1)` tricks, bit set/clear/toggle — see Section 6.
- **Time complexity:** O(n) or O(number of bits), O(1) space.
- **Common mistakes:** operator precedence, confusing `&`/`&&`, negative number shift behavior.
- **Top interview questions:** Single Number, Number of 1 Bits, Counting Bits, Single Number II.
