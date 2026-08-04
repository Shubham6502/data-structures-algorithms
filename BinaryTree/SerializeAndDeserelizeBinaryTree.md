# 297. Serialize and Deserialize Binary Tree

## Problem Statement

Design an algorithm to **serialize** and **deserialize** a binary tree.

- **Serialize:** Convert a binary tree into a string.
- **Deserialize:** Convert the serialized string back into the original binary tree.

The deserialized tree should have the **same structure and node values** as the original tree.

---

## Approach

We use **Level Order Traversal (Breadth-First Search)** for both serialization and deserialization.

### Serialization

1. If the tree is empty, return `"null"`.
2. Create a queue and add the root.
3. Process nodes level by level:
   - If the current node is `null`, append `"null,"`.
   - Otherwise:
     - Append the node value.
     - Add its left and right children to the queue.
4. Continue until the queue becomes empty.

Storing `"null"` ensures that the tree structure is preserved.

---

### Deserialization

1. If the serialized string is `"null"`, return `null`.
2. Split the string using commas.
3. Create the root node using the first value.
4. Use a queue to reconstruct the tree level by level.
5. For every parent node:
   - Read the next value for the left child.
   - Read the following value for the right child.
   - If the value is not `"null"`, create the node and add it to the queue.
6. Return the reconstructed root.

---

## Code

```java
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null)
            return "null";

        Queue<TreeNode> q = new LinkedList<>();
        StringBuilder sb = new StringBuilder();

        q.add(root);

        while (!q.isEmpty()) {

            TreeNode curr = q.poll();

            if (curr == null) {
                sb.append("null,");
                continue;
            }

            sb.append(curr.val);
            sb.append(",");

            q.add(curr.left);
            q.add(curr.right);
        }

        return sb.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {

        if (data.equals("null"))
            return null;

        String[] arr = data.split(",");

        TreeNode root = new TreeNode(Integer.parseInt(arr[0]));

        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);

        int i = 1;

        while (!q.isEmpty() && i < arr.length) {

            TreeNode curr = q.poll();

            if (!arr[i].equals("null")) {
                curr.left = new TreeNode(Integer.parseInt(arr[i]));
                q.add(curr.left);
            }
            i++;

            if (i < arr.length && !arr[i].equals("null")) {
                curr.right = new TreeNode(Integer.parseInt(arr[i]));
                q.add(curr.right);
            }
            i++;
        }

        return root;
    }
}
```

---

## Dry Run

### Input Tree

```
        1
       / \
      2   3
         / \
        4   5
```

---

### Serialization

Initial Queue

```
[1]
```

Output

```
1,
```

Queue

```
[2,3]
```

Output

```
1,2,3,
```

Queue

```
[null,null,4,5]
```

Output

```
1,2,3,null,null,4,5,
```

Queue

```
[null,null,null,null]
```

Final Serialized String

```
1,2,3,null,null,4,5,null,null,null,null,
```

---

### Deserialization

Array

```
["1","2","3","null","null","4","5","null","null","null","null"]
```

Step 1

```
Create root = 1
```

Queue

```
[1]
```

---

Step 2

```
Parent = 1

Left = 2
Right = 3
```

Queue

```
[2,3]
```

---

Step 3

```
Parent = 2

Left = null
Right = null
```

Queue

```
[3]
```

---

Step 4

```
Parent = 3

Left = 4
Right = 5
```

Queue

```
[4,5]
```

---

Final Tree

```
        1
       / \
      2   3
         / \
        4   5
```

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

- Every node is visited exactly once during serialization.
- Every node is reconstructed exactly once during deserialization.

---

### Space Complexity

```
O(n)
```

- Queue stores nodes during BFS.
- Serialized string also stores every node and null marker.

---

## Why Store `"null"`?

Without storing null values, different trees may produce the same serialized string.

Example

### Tree 1

```
    1
   /
  2
```

### Tree 2

```
    1
     \
      2
```

Without `"null"` both become

```
1,2
```

With `"null"`

Tree 1

```
1,2,null,null,null
```

Tree 2

```
1,null,2,null,null
```

Now both trees can be reconstructed correctly.

---

## Key Observations

- Level Order Traversal preserves node ordering.
- `"null"` markers preserve the tree structure.
- Queue is used in both serialization and deserialization.
- Serialization and deserialization are inverse operations.
- Every node is processed exactly once.

---

## Common Mistakes

### ❌ Forgetting to store `"null"`

Without null markers, tree structure is lost.

---

### ❌ Accessing array without checking bounds

Always verify

```java
if (i < arr.length)
```

before accessing `arr[i]`.

---

### ❌ Not adding children during serialization

For every non-null node, always add

```java
q.add(curr.left);
q.add(curr.right);
```

Otherwise, the tree cannot be reconstructed correctly.

---

### ❌ Processing children of a null node

Correct implementation

```java
if (curr == null) {
    sb.append("null,");
    continue;
}
```

Using `continue` prevents accessing `curr.left` and `curr.right`.

---

## Pattern

- **Binary Tree**
- **Breadth-First Search (BFS)**
- **Queue**
- **Serialization & Deserialization**
- **Level Order Traversal**
