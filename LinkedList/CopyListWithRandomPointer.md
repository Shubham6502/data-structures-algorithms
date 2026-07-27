# Copy List with Random Pointer

**Difficulty:** Medium  
**Topic:** Linked List, HashMap

---

## Problem Statement

Given the head of a linked list where each node contains:

- `val` → Integer value.
- `next` → Pointer to the next node.
- `random` → Pointer to any node in the list or `null`.

Create a **deep copy** of the linked list.

A deep copy means:

- Every node should be newly created.
- The copied list should have the same `next` and `random` connections.
- No copied node should reference any node from the original list.

---

## Approach (HashMap)

We solve this problem in **two passes**.

### Pass 1: Create Copy Nodes

Traverse the original list and create a new node for every original node.

Store the mapping:

```
Original Node -> Copied Node
```

Example:

```
Original:

A -> B -> C

Map:

A -> A'
B -> B'
C -> C'
```

At this stage, only the nodes are created.
The `next` and `random` pointers are still `null`.

---

### Pass 2: Connect Pointers

Traverse the original list again.

For every original node:

```
copy.next = map.get(original.next)
copy.random = map.get(original.random)
```

Since every original node already has a copied node in the map,
we can easily connect both pointers.

---

## Dry Run

### Original List

```
1 -----> 2 -----> 3
|         |         |
|         |         |
↓         ↓         ↓
3         1       null
```

### Step 1: Create Copies

```
Map

1 -> 1'
2 -> 2'
3 -> 3'
```

Copied nodes:

```
1'   2'   3'
```

(No connections yet.)

---

### Step 2: Connect Next

```
1' -> 2' -> 3'
```

---

### Step 3: Connect Random

```
1'.random = 3'
2'.random = 1'
3'.random = null
```

Final copied list:

```
1' -----> 2' -----> 3'
|           |          |
↓           ↓          ↓
3'          1'       null
```

---

## Algorithm

1. If `head == null`, return `null`.
2. Create a `HashMap<OriginalNode, CopiedNode>`.
3. Traverse the original list.
   - Create a copy of each node.
   - Store it in the map.
4. Traverse again.
   - Set `next`.
   - Set `random`.
5. Return the copied head.

---

## Code

```java
class Solution {
    public Node copyRandomList(Node head) {

        if (head == null) return null;

        HashMap<Node, Node> map = new HashMap<>();

        Node curr = head;

        // First Pass: Create copy nodes
        while (curr != null) {
            map.put(curr, new Node(curr.val));
            curr = curr.next;
        }

        curr = head;

        // Second Pass: Connect next and random pointers
        while (curr != null) {
            Node copy = map.get(curr);

            copy.next = map.get(curr.next);
            copy.random = map.get(curr.random);

            curr = curr.next;
        }

        return map.get(head);
    }
}
```

---

## Complexity Analysis

### Time Complexity

- First traversal: **O(n)**
- Second traversal: **O(n)**

**Total:** **O(n)**

---

### Space Complexity

- HashMap stores one copied node for every original node.

**Space:** **O(n)**

---

## Key Insight

Instead of trying to connect pointers while creating nodes, first create **all copied nodes**.

Once every original node has a corresponding copied node in the `HashMap`, connecting both `next` and `random` pointers becomes straightforward.

```
Original Node
      │
      ▼
HashMap
      │
      ▼
Copied Node
```

The `HashMap` acts as a bridge between the original list and the copied list.

---

## Interview Tips

- A **deep copy** creates completely new nodes.
- The copied list must not share any node with the original list.
- Two-pass traversal is the simplest and most readable approach.
- Remember that `map.get(null)` returns `null`, so no extra checks are needed when assigning `next` or `random`.

---

## Similar Problems

- Clone Graph
- Copy List with Random Pointer (Optimized O(1) Space)
- Clone Binary Tree with Random Pointer
