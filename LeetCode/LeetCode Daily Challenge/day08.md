# Problem: Minimum Number of Swaps to Make the String Balanced

[LeetCode Problem Link](https://leetcode.com/problems/minimum-number-of-swaps-to-make-the-string-balanced/)

## Approach

To make a string of brackets balanced, we can count the number of unmatched `[` and `]` brackets as we traverse the string. The main idea is to count how many `]` are unmatched since each of these needs a corresponding `[` to balance.

### Steps:

1. **Count Unmatched Brackets**:

   - Traverse the string and maintain a count of `[` and `]`.
   - For every `]`, check if there's a corresponding `[`. If there is, decrease the count of unmatched `[`. If not, increase the count of unmatched `]`.

2. **Calculate Required Swaps**:
   - The number of swaps needed to balance the string is half the number of unmatched `]` since each swap can correct two unmatched brackets.

### Time Complexity

- The time complexity is \(O(N)\), where \(N\) is the length of the string, as we iterate through the string once.

### Space Complexity

- The space complexity is \(O(1)\) since we only use a constant amount of extra space.

## C++ Code Implementation

```cpp
class Solution {
public:
    int minSwaps(string s) {
        int openCount = 0; // Count of '[' brackets
        int closeCount = 0; // Count of ']' brackets

        // Traverse the string and count unmatched brackets
        for (char ch : s) {
            if (ch == '[') {
                openCount++; // Increase the count for opening brackets
            } else {
                // If we encounter a closing bracket
                if (openCount > 0) {
                    openCount--; // Match it with an opening bracket
                } else {
                    closeCount++; // Increase the count for unmatched closing brackets
                }
            }
        }

        // The number of swaps needed is half the unmatched closing brackets
        return (closeCount + 1) / 2; // Each swap fixes two unmatched closing brackets
    }
};
```
