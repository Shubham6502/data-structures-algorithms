# Maximum Depth of Binary Tree

## Problem

Given the `root` of a binary tree, return its **maximum depth**.

The **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

---

## Approach (Recursive DFS)

Use **Depth First Search (DFS)** with recursion.

For every node:

1. Find the maximum depth of the left subtree.
2. Find the maximum depth of the right subtree.
3. The depth of the current node is:
   ```
   max(leftDepth, rightDepth) + 1
   ```
4. Return the calculated depth.

---

## Algorithm

1. If the current node is `null`, return `0`.
2. Recursively calculate the depth of the left subtree.
3. Recursively calculate the depth of the right subtree.
4. Return the greater depth plus one for the current node.

---

## Java Code

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;

        int leftHt = maxDepth(root.left);
        int rightHt = maxDepth(root.right);

        int maxHt = Math.max(leftHt, rightHt) + 1;

        return maxHt;
    }
}
```

---

## Dry Run

### Input

```
        3
       / \
      9   20
         /  \
        15   7
```

### Recursive Calls

```
maxDepth(3)
│
├── maxDepth(9)
│     ├── 0
│     └── 0
│     → returns 1
│
└── maxDepth(20)
      ├── maxDepth(15)
      │      → returns 1
      │
      └── maxDepth(7)
             → returns 1
      → returns 2

maxDepth(3)
= max(1, 2) + 1
= 3
```

### Output

```
3
```

---

## Example

### Input

```
root = [3,9,20,null,null,15,7]
```

### Output

```
3
```

---

## Complexity Analysis

| Complexity | Value |
|------------|-------|
| Time | **O(n)** |
| Space | **O(h)** (Recursive call stack) |

Where:

- **n** = Number of nodes
- **h** = Height of the tree

### Best Case (Balanced Tree)

```
Space = O(log n)
```

### Worst Case (Skewed Tree)

```
Space = O(n)
```

---

## Why `+1`?

The `+1` counts the **current node**.

For every node:

```
Depth = max(left subtree depth, right subtree depth) + 1
```

Example:

```
      A
     / \
    B   C
```

```
Depth(A)
= max(Depth(B), Depth(C)) + 1
```

---

## Edge Cases

- Empty tree → `0`
- Single node → `1`
- Left-skewed tree
- Right-skewed tree
- Balanced tree

---

## Pattern

- Binary Tree
- Recursion
- Depth First Search (DFS)
- Divide and Conquer

---

## Key Points

- Visit every node exactly once.
- Compute left and right subtree depths recursively.
- Return the larger depth plus one.
- Base case is when the node is `null`.
- This is the most common recursive solution asked in coding interviews.

---

## Summary

- Traverse the tree using **DFS**.
- Calculate the height of both subtrees.
- Return the larger height plus one.
- Time Complexity: **O(n)**
- Space Complexity: **O(h)**
