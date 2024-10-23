# Split a String Into the Max Number of Unique Substrings

[LeetCode Problem Link](https://leetcode.com/problems/split-a-string-into-the-max-number-of-unique-substrings/description)

## Approach: Backtracking with a Set

### Idea

1. **Set to Track Unique Substrings**:
   - Use a set to track which substrings have been used already.
2. **Backtracking**:

   - Try each possible substring starting from the current position.
   - If the substring is not in the set, add it to the set and recursively find the next substring.
   - Backtrack if no further unique split is possible.

3. **Maximizing Unique Substrings**:
   - Keep track of the maximum number of unique substrings found during the backtracking process.

### Time Complexity

- **O(2^n)**: For each position in the string, we try multiple possible substrings, leading to an exponential number of combinations.

### Space Complexity

- **O(n)**: The recursion depth is proportional to the length of the string, and the set can store up to `n` substrings.

### Code

```cpp
class Solution {
public:
    int maxUniqueSplit(string s) {
        unordered_set<string> seen;  // To store unique substrings
        return backtrack(s, 0, seen);
    }

    // Helper function for backtracking
    int backtrack(const string& s, int start, unordered_set<string>& seen) {
        if (start == s.size()) return 0;  // Base case: reached the end of the string

        int maxSplits = 0;

        // Try every possible substring starting from `start`
        for (int end = start + 1; end <= s.size(); ++end) {
            string substr = s.substr(start, end - start);  // Current substring

            if (seen.find(substr) == seen.end()) {  // If the substring is not already used
                seen.insert(substr);  // Add it to the set
                // Recursively find the next split and update maxSplits
                maxSplits = max(maxSplits, 1 + backtrack(s, end, seen));
                seen.erase(substr);  // Backtrack: remove the substring from the set
            }
        }

        return maxSplits;
    }
};
```
