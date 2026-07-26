# Linked List Cycle (LeetCode 141)

## Problem Statement

Given the `head` of a linked list, determine whether the linked list has a cycle.

A cycle exists if some node in the list can be reached again by continuously following the `next` pointer.

Return:

- `true` → if a cycle exists.
- `false` → otherwise.

---

## Approach (Floyd's Cycle Detection Algorithm)

This problem can be solved efficiently using **two pointers**:

- **Slow Pointer** → moves one step at a time.
- **Fast Pointer** → moves two steps at a time.

### Logic

1. Initialize both pointers at the head.
2. Move:
   - `slow = slow.next`
   - `fast = fast.next.next`
3. If the linked list contains a cycle, the fast pointer will eventually catch the slow pointer.
4. If the fast pointer reaches `null`, there is no cycle.

This algorithm is also known as the **Tortoise and Hare Algorithm**.

---

# Dry Run

## Example 1

```
1 → 2 → 3 → 4
      ↑     ↓
      ← ← ←
```

### Initial

```
slow = 1
fast = 1
```

### Iteration 1

```
slow = 2
fast = 3
```

### Iteration 2

```
slow = 3
fast = 2
```

### Iteration 3

```
slow = 4
fast = 4

slow == fast
```

**Return**

```
true
```

---

## Example 2

```
1 → 2 → 3 → 4 → null
```

### Initial

```
slow = 1
fast = 1
```

### Iteration 1

```
slow = 2
fast = 3
```

### Iteration 2

```
slow = 3
fast = null
```

The fast pointer reached `null`.

**Return**

```
false
```

---

# Why Does This Work?

Imagine runners on a circular track:

- Slow runner moves **1 step**.
- Fast runner moves **2 steps**.

If the track is circular (cycle), the fast runner will eventually lap and meet the slow runner.

If there is no circle, the fast runner simply reaches the end.

---

# Algorithm

1. Initialize:
   - `slow = head`
   - `fast = head`
2. While `fast` and `fast.next` are not `null`:
   - Move slow by one node.
   - Move fast by two nodes.
   - If `slow == fast`, return `true`.
3. If the loop ends, return `false`.

---

# Code

```java
public class Solution {
    public boolean hasCycle(ListNode head) {

        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {

            slow = slow.next;
            fast = fast.next.next;

            if (slow == fast) {
                return true;
            }
        }

        return false;
    }
}
```

---

# Complexity Analysis

### Time Complexity

```
O(N)
```

- Each pointer traverses the linked list at most once.

### Space Complexity

```
O(1)
```

- No extra data structures are used.

---

# Why Not Use a HashSet?

Another approach is to store every visited node in a `HashSet`.

- If a node is visited again → cycle exists.
- Otherwise continue.

### Complexity

| Approach | Time | Space |
|----------|------|-------|
| HashSet | O(N) | O(N) |
| Floyd's Algorithm | O(N) | O(1) ✅ |

Floyd's algorithm is preferred because it uses **constant extra space**.

---

# Key Points to Remember

- Use **two pointers**:
  - Slow → 1 step
  - Fast → 2 steps
- If `slow == fast`, a cycle exists.
- If `fast == null` or `fast.next == null`, there is no cycle.
- Floyd's Cycle Detection Algorithm is also called the **Tortoise and Hare Algorithm**.
- Time Complexity: **O(N)**
- Space Complexity: **O(1)**
