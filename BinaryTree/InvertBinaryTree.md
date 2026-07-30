# Invert Binary Tree

## Problem
Given the `root` of a binary tree, invert the tree and return its root.

---

## Approach (Recursive DFS)

For every node:

1. Recursively invert the left subtree.
2. Recursively invert the right subtree.
3. Swap the left and right children.
4. Return the current node.

---

## Algorithm

1. If the current node is `null`, return `null`.
2. Recursively invert the left subtree.
3. Recursively invert the right subtree.
4. Swap the left and right child using a temporary variable.
5. Return the root.

---

## Java Code

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {

        if (root == null) return null;

        invertTree(root.left);
        invertTree(root.right);

        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;

        return root;
    }
}
```

---

## Dry Run

### Input

```
    4
   / \
  2   7
 / \ / \
1  3 6  9
```

### Recursive Calls

- Invert subtree rooted at `2`
- Invert subtree rooted at `7`
- Swap children of `2`
- Swap children of `7`
- Swap children of `4`

### Output

```
    4
   / \
  7   2
 / \ / \
9  6 3  1
```

---

## Complexity Analysis

| Complexity | Value |
|------------|-------|
| Time | **O(n)** |
| Space | **O(h)** (Recursive stack, where `h` is the height of the tree) |

- **Balanced Tree:** `O(log n)`
- **Skewed Tree:** `O(n)`

---

## Key Points

- Uses **Depth First Search (DFS)**.
- Each node is visited exactly once.
- Swapping happens after recursively processing both subtrees.
- A temporary variable is required while swapping to avoid losing a subtree.

---

## Common Mistake ❌

Incorrect code:

```java
root.left = invertTree(root.right);
root.right = invertTree(root.left);
```

### Why is it wrong?

After assigning:

```java
root.left = invertTree(root.right);
```

the original left subtree is lost. The next line:

```java
root.right = invertTree(root.left);
```

uses the **modified** `root.left` instead of the original one, resulting in an incorrect tree.

Always use a temporary variable or swap after recursively inverting both subtrees.

---

## Pattern

- **Tree Traversal**
- **Recursion**
- **Depth First Search (DFS)**
