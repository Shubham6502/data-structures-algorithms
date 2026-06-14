LeetCode 2130 - Maximum Twin Sum of a Linked List


Problem Statement  

Given the head of a linked list with an even number of nodes, find the maximum twin sum of the linked list.

A twin pair is defined as:

Node(i) + Node(n - 1 - i)

where n is the length of the linked list.

Example  
Input: 1 -> 2 -> 3 -> 4

Twin Sums:  
1 + 4 = 5
2 + 3 = 5

Output: 5  
Approach  
Step 1: Find the Middle of the Linked List

Use the slow and fast pointer technique:

Slow pointer moves one step at a time.  
Fast pointer moves two steps at a time.

When the fast pointer reaches the end, the slow pointer will be at the middle.

Step 2: Reverse the Second Half

Reverse the linked list starting from the middle node.

Example:

Original:  
1 -> 2 -> 3 -> 4

Second Half:  
3 -> 4

After Reverse:  
4 -> 3  
Step 3: Calculate Twin Sums

Traverse both halves simultaneously:

First Half : 1 -> 2  
Second Half: 4 -> 3

1 + 4 = 5  
2 + 3 = 5

Keep track of the maximum twin sum.

Algorithm
Find the middle node using slow and fast pointers.  
Reverse the second half of the linked list.  
Traverse both halves together.  
Calculate twin sums and update the maximum value.  
Return the maximum twin sum.  
Java Solution  
```
class Solution {
    public int pairSum(ListNode head) {
      // Find middle of linked list
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // Reverse second half
        ListNode prev = null;
        ListNode curr = slow;

        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }

        // Calculate maximum twin sum
        int maxSum = 0;
        ListNode first = head;
        ListNode second = prev;

        while (second != null) {
            maxSum = Math.max(maxSum, first.val + second.val);
            first = first.next;
            second = second.next;
        }

        return maxSum;
      }
    }  
```
Complexity Analysis  
Complexity	Value  
Time	O(n)  
Space	O(1)  
