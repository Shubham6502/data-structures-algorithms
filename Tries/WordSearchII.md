# Word Search II - Complete Notes

## Problem

Given:

* A `char[][] board`
* A `String[] words`

Find all words that can be formed in the board.

Rules:

* Move only in 4 directions:

  * Up
  * Down
  * Left
  * Right
* A cell cannot be used more than once in the same word.

---

# Example

```text
board =
[
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]

words = ["oath", "pea", "eat", "rain"]
```

Output:

```text
["oath", "eat"]
```

---

# Best Approach

Use:

```text
Trie + DFS + Backtracking
```

## Main Flow

```text
words[]
   ↓
Build Trie
   ↓
Start DFS from every cell in board
   ↓
Check Trie while moving on board
   ↓
Found word → Add to result
```

---

# Important Concept

## Wrong Approach

```text
Board → Build Trie ❌
```

## Correct Approach

```text
Words → Build Trie
Board → DFS using Trie
```

The Trie stores all the words.

DFS searches for those words on the board.

---

# Complete Java Syntax Used

## 1. Character Array

```java
char[][] board;
```

Example:

```java
char[][] board = {
    {'a', 'b', 'c'},
    {'d', 'e', 'f'}
};
```

Access an element:

```java
char ch = board[0][1];
```

Output:

```text
b
```

---

# 2. Array Length

For rows:

```java
board.length
```

For columns:

```java
board[0].length
```

Example:

```java
for (int i = 0; i < board.length; i++) {
    for (int j = 0; j < board[0].length; j++) {
        System.out.println(board[i][j]);
    }
}
```

---

# 3. String Array

```java
String[] words;
```

Example:

```java
String[] words = {
    "oath",
    "pea",
    "eat"
};
```

---

# 4. For-Each Loop

```java
for (String word : words) {
    System.out.println(word);
}
```

Equivalent normal loop:

```java
for (int i = 0; i < words.length; i++) {
    String word = words[i];
}
```

---

# 5. Character at Index

```java
word.charAt(i)
```

Example:

```java
String word = "apple";

char ch = word.charAt(0);
```

Result:

```text
a
```

---

# 6. Convert Character to Trie Index

Trie array size:

```java
Node[] child = new Node[26];
```

Indexes:

```text
a → 0
b → 1
c → 2
...
z → 25
```

Syntax:

```java
int idx = ch - 'a';
```

Example:

```java
char ch = 'c';

int idx = ch - 'a';
```

Result:

```text
2
```

Using a String:

```java
int idx = word.charAt(i) - 'a';
```

Using the board:

```java
int idx = board[i][j] - 'a';
```

---

# Important Mistake

## Wrong

```java
int idx = word.charAt(i);
```

Why wrong?

```text
'a' = 97
'b' = 98
'c' = 99
```

But:

```java
Node[] child = new Node[26];
```

Valid indexes are:

```text
0 to 25
```

## Correct

```java
int idx = word.charAt(i) - 'a';
```

---

# 7. Trie Node

```java
class Node {

    Node[] child = new Node[26];

    String word;
}
```

## Meaning

```java
Node[] child = new Node[26];
```

Stores references to child nodes.

Example:

```text
child[0] → 'a'
child[1] → 'b'
child[2] → 'c'
```

---

# 8. Root Node

```java
Node root = new Node();
```

The root is the starting point of the Trie.

Example:

```text
        root
       /    \
      a      c
```

---

# Step 1: Build Trie

We insert every word into the Trie.

```java
public void insert(String[] words) {

    for (String word : words) {

        Node curr = root;

        for (int i = 0; i < word.length(); i++) {

            int idx = word.charAt(i) - 'a';

            if (curr.child[idx] == null) {
                curr.child[idx] = new Node();
            }

            curr = curr.child[idx];
        }

        curr.word = word;
    }
}
```

---

# Insert Syntax Explained

## Start from Root

```java
Node curr = root;
```

Every word starts from the root.

---

# Important Mistake: Not Resetting curr

## Wrong

```java
Node curr = root;

for (String word : words) {

    // insert word

}
```

After inserting the first word, `curr` stays at the end of that word.

The next word will start from the wrong position.

## Correct

```java
for (String word : words) {

    Node curr = root;

    // insert word

}
```

Every word starts again from:

```text
root
```

---

# Create New Node

```java
if (curr.child[idx] == null) {
    curr.child[idx] = new Node();
}
```

Meaning:

```text
If this character does not exist
→ Create a new Trie node
```

---

# Move to Next Node

```java
curr = curr.child[idx];
```

