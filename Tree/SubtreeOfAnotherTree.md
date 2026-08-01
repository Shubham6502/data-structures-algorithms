# Subtree of Another Tree (LeetCode 572)

## Problem Statement

Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values as `subRoot`, otherwise return `false`.

A subtree of a binary tree is a node in the tree along with all of its descendants.

---

## Approach

We solve this problem using **Recursion (Depth-First Search)**.

The idea is:

1. Traverse every node in the main tree.
2. Whenever a node's value matches `subRoot`'s root value, check whether the two trees are identical.
3. If they are identical, return `true`.
4. Otherwise, continue searching in the left and right subtrees.
5. If no matching subtree is found, return `false`.

---

## Algorithm

### Step 1: Compare Two Trees

Create a helper function `isSame()`.

- If both nodes are `null`, return `true`.
- If one node is `null`, return `false`.
- If node values differ, return `false`.
- Recursively compare:
  - Left subtree
  - Right subtree
- Return `true` only if both subtrees match.

### Step 2: Search for the Subtree

For every node in `root`:

- If the current subtree is identical to `subRoot`, return `true`.
- Otherwise, search the left subtree.
- If not found, search the right subtree.

---

## Java Solution

```java
class Solution {

    public boolean isSame(TreeNode root, TreeNode subRoot) {

        if (root == null && subRoot == null) return true;

        if (root == null || subRoot == null) return false;

        if (root.val != subRoot.val) return false;

        return isSame(root.left, subRoot.left)
                && isSame(root.right, subRoot.right);
    }

    public boolean isSubtree(TreeNode root, TreeNode subRoot) {

        if (subRoot == null) return true;

        if (root == null) return false;

        if (isSame(root, subRoot))
            return true;

        return isSubtree(root.left, subRoot)
                || isSubtree(root.right, subRoot);
    }
}
```

---

## Dry Run

### Example

```
Root Tree

        3
       / \
      4   5
     / \
    1   2

SubRoot

      4
     / \
    1   2
```

### Execution

```
Start at node 3

3 != 4
Search left subtree

Current node = 4

Compare both trees

4 == 4
1 == 1
2 == 2

All nodes match

Answer = true
```

---

### Example 2

```
Root Tree

        3
       / \
      4   5
     / \
    1   2
       /
      0

SubRoot

      4
     / \
    1   2
```

Comparison fails because the root tree has an extra node (`0`) under node `2`.

```
Answer = false
```

---

## Time Complexity

- **Worst Case:** **O(n × m)**

Where:

- `n` = Number of nodes in `root`
- `m` = Number of nodes in `subRoot`

In the worst case, we compare the entire `subRoot` at many nodes of `root`.

---

## Space Complexity

- **O(h)**

Where `h` is the height of the tree due to the recursion stack.

- Balanced Tree: **O(log n)**
- Skewed Tree: **O(n)**

---

## Key Points

- Traverse every node of the main tree.
- At each node, check whether both trees are identical.
- Use a helper function to compare two trees recursively.
- Return `true` immediately when a matching subtree is found.
- If no matching subtree exists, return `false`.

---

## Pattern Used

- Binary Tree
- Depth-First Search (DFS)
- Recursion
- Tree Comparison
