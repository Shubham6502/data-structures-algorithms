# LeetCode 2095 - Delete the Middle Node of a Linked List

## Problem Statement

Given the `head` of a singly linked list, delete the middle node and return the head of the modified linked list.

The middle node of a linked list of size `n` is the `⌊n / 2⌋th` node using 0-based indexing.

### Example

```text
Input: 1 -> 3 -> 4 -> 7 -> 1 -> 2 -> 6

Output: 1 -> 3 -> 4 -> 1 -> 2 -> 6
```

The middle node is `7`, so it is removed from the list.

---

## Approach

### Using Slow and Fast Pointers

We use two pointers:

* `slow` moves one step at a time.
* `fast` moves two steps at a time.

When `fast` reaches the end of the linked list, `slow` will be positioned just before the middle node.

After locating the middle node, we simply skip it by updating:

```java
slow.next = slow.next.next;
```

This removes the middle node from the linked list.

---

## Algorithm

1. If the list contains only one node, return `null`.
2. Initialize `slow` and `fast` pointers at the head.
3. Move `fast` two steps ahead initially.
4. Traverse the list:

   * Move `slow` one step.
   * Move `fast` two steps.
5. When the loop ends, `slow.next` points to the middle node.
6. Delete the middle node by skipping it.
7. Return the head of the modified list.

---

## Java Solution

```java
class Solution {
    public ListNode deleteMiddle(ListNode head) {
        if(head.next == null) return null;

        ListNode slow = head;
        ListNode fast = head;

        fast = fast.next.next;

        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }

        slow.next = slow.next.next;

        return head;
    }
}
```

---



## Complexity Analysis

| Complexity       | Value |
| ---------------- | ----- |
| Time Complexity  | O(n)  |
| Space Complexity | O(1)  |


---

## Learning Outcome

This problem demonstrates how the slow and fast pointer technique can efficiently locate the middle of a linked list in a single traversal while maintaining O(1) extra space.