Meaning:

```text
Current Node
    ↓
Next Character Node
```

---

# Store Complete Word

```java
curr.word = word;
```

Example:

```text
o → a → t → h

                ↑
             word = "oath"
```

---

# Why Store String Instead of boolean?

Another Trie implementation can use:

```java
boolean eow;
```

Example:

```java
boolean eow;
```

But for Word Search II:

```java
String word;
```

is convenient.

When a word is found:

```java
result.add(curr.word);
```

No need to build the word separately.

---

# Step 2: Start DFS from Every Board Cell

Syntax:

```java
for (int i = 0; i < board.length; i++) {

    for (int j = 0; j < board[0].length; j++) {

        dfs(board, i, j, root, result);
    }
}
```

Why?

Because any cell can be the starting point of a word.

---

# DFS Function Syntax

```java
public void dfs(
        char[][] board,
        int i,
        int j,
        Node curr,
        List<String> result
) {
}
```

Parameters:

```text
board  → Game board
i      → Current row
j      → Current column
curr   → Current Trie node
result → List of found words
```

---

# Step 3: Boundary Check

```java
if (
    i < 0 ||
    j < 0 ||
    i >= board.length ||
    j >= board[0].length
) {
    return;
}
```

This checks whether we moved outside the board.

---

# Boundary Conditions

## Top

```java
i < 0
```

## Left

```java
j < 0
```

## Bottom

```java
i >= board.length
```

## Right

```java
j >= board[0].length
```

---

# Important: Use OR `||`

Correct:

```java
if (
    i < 0 ||
    j < 0 ||
    i >= board.length ||
    j >= board[0].length
) {
    return;
}
```

If even one condition is true, stop DFS.

---

# Step 4: Check Visited Cell

```java
if (board[i][j] == '#') {
    return;
}
```

`'#'` means the cell is already used in the current path.

---

# Step 5: Convert Board Character to Index

```java
int idx = board[i][j] - 'a';
```

Example:

```java
board[i][j] = 'c';
```

Then:

```text
'c' - 'a' = 2
```

---

# Step 6: Trie Pruning

```java
if (curr.child[idx] == null) {
    return;
}
```

Meaning:

```text
Current board character
        ↓
Does this character exist in Trie?
        ↓
No
        ↓
Stop DFS
```

This is the main optimization.

---

# Step 7: Move Forward in Trie

```java
curr = curr.child[idx];
```

The board and Trie move together.

Example:

```text
Board Path: o → a → t → h

Trie:      o → a → t → h
```

---

# Step 8: Check if Word is Found

```java
if (curr.word != null) {
    result.add(curr.word);
    curr.word = null;
}
```

If `curr.word` is not null:

```text
A complete word is found
```

---

# Why Set Word to null?

```java
curr.word = null;
```

This prevents duplicates.

Example:

```text
Found "eat"
        ↓
Add to result
        ↓
Set word = null
```

If another DFS path reaches the same Trie node:

```text
word == null
```

So it will not be added again.

---

# Step 9: Mark Cell as Visited

Save original value:

```java
char temp = board[i][j];
```

Mark as visited:

```java
board[i][j] = '#';
```

---

# Step 10: Explore 4 Directions

## Down

```java
dfs(board, i + 1, j, curr, result);
```

## Up

```java
dfs(board, i - 1, j, curr, result);
```

## Right

```java
dfs(board, i, j + 1, curr, result);
```

## Left

```java
dfs(board, i, j - 1, curr, result);
```

---

# Direction Diagram

```text
         i - 1
           ↑
           |
j - 1 ← Current → j + 1
           |
           ↓
         i + 1
```

Complete:

```java
dfs(board, i + 1, j, curr, result);
dfs(board, i - 1, j, curr, result);
dfs(board, i, j + 1, curr, result);
dfs(board, i, j - 1, curr, result);
```

---

# Step 11: Backtracking

After DFS finishes:

```java
board[i][j] = temp;
```

Complete syntax:

```java
char temp = board[i][j];

board[i][j] = '#';

// DFS calls

board[i][j] = temp;
```

---

# Backtracking Flow

```text
Original:

a → b → c

Mark current cell:

# → b → c

Explore all possible paths

Restore:

a → b → c
```

---

# Complete Code

