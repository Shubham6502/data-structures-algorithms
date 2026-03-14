# LeetCode: Get K-th Happy String of Length n

## Problem
A **happy string** is a string that:
- Consists only of characters **'a', 'b', 'c'**
- **No two adjacent characters are the same**

Given two integers:
- `n` → length of the string
- `k` → return the **k-th lexicographically smallest happy string**

If fewer than `k` happy strings exist, return **""**.

---

## Example

Input

n = 3
k = 9


Output

cab


Happy strings of length 3:


aba
abc
aca
acb
bab
bac
bca
bcb
cab
cac
cba
cbc


9th string → **cab**

---

# Approach: Backtracking

We generate all valid happy strings in **lexicographical order**.

Steps:
1. Try characters `'a'`, `'b'`, `'c'`
2. Skip if the character equals the previous character
3. Build the string recursively
4. Count valid strings
5. When count == k → store answer and stop recursion

---

# Key Idea

Avoid adjacent duplicates:


if(curr.length() > 0 && curr.charAt(curr.length()-1) == c)
continue;


---

# Java Implementation

```java
class Solution {

    int count = 0;
    String answer = "";

    public String getHappyString(int n, int k) {
        backtrack(n, k, "");
        return answer;
    }

    public void backtrack(int n, int k, String curr){

        if(curr.length() == n){
            count++;

            if(count == k){
                answer = curr;
            }
            return;
        }

        char[] chars = {'a','b','c'};

        for(char c : chars){

            if(curr.length() > 0 && curr.charAt(curr.length()-1) == c)
                continue;

            backtrack(n, k, curr + c);

            if(answer.length() > 0)
                return;
        }
    }
}
```
Complexity

Total happy strings:

3 × 2^(n-1)

Time Complexity

O(2^n)

Space Complexity

O(n)  (recursion stack)
Recursion Tree Example (n = 3)
```
       ""
     /  |  \
    a   b   c
   / \ / \ / \
 ab ac ba bc ca cb
```

Valid strings continue expanding until length n.

Important Interview Points

This is a Backtracking problem

Maintain lexicographical order

Avoid adjacent duplicate characters

Stop recursion once k-th string found

Edge Case

If k is greater than total possible happy strings:

return ""

Example

n = 1
k = 4

Only possible strings:

a b c

Output:

""
Key Takeaway

Pattern used:

Backtracking + Pruning

Important line:

if(answer.length() > 0) return;

Stops recursion once answer is found.
