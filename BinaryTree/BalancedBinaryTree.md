# 110. Balanced Binary Tree

## Problem

Given the `root` of a binary tree, determine if it is **height-balanced**.

A height-balanced binary tree is defined as:

> For every node in the tree, the height difference between its left and right subtrees is **not more than 1**.

Return `true` if the tree is balanced; otherwise, return `false`.

---

## Approach (Recursive Height Calculation)

For every node:

1. Calculate the height of its left subtree.
2. Calculate the height of its right subtree.
3. If the absolute difference between the two heights is greater than `1`, the tree is not balanced.
4. Recursively check whether the left and right subtrees are balanced.
5. Return `true` only if all nodes satisfy the balance condition.

---

## Algorithm

1. If the tree is empty, return `true`.
2. Compute the height of the left subtree.
3. Compute the height of the right subtree.
4. If `|leftHeight - rightHeight| > 1`, return `false`.
5. Recursively check the left and right subtrees.
6. Return the logical AND of both results.

---

## Java Code

```java
class Solution {

    public int height(TreeNode root) {
        if (root == null) return 0;

        int leftHt = height(root.left);
        int rightHt = height(root.right);

        return Math.max(leftHt, rightHt) + 1;
    }

    public boolean isBalanced(TreeNode root) {

        if (root == null) return true;

        int leftHt = height(root.left);
        int rightHt = height(root.right);

        if (Math.abs(leftHt - rightHt) > 1)
            return false;

        return isBalanced(root.left) && isBalanced(root.right);
    }
}
```

---

## Dry Run

### Input

```text
        3
       / \
      9   20
         /  \
        15   7
```

### Height Calculation

| Node | Left Height | Right Height | Difference | Balanced |
|------|------------:|-------------:|-----------:|:--------:|
| 9 | 0 | 0 | 0 | ✅ |
| 15 | 0 | 0 | 0 | ✅ |
| 7 | 0 | 0 | 0 | ✅ |
| 20 | 1 | 1 | 0 | ✅ |
| 3 | 1 | 2 | 1 | ✅ |

**Output**

```text
true
```

---

### Unbalanced Example

```text
      1
     /
    2
   /
  3
 /
4
```

At node `2`:

```text
Left Height = 2
Right Height = 0

Difference = 2
```

Since the difference is greater than `1`, the tree is **not balanced**.

**Output**

```text
false
```

---

## Complexity Analysis

### Time Complexity

- **O(n²)**

Reason:
- For every node, the `height()` function traverses its entire subtree.
- Since `height()` is called for every node, many subtree heights are recalculated.

### Space Complexity

- **O(h)**

Where `h` is the height of the tree due to the recursion stack.

- Balanced tree: `O(log n)`
- Skewed tree: `O(n)`

---

## Key Takeaways

- A binary tree is balanced if every node has a height difference of at most **1**.
- This solution is simple and easy to understand.
- Heights of subtrees are recalculated multiple times, making the time complexity **O(n²)**.
- The optimized interview solution computes height and balance together in a single DFS, achieving **O(n)** time complexity.
