# 81. String to Integer (atoi)

### Problem Link

[LeetCode - String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/description/)

## Approach: Simulation of `atoi()` Function

### Idea

1. **Skip leading whitespaces** until the first non-space character.
2. **Handle signs (+/-)** if present. Ensure no invalid sign combinations appear.
3. **Extract digits** while converting them to an integer.
   - If the result overflows the **32-bit signed integer range**, return `INT_MAX` or `INT_MIN` accordingly.
4. Stop processing at the first non-numeric character or end of the string.

### Time Complexity

- **O(n)**: We traverse the input string once.

### Space Complexity

- **O(1)**: No extra space is used apart from a few variables.

### Code

```cpp
int myAtoi(string s) {
    int n = s.length();
    int i = 0;

    // Step 1: Skip leading whitespaces
    while (i < n && s[i] == ' ') {
        i++;
    }

    // Step 2: Handle optional sign
    bool isNeg = false;
    if (i < n && (s[i] == '+' || s[i] == '-')) {
        if (s[i] == '-') {
            isNeg = true;
        }
        i++;
    }

    // Step 3: Convert digits to integer, handling overflow
    long no = 0;
    while (i < n && s[i] >= '0' && s[i] <= '9') {
        int digit = s[i] - '0';

        // Check for overflow
        if (isNeg && (-1) * (no * 10 + digit) < INT_MIN) {
            return INT_MIN;
        }
        if (!isNeg && no * 10 + digit > INT_MAX) {
            return INT_MAX;
        }

        no = no * 10 + digit;
        i++;
    }

    // Step 4: Apply the sign if negative
    return isNeg ? (int)(-no) : (int)no;
}
```
