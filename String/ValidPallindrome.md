# 🔤 Valid Palindrome

## 📌 Problem Statement

Given a string `s`, determine whether it is a **palindrome**, considering only **alphanumeric characters** and ignoring cases.

A palindrome reads the same forward and backward after removing non-alphanumeric characters.

---

## 🧾 Examples

### Example 1

**Input**

```
s = "A man, a plan, a canal: Panama"
```

**Output**

```
true
```

**Explanation**

After removing special characters and converting to lowercase:

```
amanaplanacanalpanama
```

which reads the same forwards and backwards.

---

### Example 2

**Input**

```
s = "race a car"
```

**Output**

```
false
```

---

## 🧠 Approach (Two Pointer Technique)

### Idea

We use two pointers:

* One starting from the **beginning**
* One starting from the **end**

Steps:

1. Convert the string to lowercase.
2. Ignore characters that are not letters or digits.
3. Compare valid characters from both ends.
4. If any mismatch occurs → not a palindrome.
5. Continue until pointers meet.

---

## ⚙️ Algorithm Steps

1. Initialize two pointers:

   * `i = 0`
   * `j = length - 1`
2. Convert string to lowercase.
3. While `i <= j`:

   * Skip non-alphanumeric characters.
   * Compare characters.
   * If mismatch → return false.
   * Move both pointers inward.
4. If loop completes → return true.

---

## 💻 Java Solution

```java
class Solution { 
    public boolean isPalindrome(String s) {
        int i = 0, j = s.length() - 1;
        s = s.toLowerCase();

        while (i <= j) {
            if (!Character.isLetterOrDigit(s.charAt(i))) {
                i++;
            } else if (!Character.isLetterOrDigit(s.charAt(j))) {
                j--;
            } else if (s.charAt(i) != s.charAt(j)) {
                return false;
            } else {
                i++;
                j--;
            }
        }
        return true;
    }
}
```

---

## ⏱ Complexity Analysis

| Complexity Type  | Value    |
| ---------------- | -------- |
| Time Complexity  | **O(n)** |
| Space Complexity | **O(1)** |

### Explanation

* Each character is visited at most once.
* No extra data structure is used.

---

## 🔍 Key Concepts Used

* Two Pointer Technique
* String Traversal
* Character Validation
* Case Normalization

---

## 🧩 Pattern Recognition

This problem is a classic example of:

✅ **Two Pointer Pattern**

Common related problems:

* Reverse String
* Valid Palindrome II
* Container With Most Water

---

## 📚 Learning Takeaway

> When comparing elements from both ends of a sequence, the two-pointer technique often provides an optimal O(n) solution.

---

## 🏷 Tags

`String` `Two Pointers` `Easy` `Interview Favorite`

---

👨‍💻 **Author:** Shubham Patil
📈 Daily DSA Practice Journey
