# Implement Trie (Prefix Tree)

## Problem

Implement a Trie (Prefix Tree) with the following operations:

* `insert(String word)` — Inserts a word into the Trie.
* `search(String word)` — Returns `true` if the word exists in the Trie.
* `startsWith(String prefix)` — Returns `true` if any word in the Trie starts with the given prefix.

---

## Approach

A Trie stores characters in a tree-like structure.

Each node contains:

* `child[]` — An array of 26 child nodes for lowercase English letters.
* `eow` — Stands for **End Of Word** and indicates whether a complete word ends at this node.

### Insert

1. Start from the root node.
2. For every character:

   * Calculate its index using `char - 'a'`.
   * If the child node does not exist, create it.
   * Move to the child node.
3. Mark the final node as the end of the word.

### Search

1. Start from the root.
2. Traverse each character.
3. If a required child does not exist, return `false`.
4. After traversing the complete word, check `eow`.
5. Return `true` only if the node represents the end of a complete word.

### Starts With

1. Start from the root.
2. Traverse every character of the prefix.
3. If any character is missing, return `false`.
4. If all characters exist, return `true`.

---

## Java Solution

```java
class Trie {

    class Node {
        Node child[] = new Node[26];
        boolean eow;

        Node() {
            for (int i = 0; i < 26; i++) {
                child[i] = null;
            }
            eow = false;
        }
    }

    Node root;

    public Trie() {
        root = new Node();
    }

    public void insert(String word) {
        Node curr = root;

        for (int i = 0; i < word.length(); i++) {
            int idx = word.charAt(i) - 'a';

            if (curr.child[idx] == null) {
                curr.child[idx] = new Node();
            }

            curr = curr.child[idx];
        }

        curr.eow = true;
    }

    public boolean search(String word) {
        Node curr = root;

        for (int i = 0; i < word.length(); i++) {
            int idx = word.charAt(i) - 'a';

            if (curr.child[idx] == null) {
                return false;
            }

            curr = curr.child[idx];
        }

        if (curr.eow == true) {
            return true;
        }

        return false;
    }

    public boolean startsWith(String prefix) {
        Node curr = root;

        for (int i = 0; i < prefix.length(); i++) {
            int idx = prefix.charAt(i) - 'a';

            if (curr.child[idx] == null) {
                return false;
            }

            curr = curr.child[idx];
        }

        return true;
    }
}
```

---

## Example

Suppose we insert:

```text
apple
app
```

The Trie will look conceptually like:

```text
root
 |
 a
 |
 p
 |
 p ── e ── l ── e
 |
 eow
```

After inserting `"apple"`:

```text
search("apple")     → true
search("app")       → false
startsWith("app")   → true
```

After inserting `"app"`:

```text
search("apple")     → true
search("app")       → true
startsWith("ap")    → true
startsWith("xyz")   → false
```

---

## Time Complexity

Let `L` be the length of the word or prefix.

| Operation      | Time Complexity |
| -------------- | --------------- |
| `insert()`     | `O(L)`          |
| `search()`     | `O(L)`          |
| `startsWith()` | `O(L)`          |

### Space Complexity

For each new character, a new Trie node may be created.

**Space:** `O(N × L × 26)` in the worst case for `N` words of maximum length `L`.

In practice, shared prefixes reduce the number of nodes significantly.

---

## Key Point

The most important concept is:

```java
curr.child[idx]
```

This moves from one character node to the next.

And:

```java
curr.eow = true;
```

marks that a **complete word ends at this node**.

Without `eow`, you could determine that a prefix exists, but you could not correctly distinguish between a complete word and merely a prefix.
