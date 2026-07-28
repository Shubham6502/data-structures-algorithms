# Merge K Sorted Lists (Divide and Conquer)

**Problem:** LeetCode 23 - Merge k Sorted Lists  
**Difficulty:** Hard

---

## Problem Statement

You are given an array of `k` sorted linked lists.

Merge all the linked lists into one sorted linked list and return its head.

---

## Approach

Instead of merging all lists one by one, use the **Divide and Conquer** technique.

### Steps

1. Divide the array of linked lists into two halves.
2. Recursively solve the left half.
3. Recursively solve the right half.
4. Merge the two sorted linked lists.
5. Continue until only one merged list remains.

This approach works similarly to **Merge Sort**.

---

## Algorithm

### `mergeKLists()`

- If there are no lists, return `null`.
- Call `divideLists()` on the entire array.

---

### `divideLists()`

- If only one list exists, return it.
- Find the middle index.
- Divide the array into two halves.
- Recursively merge both halves.
- Merge the two sorted linked lists.

---

### `mergeTwoLists()`

- If one list is empty, return the other.
- Compare the current nodes.
- Attach the smaller node first.
- Recursively merge the remaining nodes.
- Return the head of the merged list.

---

## Dry Run

### Input

```
lists =

L1: 1 → 4 → 5
L2: 1 → 3 → 4
L3: 2 → 6
```

### Step 1

Divide

```
           [L1 L2 L3]
           /        \
      [L1 L2]      [L3]
      /    \
    [L1]  [L2]
```

---

### Step 2

Merge L1 and L2

```
1→4→5
1→3→4

↓

1→1→3→4→4→5
```

---

### Step 3

Merge with L3

```
1→1→3→4→4→5
2→6

↓

1→1→2→3→4→4→5→6
```

---

## Code

```java
class Solution {

    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {

        if (l1 == null) return l2;
        if (l2 == null) return l1;

        if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }

    public ListNode divideLists(ListNode[] lists, int left, int right) {

        if (left == right)
            return lists[left];

        int mid = left + (right - left) / 2;

        ListNode l1 = divideLists(lists, left, mid);
        ListNode l2 = divideLists(lists, mid + 1, right);

        return mergeTwoLists(l1, l2);
    }

    public ListNode mergeKLists(ListNode[] lists) {

        if (lists.length == 0)
            return null;

        return divideLists(lists, 0, lists.length - 1);
    }
}
```

---

# Time Complexity

Let

- `k` = Number of linked lists
- `N` = Total number of nodes in all linked lists

### Divide

```
log₂(k) levels
```

### Merge

At every level, all `N` nodes are processed once.

```
O(N)
```

### Overall

```
O(N log k)
```

---

# Space Complexity

### Recursive Merge

```
O(N)
```

(in the worst case due to recursive calls while merging)

### Divide and Conquer Recursion

```
O(log k)
```

Overall recursive stack is dominated by merging:

```
O(N)
```

---

# Why Divide and Conquer?

Suppose there are **8 linked lists**.

### Brute Force

```
((((L1+L2)+L3)+L4)+L5)+L6)+L7)+L8
```

Many nodes are merged repeatedly.

---

### Divide and Conquer

```
        8 Lists
       /      \
     4          4
    / \        / \
   2   2      2   2
  / \ / \    / \ / \
 L L L L    L L L L
```

Each merge happens on similarly sized lists, reducing unnecessary work.

This improves the complexity from:

```
O(N × k)
```

to

```
O(N log k)
```

---

# Key Points

- Uses **Divide and Conquer** (similar to Merge Sort).
- Recursively splits the array of linked lists.
- Merges two sorted linked lists at each step.
- Much faster than merging lists one by one.
- Time Complexity: **O(N log k)**
- Space Complexity: **O(N)** (recursive merge stack)

---
