# Design Add and Search Words Data Structure

## Problem

Implement a data structure that supports:

* `addWord(String word)` — Adds a word to the dictionary.
* `search(String word)` — Searches for a word in the dictionary.

The search supports the special character:

```text
.
```

The `.` character can represent **any single lowercase English letter**.

### Example

```text
addWord("bad")
addWord("dad")
addWord("mad")

search("pad") → false
search("bad") → true
search(".ad") → true
search("b..") → true
```

---

# Approach

We use a **Trie + DFS (Depth-First Search)**.

The Trie stores all words character by character.

Each `Node` contains:

* `child[]` — Array of 26 child nodes.
* `eow` — Indicates whether a complete word ends at this node.

The important part is handling the `.` character.

### Normal Character

If the current character is not `.`:

```java
int index = ch - 'a';
return dfs(word, curr.child[index], idx + 1);
```

We follow only the corresponding child.

### Dot Character `.`

If the current character is `.`:

```java
for (int i = 0; i < 26; i++) {
    if (curr.child[i] != null &&
        dfs(word, curr.child[i], idx + 1)) {
        return true;
    }
}
```

We try **all possible children** because `.` can represent any letter.

If any path produces a valid word, return `true`.

---

# Why DFS Is Required

For a normal Trie search, there is only one possible path.

For example:

```text
search("bad")
```

The path is:

```text
b → a → d
```

But for:

```text
search(".ad")
```

`.` can represent any character:

```text
bad
dad
mad
```

So we need to explore multiple paths.

DFS allows us to recursively explore every possible character when `.` is encountered.

---

# Java Solution

```java
class WordDictionary {

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

    public WordDictionary() {
        root = new Node();
    }

    public void addWord(String word) {
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

    public boolean dfs(String word, Node curr, int idx) {

        if (curr == null) {
            return false;
        }

        if (idx == word.length()) {
            return curr.eow;
        }

        char ch = word.charAt(idx);

        // If current character is '.',
        // try every possible child.
        if (ch == '.') {

            for (int i = 0; i < 26; i++) {

                if (curr.child[i] != null &&
                    dfs(word, curr.child[i], idx + 1)) {

                    return true;
                }
            }

            return false;

        } else {

            int index = ch - 'a';

            return dfs(
                word,
                curr.child[index],
                idx + 1
            );
        }
    }

    public boolean search(String word) {
        return dfs(word, root, 0);
    }
}
```

---

# Dry Run

Suppose we add:

```text
bad
dad
mad
```

The Trie contains:

```text
        root
       / | \
      b  d  m
      |  |  |
      a  a  a
      |  |  |
      d  d  d
```

Now search:

```text
.ad
```

### Step 1

Current character:

```text
.
```

`.` can represent:

```text
b
d
m
```

So DFS explores:

```text
b → a → d
```

This reaches `eow = true`.

Therefore:

```text
search(".ad") → true
```

---

# Another Example

Search:

```text
b..
```

The first character is fixed:

```text
b
```

The second character is `.`:

```text
b → any character
```

The third character is also `.`:

```text
b → any → any
```

If any complete word exists on one of these paths, return `true`.

---

# Base Case

The most important base condition is:

```java
if (idx == word.length()) {
    return curr.eow;
}
```

This means we have processed the complete search string.

Now we must check whether the current Trie node represents the **end of a complete word**.

This is important because:

```text
bad
```

exists, but:

```text
ba
```

should not return `true` unless `"ba"` was also added.

---

# Complexity Analysis

Let:

* `L` = length of the word.
* `N` = number of words stored in the Trie.
* `.` characters can cause branching.

## `addWord()`

Each character is processed once.

```text
Time: O(L)
```

## `search()`

For normal characters, we follow one path.

```text
Time: O(L)
```

However, `.` can branch into multiple Trie paths.

In the worst case:

```text
O(26^L)
```

because every `.` may explore up to 26 children.

In practice, the Trie structure significantly reduces unnecessary exploration.

## Space

Each Trie node contains an array of 26 references.

```text
Space: O(number of Trie nodes × 26)
```

The DFS recursion uses:

```text
O(L)
```

additional stack space.

---

# Key Difference: Trie vs WordDictionary

A normal Trie search:

```java
search("apple")
```

follows exactly one path.

`WordDictionary` adds wildcard support:

```java
search("a..le")
```

When `.` appears, DFS explores all possible children.

### Core Logic

```java
if (ch == '.') {
    for (int i = 0; i < 26; i++) {
        if (curr.child[i] != null &&
            dfs(word, curr.child[i], idx + 1)) {
            return true;
        }
    }

    return false;
}
```

**Remember:** `.` means **one arbitrary character**, not zero or multiple characters.
