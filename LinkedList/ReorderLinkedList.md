# Reorder List

## Problem

Given the head of a singly linked list `L`:

```
L0 → L1 → L2 → ... → Ln-1 → Ln
```

Reorder it to:

```
L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...
```

The node values **cannot be modified**. Only the node links should be changed.

---

# Approach

The problem can be solved in **three steps**:

1. Find the middle of the linked list.
2. Reverse the second half.
3. Merge both halves alternately.

This approach modifies the list **in-place** using constant extra space.

---

# Step 1: Find the Middle

Use the **Slow and Fast Pointer** technique.

- `slow` moves one node at a time.
- `fast` moves two nodes at a time.

When `fast` reaches the end, `slow` points to the middle.

### Code

```java
public ListNode findMid(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;

    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    return slow;
}
```

---

# Step 2: Reverse the Second Half

Split the list into two parts.

Example:

```
1 → 2 → 3 → 4 → 5
```

After finding the middle:

```
First Half:
1 → 2 → 3

Second Half:
4 → 5
```

Disconnect both halves:

```java
ListNode curr = mid.next;
mid.next = null;
```

Reverse the second half using three pointers.

```java
ListNode prev = null;

while (curr != null) {
    ListNode next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
}
```

Now the second half becomes:

```
5 → 4
```

---

# Step 3: Merge the Two Halves

Now merge alternately.

```
Left : 1 → 2 → 3

Right: 5 → 4
```

Merge one node from the left and one from the right.

```
1 → 5 → 2 → 4 → 3
```

Code:

```java
ListNode left = head;
ListNode right = prev;

while (right != null) {

    ListNode leftNext = left.next;
    ListNode rightNext = right.next;

    left.next = right;
    right.next = leftNext;

    left = leftNext;
    right = rightNext;
}
```

---

# Dry Run

## Input

```
1 → 2 → 3 → 4 → 5
```

---

## Step 1: Find Middle

```
Slow = 3

1 → 2 → 3 → 4 → 5
```

---

## Step 2: Split

```
First Half

1 → 2 → 3

Second Half

4 → 5
```

---

## Step 3: Reverse Second Half

```
5 → 4
```

---

## Step 4: Merge

Iteration 1

```
Left  : 1 → 2 → 3

Right : 5 → 4

Result:

1 → 5 → 2 → 3
```

---

Iteration 2

```
Left  : 2 → 3

Right : 4

Result:

1 → 5 → 2 → 4 → 3
```

---

Final Answer

```
1 → 5 → 2 → 4 → 3
```

---

# Complete Code

```java
class Solution {

    public ListNode findMid(ListNode head) {

        ListNode slow = head;
        ListNode fast = head;

        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        return slow;
    }

    public void reorderList(ListNode head) {

        if (head == null || head.next == null)
            return;

        // Step 1: Find Middle
        ListNode mid = findMid(head);

        // Step 2: Reverse Second Half
        ListNode curr = mid.next;
        mid.next = null;

        ListNode prev = null;

        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }

        // Step 3: Merge Two Halves
        ListNode left = head;
        ListNode right = prev;

        while (right != null) {

            ListNode leftNext = left.next;
            ListNode rightNext = right.next;

            left.next = right;
            right.next = leftNext;

            left = leftNext;
            right = rightNext;
        }
    }
}
```

---

# Complexity Analysis

### Time Complexity

- Finding the middle: **O(n)**
- Reversing the second half: **O(n)**
- Merging both halves: **O(n)**

Overall:

```
O(n)
```

---

### Space Complexity

Only a few pointers are used.

```
O(1)
```

---

# Key Takeaways

- Uses the **Slow & Fast Pointer** technique to find the middle.
- Reverses the second half **in-place** using three pointers.
- Merges both halves alternately without using extra memory.
- The linked list is modified **in-place**, making the solution optimal.
- **Important:** Always disconnect the first half (`mid.next = null`) before reversing to avoid creating cycles.
