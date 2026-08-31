# Pattern 11: Trie (Prefix Tree)

## 1. Pattern Name
**Trie** (pronounced "try", short for re**trie**val tree)

## 2. Explain the Pattern in Very Simple English
Imagine a family tree, but instead of people, each branch represents a **letter**. Words that share the same starting letters share the same path down the tree, and they only "split apart" where their spellings differ.

For example, "CAT" and "CAR" share the path C → A, then split into T and R. This lets us store and search words very efficiently, especially when we need to check **prefixes** (like autocomplete).

## 3. When Should I Think of This Pattern?
- Problem involves a **dictionary of words** and prefix-based search
- Asks for **autocomplete** or **typeahead** suggestions
- Asks to check if a **word or prefix exists** in a set of words
- Involves **word search in a grid** combined with a dictionary
- Words like "prefix", "starts with", "dictionary", "autocomplete"

## 4. Mental Trigger
> "Whenever I need to repeatedly check prefixes of words against a dictionary, I think of Trie."

## 5. Visual Explanation (ASCII)
```
Inserting: "cat", "car", "dog"

        (root)
        /    \
       c      d
       |      |
       a      o
      / \     |
     t   r    g
     *   *    *   (* marks end of a valid word)

Searching "ca" as prefix → walk c → a → found, prefix exists
Searching "cat" as word → walk c → a → t → check '*' marker → word exists
```

## 6. Generic Java Pseudocode Template
```java
class TrieNode {
    TrieNode[] children = new TrieNode[26]; // for lowercase a-z
    boolean isEndOfWord = false;
}

class Trie {
    TrieNode root = new TrieNode();

    void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            if (node.children[index] == null) {
                node.children[index] = new TrieNode();
            }
            node = node.children[index];
        }
        node.isEndOfWord = true;
    }

    boolean search(String word) {
        TrieNode node = findNode(word);
        return node != null && node.isEndOfWord;
    }

    boolean startsWith(String prefix) {
        return findNode(prefix) != null;
    }

    private TrieNode findNode(String str) {
        TrieNode node = root;
        for (char c : str.toCharArray()) {
            int index = c - 'a';
            if (node.children[index] == null) return null;
            node = node.children[index];
        }
        return node;
    }
}
```

## 7. Time Complexity
- **Time:** O(L) for insert/search/prefix-check, where L is the length of the word/prefix — independent of how many words are stored.
- **Space:** O(N × L × 26) in the worst case, where N is the number of words and L is average word length (each node can have up to 26 children).
- **Why it beats brute force:** Checking a prefix against a list of N words with string comparisons takes O(N × L). A Trie answers the same query in O(L), completely independent of how many words are stored.

## 8. Common Mistakes
- Forgetting to mark `isEndOfWord = true` at the end of insertion (breaks word-vs-prefix distinction)
- Not handling the **character set size** correctly (26 for lowercase letters, but may need more slots for uppercase, digits, or Unicode)
- Confusing "prefix exists" with "word exists" — a prefix can exist in the Trie without being a complete word
- Excessive memory usage for large datasets — using a `HashMap<Character, TrieNode>` per node can save memory over a fixed 26-array if sparse
- Not handling **duplicate insertions** properly (usually harmless, but be aware of word-count use cases)

## 9. Real Interview Examples

| Problem | Difficulty | Why Trie Works | Key Observation |
|---|---|---|---|
| Implement Trie (Prefix Tree) | Medium | Direct implementation of insert/search/startsWith | Foundation for all other Trie problems |
| Design Add and Search Words Data Structure | Medium | Trie + DFS to handle wildcard '.' character | Recursive search branches on wildcard |
| Word Search II | Hard | Build a Trie of the dictionary, then DFS the grid checking valid prefixes | Prunes DFS paths early using Trie prefix check |
| Longest Word in Dictionary | Medium | Insert all words, then DFS/BFS the Trie to find the longest buildable word | Every prefix along the way must also be a valid word |
| Replace Words | Medium | Insert all "roots" into a Trie, find shortest root prefix for each word | First matching prefix along Trie path wins |

## 10. LeetCode Problems
- Medium: [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)
- Medium: [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)
- Medium: [Replace Words](https://leetcode.com/problems/replace-words/)
- Medium: [Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)
- Hard: [Word Search II](https://leetcode.com/problems/word-search-ii/)

## 11. Interview Variations
Instead of "does this word exist," interviewers may ask you to:
- Support **wildcard search** (`.` matches any character)
- Combine Trie with **DFS on a grid** (Word Search II)
- Return **all words with a given prefix** (autocomplete)
- Count **how many words** start with a given prefix (add a counter field to TrieNode)
- Use a Trie for **bit manipulation problems** (Maximum XOR of Two Numbers — binary Trie)

## 12. Pattern Recognition Checklist
```
Does the problem involve a set/dictionary of words?
        ↓ yes
Do I need to repeatedly check prefixes or do prefix-based search?
        ↓ yes
Would a HashSet be too slow or not support prefix queries?
        ↓ yes
→ Think Trie
```

## 13. Similar Patterns (Don't Confuse These)
- **Trie vs HashMap/HashSet:** A HashSet can check if an exact word exists in O(1) average time, but CANNOT efficiently answer "does any word start with this prefix?" — that's where Trie shines.
- **Trie vs Backtracking:** Trie is a data structure; Backtracking is often used ALONGSIDE Trie (e.g., Word Search II) to explore paths while checking valid prefixes via the Trie.

## 14. Revision Summary (Cheat Sheet)
- **What it does:** Stores words character-by-character in a tree so shared prefixes are stored only once, enabling fast prefix search.
- **When to use:** Dictionary/prefix problems, autocomplete, word search with a dictionary.
- **Recognition clues:** "prefix", "starts with", "dictionary", "autocomplete".
- **Java template:** TrieNode with children array + isEndOfWord flag — see Section 6.
- **Time complexity:** O(L) per operation (L = word/prefix length), O(N×L×26) space.
- **Common mistakes:** forgetting end-of-word marker, confusing prefix vs word existence.
- **Top interview questions:** Implement Trie, Word Search II, Replace Words.
