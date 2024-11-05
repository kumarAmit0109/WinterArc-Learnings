# Rotate String

**Problem Link**: [Rotate String](https://leetcode.com/problems/rotate-string)

## Approach 1: Brute Force with Checking Indices

### Approach

1. **Length Check**:

   - First, check if the lengths of the two strings `s` and `goal` are equal. If not, return `false`.

2. **Find Matching Indices**:

   - Iterate through string `s` and store indices where the first character of `goal` matches any character in `s`.

3. **Check for Rotation**:

   - For each matching index, check if starting from that index in `s` matches the entire string `goal` by using a while loop and modulo arithmetic to wrap around the string.

4. **Return Result**:

   - If a match is found for any starting index, return `true`; otherwise, return `false`.

### Time Complexity

- **O(n^2)**, where `n` is the length of the strings, as each matching index may require checking the entire string.

### Space Complexity

- **O(n)**, for storing indices where the first character matches.

### Code Implementation

```cpp
class Solution {
public:
    bool rotateString(string s, string goal) {
        int m = s.length(), n = goal.length();
        if (m != n) return false;

        vector<int> presentAt;
        for (int i = 0; i < m; i++) {
            if (goal[0] == s[i]) {
                presentAt.push_back(i);
            }
        }

        if (presentAt.empty()) return false;

        for (int idx : presentAt) {
            int j = 0, k = idx;
            while (j < n && goal[j] == s[k]) {
                j++;
                k = (k + 1) % m;
            }
            if (j == n) return true;
        }

        return false;
    }
};
```

## Approach 2: Efficient Concatenation Check

### Approach

1. **Length Check**:

   - Check if the lengths of `s` and `goal` are equal. If not, return `false`.

2. **Concatenate String**:

   - Create a new string by concatenating `s` with itself.

3. **Check for Substring**:

   - Use the `find` method to check if `goal` is a substring of the concatenated string.

4. **Return Result**:

   - If `goal` is found in the concatenated string, return `true`; otherwise, return `false`.

### Time Complexity

- **O(n)**, where `n` is the length of the strings, as checking for a substring is linear.

### Space Complexity

- **O(n)**, for storing the concatenated string.

### Code Implementation

```cpp
class Solution {
public:
    bool rotateString(string s, string goal) {
        int m = s.length();
        int n = goal.length();

        if (m != n) return false;

        // Concatenate s with itself
        string concatenated = s + s;

        // Check if goal is a substring of concatenated
        return concatenated.find(goal) != string::npos;
    }
};
```
