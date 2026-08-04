# 124. Binary Tree Maximum Path Sum

## Problem Statement

Given the `root` of a binary tree, return the **maximum path sum**.

A **path** is any sequence of nodes connected by edges. The path:
- Can start and end at any node.
- Must contain at least one node.
- Cannot visit the same node more than once.

---

## Approach

Use **Postorder Traversal (DFS)** because we need the maximum contribution from both left and right subtrees before processing the current node.

For every node:

1. Recursively calculate the maximum contribution from the left subtree.
2. Recursively calculate the maximum contribution from the right subtree.
3. Ignore negative contributions using:
   ```java
   Math.max(0, subtreeSum)
   ```
4. Compute the maximum path passing through the current node:
   ```text
   currentPathSum = left + right + root.val
   ```
5. Update the global maximum.
6. Return the maximum contribution to the parent:
   ```text
   root.val + max(left, right)
   ```
   Since a parent cannot continue through both branches, only one side is returned.

---

## Algorithm

1. If the node is `null`, return `0`.
2. Compute left subtree contribution.
3. Compute right subtree contribution.
4. Ignore negative sums.
5. Update the global maximum path.
6. Return the current node's maximum contribution.
7. Return the global maximum after traversal.

---

## Code

```java
class Solution {
    int maxSum = Integer.MIN_VALUE;

    public int getMaxSum(TreeNode root) {
        if (root == null)
            return 0;

        int leftSum = Math.max(0, getMaxSum(root.left));
        int rightSum = Math.max(0, getMaxSum(root.right));

        int currentSum = leftSum + rightSum + root.val;

        maxSum = Math.max(maxSum, currentSum);

        return root.val + Math.max(leftSum, rightSum);
    }

    public int maxPathSum(TreeNode root) {
        getMaxSum(root);
        return maxSum;
    }
}
```

---

## Dry Run

### Example

```
        -10
        /  \
       9    20
           /  \
          15   7
```

### Step 1

Node `15`

```
left = 0
right = 0

currentSum = 15

maxSum = 15

Return = 15
```

---

### Step 2

Node `7`

```
left = 0
right = 0

currentSum = 7

maxSum = 15

Return = 7
```

---

### Step 3

Node `20`

```
left = 15
right = 7

currentSum = 15 + 7 + 20 = 42

maxSum = 42

Return = 20 + max(15,7) = 35
```

---

### Step 4

Node `9`

```
left = 0
right = 0

currentSum = 9

maxSum = 42

Return = 9
```

---

### Step 5

Node `-10`

```
left = 9
right = 35

currentSum = 9 + 35 - 10 = 34

maxSum = 42

Return = -10 + max(9,35) = 25
```

---

## Final Answer

```
42
```

Path:

```
15 → 20 → 7
```

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

Each node is visited exactly once.

### Space Complexity

```
O(h)
```

- `h` = height of the tree
- Worst Case: `O(n)` (Skewed Tree)
- Best Case: `O(log n)` (Balanced Tree)

---

## Key Observations

- A path can start and end at any node.
- Ignore negative subtree sums using `Math.max(0, ...)`.
- The global answer considers both left and right children.
- The value returned to the parent includes only one branch.
- Postorder traversal is required because the current node depends on its children's results.

---

## Interview Tip

There are **two different calculations** at each node:

### 1. Update Global Maximum

```
left + right + root.val
```

This represents the best path passing through the current node.

### 2. Return to Parent

```
root.val + max(left, right)
```

Only one branch can continue upward, so return the larger contribution.

---

## Common Mistakes

❌ Including negative subtree sums.

```java
int left = getMaxSum(root.left);
```

✅ Correct:

```java
int left = Math.max(0, getMaxSum(root.left));
```

---

❌ Returning:

```java
left + right + root.val
```

A parent cannot continue through both branches.

✅ Correct:

```java
root.val + Math.max(left, right)
```

---

## Pattern

**Tree DP + Postorder Traversal + Global Variable**
