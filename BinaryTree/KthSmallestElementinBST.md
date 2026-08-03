# Kth Smallest Element in a Binary Search Tree

## Problem

Given the `root` of a Binary Search Tree (BST) and an integer `k`, return the **kth smallest** value in the BST.

---

## Intuition

A Binary Search Tree has a special property:

- Left subtree contains smaller values.
- Root comes after all values in the left subtree.
- Right subtree contains larger values.

Therefore, an **Inorder Traversal (Left → Root → Right)** visits the nodes in **ascending order**.

So, if we perform an inorder traversal and count the visited nodes:
- The 1st visited node is the smallest.
- The 2nd visited node is the second smallest.
- ...
- The kth visited node is the answer.

---

## Approach

1. Perform an inorder traversal.
2. Maintain a counter `count` to track the number of visited nodes.
3. Whenever a node is visited:
   - Increment `count`.
   - If `count == k`, store the node's value in `res`.
4. Continue the traversal.
5. Return `res`.

---

## Algorithm

1. If the current node is `null`, return.
2. Traverse the left subtree.
3. Increment the visited node count.
4. If `count == k`, store the current node's value.
5. Traverse the right subtree.
6. Return the stored result.

---

## Code

```java
class Solution {

    int count = 0;
    int res = 0;

    public int kthSmallest(TreeNode root, int k) {

        if (root == null)
            return 0;

        kthSmallest(root.left, k);

        count++;

        if (count == k)
            res = root.val;

        kthSmallest(root.right, k);

        return res;
    }
}
```

---

## Dry Run

### Input

```
        5
       / \
      3   6
     / \
    2   4
   /
  1

k = 3
```

### Inorder Traversal

```
1 → 2 → 3 → 4 → 5 → 6
```

| Visited Node | Count | Result |
|--------------|------:|--------|
| 1 | 1 | - |
| 2 | 2 | - |
| 3 | 3 | ✅ Answer = 3 |
| 4 | 4 | - |
| 5 | 5 | - |
| 6 | 6 | - |

Output:

```
3
```

---

## Why Inorder Traversal Works

For every BST:

```
Left Subtree
      ↓
Current Node
      ↓
Right Subtree
```

This order always produces the elements in **sorted (ascending)** order.

Example:

```
      4
     / \
    2   6
   / \ / \
  1  3 5  7
```

Inorder:

```
1 2 3 4 5 6 7
```

Thus, the kth visited node is the kth smallest element.

---

## Time Complexity

- Every node is visited at most once.

**Time Complexity:** `O(n)`

> Although the answer may be found earlier, this implementation continues traversing the remaining nodes, so the worst-case time complexity is `O(n)`.

---

## Space Complexity

The recursive call stack depends on the tree height.

**Space Complexity:** `O(h)`

- Balanced BST: `O(log n)`
- Skewed BST: `O(n)`

---

## Optimization

This solution continues traversing even after finding the kth smallest element.

It can be optimized by stopping the recursion immediately once `count == k`, avoiding unnecessary traversal of the remaining nodes while keeping the same worst-case complexity.
