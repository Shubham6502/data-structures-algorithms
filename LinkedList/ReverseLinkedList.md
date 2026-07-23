# Reverse Linked List

## Problem
Given the `head` of a singly linked list, reverse the list and return the new head.

---

## Approach (Iterative - Three Pointer Technique)

We reverse the linked list by changing the direction of each node's `next` pointer.

We use **three pointers**:

- **prev** → Points to the previous node (initially `null`)
- **curr** → Points to the current node being processed
- **next** → Stores the next node before changing the link

### Steps

1. Initialize:
   - `prev = null`
   - `curr = head`

2. Traverse the list until `curr == null`.

3. For every node:
   - Store the next node.
   - Reverse the current node's pointer.
   - Move `prev` one step ahead.
   - Move `curr` one step ahead.

4. When traversal finishes, `prev` becomes the new head.

---

## Dry Run

### Input

```
1 → 2 → 3 → 4 → 5 → null
```

### Initial State

```
prev = null
curr = 1
```

---

### Iteration 1

```
next = 2

1 → null

prev = 1
curr = 2
```

Result:

```
null ← 1    2 → 3 → 4 → 5
```

---

### Iteration 2

```
next = 3

2 → 1

prev = 2
curr = 3
```

Result:

```
null ← 1 ← 2    3 → 4 → 5
```

---

### Iteration 3

```
next = 4

3 → 2

prev = 3
curr = 4
```

Result:

```
null ← 1 ← 2 ← 3    4 → 5
```

---

### Iteration 4

```
next = 5

4 → 3

prev = 4
curr = 5
```

Result:

```
null ← 1 ← 2 ← 3 ← 4    5
```

---

### Iteration 5

```
next = null

5 → 4

prev = 5
curr = null
```

Final Result:

```
5 → 4 → 3 → 2 → 1 → null
```

Return `prev`.

---

## Code

```java
class Solution {
    public ListNode reverseList(ListNode head) {

        ListNode prev = null;
        ListNode curr = head;

        while (curr != null) {

            ListNode next = curr.next;

            curr.next = prev;

            prev = curr;
            curr = next;
        }

        return prev;
    }
}
```

---

## Why It Works

At every iteration:

- Save the next node before breaking the link.
- Reverse the current node's pointer.
- Move both pointers forward.
- Continue until the entire list is reversed.

By the end:

- `curr` becomes `null`.
- `prev` points to the new head of the reversed list.

---

## Complexity Analysis

### Time Complexity

- We visit each node exactly once.

**Time:** `O(n)`

---

### Space Complexity

- Only three pointers are used regardless of list size.

**Space:** `O(1)`

---

## Key Takeaways

- Uses the **Three Pointer Technique** (`prev`, `curr`, `next`).
- Reverses the list **in-place**.
- No extra data structure is required.
- One of the most fundamental linked list algorithms asked in coding interviews.
