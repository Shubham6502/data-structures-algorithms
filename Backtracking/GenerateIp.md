# Generate IP Address (GFG)

## Problem

Given a string containing only digits, return all possible valid IP addresses that can be formed by inserting dots into the string.

A valid IP address:

* Contains exactly **4 parts**
* Each part must be between **0 and 255**
* **No leading zeros** allowed (except `0` itself)

---

## Approach (Backtracking)

1. Try forming segments of length **1 to 3 digits**.
2. Check if the segment is valid.
3. Add the segment to the current IP.
4. Recursively process the remaining string.
5. When **4 parts are formed and the string is fully used**, store the result.

---

## Java Implementation

```java
import java.util.*;

class Solution {
    
    public ArrayList<String> generateIp(String s) {
        ArrayList<String> res = new ArrayList<>();
        backtrack(s, 0, res, "", 0);
        return res;
    }

    public void backtrack(String s, int idx, ArrayList<String> res, String curr, int parts) {

        // If 4 parts are formed and string is fully used
        if (parts == 4 && idx == s.length()) {
            res.add(curr.substring(0, curr.length() - 1));
            return;
        }

        // Stop if parts exceed or string finished early
        if (parts == 4 || idx == s.length()) return;

        // Try segments of length 1 to 3
        for (int i = 1; i < 4 && idx + i <= s.length(); i++) {

            String sub = s.substring(idx, idx + i);

            // Check for leading zero or value >255
            if ((sub.startsWith("0") && sub.length() > 1) || Integer.parseInt(sub) > 255)
                continue;

            backtrack(s, idx + i, res, curr + sub + ".", parts + 1);
        }
    }
}
```

---

## Example

Input

```
s = "25525511135"
```

Output

```
["255.255.11.135", "255.255.111.35"]
```

---

## Time Complexity

```
O(3^4) ≈ Constant
```

Because we try **at most 3 choices for each of 4 parts**.

---

## Key Points

* Use **Backtracking**
* Maximum **4 segments**
* Segment value must be **0–255**
* Avoid **leading zeros**

---
