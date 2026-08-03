# Construct Binary Tree from Preorder and Inorder Traversal  (Leetcode:105)

## Problem

Given two integer arrays:

- `preorder` → Preorder traversal of a binary tree.
- `inorder` → Inorder traversal of the same binary tree.

Construct and return the original binary tree.

---

## Intuition

The solution is based on the properties of tree traversals.

### Preorder Traversal

```
Root → Left → Right
```

- The first element is always the **root** of the current subtree.

### Inorder Traversal

```
Left → Root → Right
```

- Elements on the left of the root belong to the left subtree.
- Elements on the right of the root belong to the right subtree.

By combining these two properties, we can recursively construct the tree.

---

## Approach

1. Maintain an index `preIdx` pointing to the current root in the preorder array.
2. Create a node using `preorder[preIdx]`.
3. Find this value in the inorder array.
4. Everything to the left of that index belongs to the left subtree.
5. Everything to the right belongs to the right subtree.
6. Recursively construct:
   - Left subtree
   - Right subtree
7. Return the constructed root.

---

## Algorithm

1. If `left > right`, return `null`.
2. Create a new node using `preorder[preIdx]`.
3. Find its index in the inorder array.
4. Increment `preIdx`.
5. Build the left subtree.
6. Build the right subtree.
7. Return the root.

---

## Code

```java
class Solution {

    int preIdx = 0;

    public int findIdx(int[] inorder, int left, int right, int val) {
        for (int i = left; i <= right; i++) {
            if (val == inorder[i])
                return i;
        }
        return -1;
    }

    public TreeNode build(int[] preorder, int[] inorder, int left, int right) {

        if (left > right)
            return null;

        TreeNode root = new TreeNode(preorder[preIdx]);

        int idx = findIdx(inorder, left, right, preorder[preIdx]);

        preIdx++;

        root.left = build(preorder, inorder, left, idx - 1);
        root.right = build(preorder, inorder, idx + 1, right);

        return root;
    }

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        return build(preorder, inorder, 0, inorder.length - 1);
    }
}
```

---

## Dry Run

### Input

```
preorder = [3,9,20,15,7]

inorder  = [9,3,15,20,7]
```

### Step 1

```
preIdx = 0

Root = 3
```

Find `3` in inorder.

```
[9 | 3 | 15 20 7]
```

Left subtree:

```
[9]
```

Right subtree:

```
[15 20 7]
```

---

### Step 2 (Left Subtree)

```
preIdx = 1

Root = 9
```

```
9
```

No children.

---

### Step 3 (Right Subtree)

```
preIdx = 2

Root = 20
```

Inorder:

```
15 | 20 | 7
```

Left subtree:

```
15
```

Right subtree:

```
7
```

---

### Step 4

```
preIdx = 3

Root = 15
```

Leaf node.

---

### Step 5

```
preIdx = 4

Root = 7
```

Leaf node.

---

### Final Tree

```
        3
       / \
      9   20
         /  \
        15   7
```

---

## Visualization

```
Preorder

3 → 9 → 20 → 15 → 7

        ↓

First element is always the root.

---------------------------------

Inorder

9 → 3 → 15 → 20 → 7

Left of root  -> Left Subtree
Right of root -> Right Subtree
```

---

## Time Complexity

### `findIdx()` takes `O(n)`.

Since it is called for every node,

**Time Complexity:** `O(n²)`

---

## Space Complexity

Recursive call stack depends on tree height.

**Space Complexity:** `O(h)`

- Balanced Tree: `O(log n)`
- Skewed Tree: `O(n)`

---

## Optimization

The current solution searches the inorder array linearly for every node.

This can be optimized by storing each inorder value and its index in a `HashMap`.

- Lookup becomes `O(1)`.
- Overall Time Complexity improves from **`O(n²)`** to **`O(n)`**.
- Space Complexity becomes **`O(n)`** due to the extra hash map.

This hash map approach is the standard optimal solution for this problem.
