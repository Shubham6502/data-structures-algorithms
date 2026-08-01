# Lowest Common Ancestor of a Binary Search Tree (LeetCode 235)

## Problem Statement

Given the root of a **Binary Search Tree (BST)** and two nodes `p` and `q`, return their **Lowest Common Ancestor (LCA)**.

The **Lowest Common Ancestor** is the lowest node in the BST that has both `p` and `q` as descendants (a node can be a descendant of itself).

---

## Approach

Since the tree is a **Binary Search Tree**, we can use its property:

- All nodes in the **left subtree** have smaller values than the root.
- All nodes in the **right subtree** have greater values than the root.

Using this property:

1. If both `p` and `q` are greater than the current node, the LCA must be in the **right subtree**.
2. If both are smaller than the current node, the LCA must be in the **left subtree**.
3. Otherwise, the current node is the point where the paths split, so it is the **Lowest Common Ancestor**.

This allows us to avoid searching the entire tree.

---

## Algorithm

1. If `root` is `null`, return `null`.
2. If `root` is equal to `p` or `q`, return `root`.
3. If both `p` and `q` are greater than `root`, search the right subtree.
4. If both `p` and `q` are smaller than `root`, search the left subtree.
5. Otherwise, return the current `root` as the LCA.

---

## Java Solution

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {

        if (root == null || root == p || root == q)
            return root;

        if (p.val > root.val && q.val > root.val) {
            return lowestCommonAncestor(root.right, p, q);
        } else if (p.val < root.val && q.val < root.val) {
            return lowestCommonAncestor(root.left, p, q);
        } else {
            return root;
        }
    }
}
```

---

## Dry Run

### Example

```
        6
      /   \
     2     8
    / \   / \
   0   4 7   9
      / \
     3   5

p = 2
q = 8
```

### Execution

```
Current Node = 6

p < 6
q > 6

The nodes lie on different sides of 6.

Therefore,

Answer = 6
```

---

### Example 2

```
        6
      /   \
     2     8
    / \
   0   4
      / \
     3   5

p = 2
q = 4
```

### Execution

```
Current Node = 6

Both nodes are smaller than 6
Move to left subtree

Current Node = 2

Current node equals p

Answer = 2
```

---

## Time Complexity

- **O(h)**

Where `h` is the height of the BST.

- Balanced BST: **O(log n)**
- Skewed BST: **O(n)**

---

## Space Complexity

- **O(h)**

Due to the recursive call stack.

- Balanced BST: **O(log n)**
- Skewed BST: **O(n)**

---

## Why This Works

The BST property lets us determine the direction to move without exploring both subtrees.

- If both nodes are on the left, move left.
- If both nodes are on the right, move right.
- If they are on different sides (or one is the current node), the current node is the Lowest Common Ancestor.

This makes the solution much more efficient than a general binary tree approach.

---

## Key Points

- Works only for **Binary Search Trees (BSTs)**.
- Uses the BST ordering property to eliminate half of the tree at each step.
- The first node where the paths to `p` and `q` diverge is the Lowest Common Ancestor.
- If one node is an ancestor of the other, that node is the LCA.

---

## Pattern Used

- Binary Search Tree (BST)
- Recursion
- Divide and Conquer
- Tree Traversal
