# 76. Minimum Window Substring

## Problem Statement

Given two strings `s` and `t`, return the **minimum window substring** of `s` such that every character in `t`, including duplicates, is present in the window.

If no such substring exists, return an empty string `""`.

### Example

```text
Input:
s = "ADOBECODEBANC"
t = "ABC"

Output:
"BANC"
```

---

## Approach: Sliding Window + HashMap

We use two HashMaps:

* `map1` → stores the required frequency of characters from `t`.
* `map2` → stores the frequency of required characters in the current window.

We also use:

* `left` → left boundary of the window.
* `right` → right boundary of the window.
* `required` → number of unique characters required.
* `formed` → number of unique characters whose required frequency is currently satisfied.
* `minLen` → length of the smallest valid window found.
* `start` → starting index of the smallest valid window.

### Algorithm

1. Store the frequency of every character in `t` inside `map1`.
2. Expand the window by moving `right`.
3. If the current character is required, add it to `map2`.
4. When a character reaches its required frequency, increment `formed`.
5. When `formed == required`, the current window is valid.
6. Shrink the window from the left to find the smallest valid window.
7. Store the minimum window found.
8. Return the minimum substring.

---

## Java Solution

```java
class Solution {
    public String minWindow(String s, String t) {
        if (t.length() > s.length()) return "";

        HashMap<Character, Integer> map1 = new HashMap<>();
        HashMap<Character, Integer> map2 = new HashMap<>();

        for (int i = 0; i < t.length(); i++) {
            map1.put(
                t.charAt(i),
                map1.getOrDefault(t.charAt(i), 0) + 1
            );
        }

        int left = 0;
        int start = 0;
        int formed = 0;
        int required = map1.size();
        int minLen = Integer.MAX_VALUE;

        for (int right = 0; right < s.length(); right++) {
            char ch = s.charAt(right);

            if (map1.containsKey(ch)) {
                map2.put(ch, map2.getOrDefault(ch, 0) + 1);

                if (map2.get(ch).equals(map1.get(ch))) {
                    formed++;
                }
            }

            while (formed == required) {

                if (right - left + 1 < minLen) {
                    minLen = right - left + 1;
                    start = left;
                }

                char leftch = s.charAt(left);

                if (map1.containsKey(leftch)) {

                    if (map2.get(leftch).equals(map1.get(leftch))) {
                        formed--;
                    }

                    map2.put(leftch, map2.get(leftch) - 1);
                }

                left++;
            }
        }

        return minLen == Integer.MAX_VALUE
                ? ""
                : s.substring(start, start + minLen);
    }
}
```

---

## Dry Run

```text
s = "ADOBECODEBANC"
t = "ABC"
```

Required characters:

```text
A → 1
B → 1
C → 1
```

The right pointer expands until all required characters are present.

```text
ADOBEC
```

This is a valid window.

Then the left pointer moves forward to shrink the window.

The process continues, and eventually we find:

```text
BANC
```

This is the smallest valid window.

```text
Answer = "BANC"
```

---

## Complexity Analysis

### Time Complexity

```text
O(n + m)
```

Where:

* `n` is the length of `s`.
* `m` is the length of `t`.

Each character is processed at most twice by the sliding window.

### Space Complexity

```text
O(k)
```

Where `k` is the number of unique characters stored in the HashMaps.

---

## Key Learning

The important condition is:

```java
formed == required
```

We do **not** compare both HashMaps directly because the current window may contain extra occurrences of required characters.

For example:

```text
Required: A = 1
Window:   A = 2
```

The window is still valid even though the frequencies are not exactly equal.

When shrinking the window, `formed` is decreased only when removing a character makes its frequency fall below the required frequency.

---

## Pattern

**Sliding Window + Frequency HashMap**

This pattern is useful when we need to find the:

* Minimum valid substring
* Longest valid substring
* Substring satisfying character-frequency conditions
