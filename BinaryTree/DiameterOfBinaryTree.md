# 543. Diameter of Binary Tree

## Problem
Given the `root` of a binary tree, return the **length of the diameter** of the tree.

The **diameter** of a binary tree is the length of the longest path between any two nodes. This path may or may not pass through the root.

> **Note:** The diameter is measured in **edges**, not nodes.

---

## Optimized Approach (Single DFS)

Instead of calculating the height of each subtree multiple times, compute the **height** and update the **diameter** during the same DFS traversal.

For every node:

- Recursively calculate the height of the left subtree.
- Recursively calculate the height of the right subtree.
- The diameter passing through the current node is:
  ```text
  leftHeight + rightHeight
  ```
- Update the global maximum diameter.
- Return the height of the current node:
  ```text
  max(leftHeight, rightHeight) + 1
  ```

Since each node is visited only once, this approach is optimal.

---

## Algorithm

1. Initialize a global variable `diameter = 0`.
2. Perform a DFS to calculate the height of each subtree.
3. For every node:
   - Find the left subtree height.
   - Find the right subtree height.
   - Update `diameter` with `leftHeight + rightHeight`.
   - Return the current node's height.
4. Return the final value of `diameter`.

---

## Java Code

```java
class Solution {

    private int diameter = 0;

    private int height(TreeNode root) {
        if (root == null) {
            return 0;
        }

        int leftHeight = height(root.left);
        int rightHeight = height(root.right);

        // Update the maximum diameter (measured in edges)
        diameter = Math.max(diameter, leftHeight + rightHeight);

        // Return the height of the current subtree
        return Math.max(leftHeight, rightHeight) + 1;
    }

    public int diameterOfBinaryTree(TreeNode root) {
        height(root);
        return diameter;
    }
}
```

---

## Dry Run

### Input

```text
        1
       / \
      2   3
     / \
    4   5
```

### Execution

| Node | Left Height | Right Height | Diameter Through Node | Global Diameter |
|------|------------:|-------------:|----------------------:|----------------:|
| 4 | 0 | 0 | 0 | 0 |
| 5 | 0 | 0 | 0 | 0 |
| 2 | 1 | 1 | 2 | 2 |
| 3 | 0 | 0 | 0 | 2 |
| 1 | 2 | 1 | 3 | 3 |

**Output**

```text
3
```

The longest path is:

```text
4 → 2 → 1 → 3
```

which contains **3 edges**.

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
  - Each node is visited exactly once.

- **Space Complexity:** `O(h)`
  - `h` is the height of the tree.
  - Worst case (skewed tree): `O(n)`
  - Balanced tree: `O(log n)`

---

## Key Takeaways

- Diameter is measured in **edges**, not nodes.
- Height and diameter can be computed in a **single DFS traversal**.
- Avoid recalculating subtree heights, reducing the complexity from **O(n²)** to **O(n)**.
- This is the optimal solution expected in coding interviews.
