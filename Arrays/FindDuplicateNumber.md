🔍 Find Duplicate Number (Leetcode 287)

📌 Problem Statement

Given an array of integers nums containing n + 1 integers, where each integer is in the range [1, n] inclusive.  
There is only one repeated number, but it may appear more than once.

👉 Return the duplicate number without modifying the array and using constant extra space.

💡 Approach: Floyd’s Tortoise and Hare (Cycle Detection)  
This solution uses the cycle detection algorithm, similar to detecting a loop in a linked list.

🔁 Key Idea   
Treat the array as a linked list:  
Index → Node  
Value → Next pointer  
Since one number repeats, it creates a cycle.  

⚙️ Algorithm Steps  
Step 1:  
Detect Intersection Point  
Use two pointers:  
slow moves one step  
fast moves two steps  
They will eventually meet inside the cycle   

Step 2:  
Find Entrance of Cycle  
Reset fast to start  
Move both pointers one step at a time  
The point where they meet again is the duplicate number  

🧠 Code Implementation 
```Java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = nums[0];
        int fast = nums[0];

        // Step 1: Detect cycle
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);

        // Step 2: Find duplicate
        fast = nums[0];
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }

        return slow;
    }
}
```
⏱️ Complexity Analysis  
Type	Complexity  
Time	O(n)  
Space	O(1)  
