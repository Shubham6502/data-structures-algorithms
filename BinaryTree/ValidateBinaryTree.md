# Validate Binary Search Tree

## Problem
Given the `root` of a binary tree, determine whether it is a **valid Binary Search Tree (BST)**.

A BST follows these rules:
- The left subtree of a node contains only nodes with values **less than** the node's value.
- The right subtree of a node contains only nodes with values **greater than** the node's value.
- Both the left and right subtrees must also be valid BSTs.

---

## Approach (Min-Max Boundary)

Instead of comparing a node only with its parent, maintain a **valid value range** for every node.

- Every node in the **left subtree** must be smaller than its parent.
- Every node in the **right subtree** must be greater than its parent.
- Pass the allowed **minimum** and **maximum** nodes while traversing recursively.

### Rules
- If `min != null`, current node value must be **greater** than `min.val`.
- If `max != null`, current node value must be **less** than `max.val`.
- Recursively validate:
  - Left subtree → update the maximum boundary.
  - Right subtree → update the minimum boundary.

---

## Algorithm

1. If the node is `null`, return `true`.
2. Check if the current node violates the minimum boundary.
3. Check if the current node violates the maximum boundary.
4. Recursively validate:
   - Left subtree with `(min, root)`
   - Right subtree with `(root, max)`
5. Return `true` only if both subtrees are valid.

---

## Code

```java
class Solution {

    public boolean valid(TreeNode root, TreeNode min, TreeNode max) {
        if (root == null) return true;

        if (min != null && min.val >= root.val) return false;
        if (max != null && max.val <= root.val) return false;

        return valid(root.left, min, root) &&
               valid(root.right, root, max);
    }

    public boolean isValidBST(TreeNode root) {
        return valid(root, null, null);
    }
}
```

---

## Dry Run

### Input

```
      5
     / \
    3   7
   / \   \
  2   4   8
```

### Traversal

- Node 5 → valid
- Node 3 → range (-∞, 5) ✔
- Node 2 → range (-∞, 3) ✔
- Node 4 → range (3, 5) ✔
- Node 7 → range (5, +∞) ✔
- Node 8 → range (7, +∞) ✔

Result:

```
true
```

---

### Invalid Example

```
      5
     / \
    3   7
       /
      4
```

For node `4`:
- It is in the right subtree of `5`.
- Allowed range is `(5, 7)`.
- `4 < 5` ❌

Result:

```
false
```

---

## Time Complexity

- Each node is visited exactly once.

**Time Complexity:** `O(n)`

---

## Space Complexity

- Recursive call stack equals the tree height.

**Space Complexity:** `O(h)`

- Best Case (Balanced Tree): `O(log n)`
- Worst Case (Skewed Tree): `O(n)`

---

## Key Idea

The mistake many solutions make is comparing a node only with its parent.

Instead, every node must satisfy the constraints imposed by **all of its ancestors**. Passing **minimum** and **maximum** boundaries during recursion ensures that every node stays within its valid range, making this the standard and correct solution for validating a Binary Search Tree.
