# Add Two Numbers (LeetCode 2)

## Problem Statement

You are given two **non-empty linked lists** representing two non-negative integers.

- The digits are stored in **reverse order**.
- Each node contains a single digit.
- Add the two numbers and return the sum as a linked list.

### Example

**Input:**
```
l1 = [2,4,3]
l2 = [5,6,4]
```

**Output:**
```
[7,0,8]
```

**Explanation**

```
342
+465
----
807

Reverse Order Output:
[7,0,8]
```

---

# Approach

We simulate the same process as manual addition.

1. Create a **dummy node** to simplify list construction.
2. Maintain a **carry** variable.
3. Traverse both linked lists until:
   - `l1` becomes `null`
   - `l2` becomes `null`
   - `carry` becomes `0`
4. For every iteration:
   - Start with `sum = carry`
   - Add value from `l1` (if exists)
   - Add value from `l2` (if exists)
   - Store:
     - `carry = sum / 10`
     - Current digit = `sum % 10`
5. Create a new node with the current digit.
6. Return `dummy.next`.

---

# Dry Run

### Input

```
l1 = 2 → 4 → 3
l2 = 5 → 6 → 4
```

### Iteration 1

```
sum = 0 + 2 + 5 = 7

carry = 0
digit = 7

Result:
7
```

---

### Iteration 2

```
sum = 0 + 4 + 6 = 10

carry = 1
digit = 0

Result:
7 → 0
```

---

### Iteration 3

```
sum = 1 + 3 + 4 = 8

carry = 0
digit = 8

Result:
7 → 0 → 8
```

---

### Final Output

```
7 → 0 → 8
```

---

# Algorithm

1. Initialize:
   - `carry = 0`
   - `dummy` node
   - `curr = dummy`
2. While either list has nodes or carry exists:
   - `sum = carry`
   - Add `l1.val` if available.
   - Add `l2.val` if available.
   - Update carry.
   - Create node with `sum % 10`.
   - Move `curr`.
3. Return `dummy.next`.

---

# Code

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {

        int carry = 0;
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;

        while (l1 != null || l2 != null || carry != 0) {

            int sum = carry;

            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            }

            if (l2 != null) {
                sum += l2.val;
                l2 = l2.next;
            }

            carry = sum / 10;

            curr.next = new ListNode(sum % 10);
            curr = curr.next;
        }

        return dummy.next;
    }
}
```

---

# Why Dummy Node?

Without a dummy node:

- Need special handling for the first node.
- More edge cases.

With a dummy node:

- Every new node is added the same way.
- Cleaner and simpler code.

Example:

```
dummy → 7 → 0 → 8

Return:
dummy.next
```

---

# Complexity Analysis

### Time Complexity

```
O(max(N, M))
```

- Traverse each linked list once.

### Space Complexity

```
O(max(N, M))
```

- A new linked list is created to store the result.

---

# Key Points to Remember

- Digits are stored in **reverse order**.
- Always include `carry` in every addition.
- Continue the loop while:
  - `l1 != null`
  - OR `l2 != null`
  - OR `carry != 0`
- Use a **dummy node** to simplify linked list construction.
- The new digit is `sum % 10`.
- The carry is `sum / 10`.
```
