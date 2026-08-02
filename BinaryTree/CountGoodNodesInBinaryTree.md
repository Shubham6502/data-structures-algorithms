# Count Good Nodes in Binary Tree

## Problem

Given the `root` of a binary tree, a node **X** is called **good** if there are **no nodes with a value greater than X** on the path from the root to X.

Return the total number of **good nodes** in the binary tree.

---

## Example

### Input

```text
        3
       / \
      1   4
     /   / \
    3   1   5
```

### Output

```text
4
```

### Explanation

Good nodes are:

- **3** (Root is always good)
- **3** (Path: `3 → 1 → 3`)
- **4** (Path: `3 → 4`)
- **5** (Path: `3 → 4 → 5`)

---

# Approach (DFS)

## Key Idea

While traversing the tree, maintain the **maximum value encountered so far** on the path from the root to the current node.

For every node:

- If `node.val >= maxSoFar`
  - It is a **good node**.
- Update the maximum value.
- Continue recursively for both left and right subtrees.

---

## Algorithm

1. Start DFS from the root.
2. Pass the maximum value seen so far (`maxSoFar`).
3. If the current node's value is greater than or equal to `maxSoFar`, count it.
4. Update

```java
maxSoFar = Math.max(maxSoFar, root.val);
```

5. Recursively process the left and right children.
6. Return the total count.

---

## Dry Run

### Input

```text
        3
       / \
      1   4
     /   / \
    3   1   5
```

---

### Step 1

Visit **3**

Path:

```text
3
```

Current Maximum = 3

```text
3 >= 3
```

✅ Good Node

Count = 1

---

### Step 2

Visit **1**

Path

```text
3 → 1
```

Maximum so far = 3

```text
1 >= 3
```

❌ Not Good

Count = 1

---

### Step 3

Visit **3**

Path

```text
3 → 1 → 3
```

Maximum so far = 3

```text
3 >= 3
```

✅ Good Node

Count = 2

---

### Step 4

Visit **4**

Path

```text
3 → 4
```

Maximum so far = 3

```text
4 >= 3
```

✅ Good Node

Count = 3

Update Maximum = 4

---

### Step 5

Visit **1**

Path

```text
3 → 4 → 1
```

Maximum so far = 4

```text
1 >= 4
```

❌ Not Good

Count = 3

---

### Step 6

Visit **5**

Path

```text
3 → 4 → 5
```

Maximum so far = 4

```text
5 >= 4
```

✅ Good Node

Count = 4

---

Final Answer

```text
4
```

---

# Java Solution (Clean Version)

```java
class Solution {

    public int countNodes(TreeNode root, int maxSoFar) {

        if (root == null)
            return 0;

        int count = 0;

        if (root.val >= maxSoFar)
            count = 1;

        maxSoFar = Math.max(maxSoFar, root.val);

        count += countNodes(root.left, maxSoFar);
        count += countNodes(root.right, maxSoFar);

        return count;
    }

    public int goodNodes(TreeNode root) {
        return countNodes(root, root.val);
    }
}
```

---

## Complexity Analysis

### Time Complexity

- Every node is visited exactly once.

**O(n)**

---

### Space Complexity

- Recursive call stack depends on the tree height.

Balanced Tree:

```text
O(log n)
```

Skewed Tree:

```text
O(n)
```

Overall:

**O(h)**

where **h** is the height of the tree.

---

# Why This Works

At every node, we always know the **largest value** encountered from the root to that node.

If

```java
root.val >= maxSoFar
```

then there is **no larger value** before it on that path, so the node is **good**.

After checking the current node, update the maximum:

```java
maxSoFar = Math.max(maxSoFar, root.val);
```

and continue exploring the children.

---

# Key Takeaways

- Traverse the tree using **DFS**.
- Carry the **maximum value seen so far** on the current path.
- A node is **good** if:

```java
root.val >= maxSoFar
```

- Update the maximum before visiting the children.
- No global variables are needed; each recursive call returns its own count.

---

## Complexity Summary

| Metric | Complexity |
|---------|------------|
| Time | **O(n)** |
| Space | **O(h)** |
| Traversal | **DFS (Preorder)** |

This is the **optimal solution** for the problem.
