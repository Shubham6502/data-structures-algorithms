## 📌 Problem Statement
Given a string `s`, find the length of the longest substring without repeating characters.

---

## 🚀 Approach (Sliding Window)

This solution uses the **Sliding Window Technique** with a `HashSet` to efficiently track unique characters.

### Key Idea:
- Maintain a window using two pointers `i` (start) and `j` (end).
- Use a `HashSet` to store characters in the current window.
- Expand the window (`j++`) when characters are unique.
- Shrink the window (`i++`) when a duplicate is found.
- Update the maximum length during each step.

---

## 🧠 Algorithm Steps

1. Initialize:
   - `i = 0`, `j = 1`
   - Add first character to the set
2. Traverse the string using `j`
3. If duplicate found:
   - Remove characters from the left (`i`) until duplicate is removed
4. Add current character to the set
5. Update `maxLen`
6. Return `maxLen`

---

## 💻 Code Implementation

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int maxLen = 0;
        Set<Character> set = new HashSet<>();

        if (s.length() <= 1) return s.length();

        int i = 0, j = 1;
        set.add(s.charAt(0));

        while (j < s.length()) {

            if (set.contains(s.charAt(j))) {
                while (i < j && s.charAt(i) != s.charAt(j)) {
                    set.remove(s.charAt(i));
                    i++;
                }
                i++;
            }

            set.add(s.charAt(j));
            maxLen = Math.max(maxLen, j - i + 1);
            j++;
        }

        return maxLen;
    }
}
```
⏱️ Time Complexity
O(n) → Each character is visited at most twice.

💾 Space Complexity
O(min(n, charset)) → For storing characters in the set.