```java
import java.util.*;

class Solution {

    // Trie Node
    class Node {

        // 26 lowercase English letters
        Node[] child = new Node[26];

        // Stores complete word
        String word;
    }

    // Trie Root
    Node root = new Node();


    // Insert all words into Trie
    public void insert(String[] words) {

        for (String word : words) {

            // Every word starts from root
            Node curr = root;

            // Traverse every character
            for (int i = 0; i < word.length(); i++) {

                // Convert character into index
                int idx = word.charAt(i) - 'a';

                // Create node if it does not exist
                if (curr.child[idx] == null) {
                    curr.child[idx] = new Node();
                }

                // Move to next node
                curr = curr.child[idx];
            }

            // Store complete word
            curr.word = word;
        }
    }


    // DFS + Backtracking
    public void dfs(
            char[][] board,
            int i,
            int j,
            Node curr,
            List<String> result
    ) {

        // Check board boundaries
        if (
            i < 0 ||
            j < 0 ||
            i >= board.length ||
            j >= board[0].length
        ) {
            return;
        }


        // Check if cell is already visited
        if (board[i][j] == '#') {
            return;
        }


        // Convert current character into Trie index
        int idx = board[i][j] - 'a';


        // Stop if current character does not exist in Trie
        if (curr.child[idx] == null) {
            return;
        }


        // Move forward in Trie
        curr = curr.child[idx];


        // Check if complete word is found
        if (curr.word != null) {

            result.add(curr.word);

            // Prevent duplicate results
            curr.word = null;
        }


        // Save current character
        char temp = board[i][j];


        // Mark cell as visited
        board[i][j] = '#';


        // Explore Down
        dfs(board, i + 1, j, curr, result);

        // Explore Up
        dfs(board, i - 1, j, curr, result);

        // Explore Right
        dfs(board, i, j + 1, curr, result);

        // Explore Left
        dfs(board, i, j - 1, curr, result);


        // Restore the cell
        // Backtracking
        board[i][j] = temp;
    }


    // Main Function
    public List<String> findWords(
            char[][] board,
            String[] words
    ) {

        // Store found words
        List<String> result = new ArrayList<>();


        // Step 1: Build Trie using words
        insert(words);


        // Step 2: Start DFS from every board cell
        for (int i = 0; i < board.length; i++) {

            for (int j = 0; j < board[0].length; j++) {

                dfs(
                    board,
                    i,
                    j,
                    root,
                    result
                );
            }
        }


        // Return result
        return result;
    }
}
```

---

# Full Algorithm

```text
1. Create Trie root

2. Insert every word into Trie

3. Loop through every cell of the board

4. Start DFS from every cell

5. Check boundaries

6. Check if cell is already visited

7. Convert board character to Trie index

8. Check if character exists in Trie

9. If not exists:
      return

10. Move to next Trie node

11. If a complete word is found:
      add it to result

12. Mark current cell as visited

13. Explore:
      Down
      Up
      Right
      Left

14. Restore current cell

15. Return result
```

---

# Important Interview Formula

```text
Words
  ↓
Build Trie
  ↓
Board
  ↓
DFS
  ↓
Backtracking
  ↓
Result
```

---

# Most Common Mistakes

## Mistake 1: Wrong Character Index

Wrong:

```java
int idx = word.charAt(i);
```

Correct:

```java
int idx = word.charAt(i) - 'a';
```

---

## Mistake 2: Not Resetting curr

Wrong:

```java
Node curr = root;

for (String word : words) {
    // insert
}
```

Correct:

```java
for (String word : words) {

    Node curr = root;

    // insert
}
```

---

## Mistake 3: Forgetting Boundary Check

Always check:

```java
if (
    i < 0 ||
    j < 0 ||
    i >= board.length ||
    j >= board[0].length
) {
    return;
}
```

---

## Mistake 4: Forgetting Visited Check

```java
if (board[i][j] == '#') {
    return;
}
```

---

## Mistake 5: Forgetting Backtracking

Wrong:

```java
board[i][j] = '#';

// DFS
```

You must restore:

```java
board[i][j] = temp;
```

---

## Mistake 6: Exploring Only 2 Directions

Wrong:

```java
Down
Right
```

Correct:

```java
Down
Up
Right
Left
```

---

# Time Complexity

Let:

```text
M = Number of rows
N = Number of columns
L = Maximum word length
```

## Trie Construction

```text
O(total characters in all words)
```

## DFS

Worst case:

```text
O(M × N × 4^L)
```

In practice, the Trie improves performance by stopping invalid paths early.

---

# Final Concept

```text
Trie stores words.

DFS searches the board.

Backtracking allows different paths.

Trie prunes invalid paths.
```

## Easy Formula to Remember

```text
Trie
+
DFS
+
Backtracking
=
Word Search II
```
