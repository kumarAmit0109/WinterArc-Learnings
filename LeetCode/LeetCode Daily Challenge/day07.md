# Problem: Minimum String Length After Removing Substrings

[LeetCode Problem Link](https://leetcode.com/problems/minimum-string-length-after-removing-substrings)

## Approach

To solve the problem of minimizing the string length after removing substrings "AB" and "CD", we can use an iterative approach. The steps are as follows:

1. **Initialize a Flag**: Use a boolean flag to track whether any pairs ("AB" or "CD") have been found and removed in the current traversal.
2. **Loop Until No Pairs Are Found**: Repeat the following until no pairs are found:
   - Reset the flag to `false` at the start of each traversal.
   - Initialize a temporary string to store the modified result.
   - Traverse the original string:
     - Check for the presence of "AB" or "CD":
       - If found, set the flag to `true` and skip the next character.
       - If not found, keep the current character and add it to the temporary string.
3. **Update the Original String**: After traversing, update the original string with the modified result.
4. **Return Result**: Once no pairs are found, return the length of the modified string.

### Time Complexity

- The time complexity is \(O(n^2)\), where \(n\) is the length of the string, as we may traverse the string multiple times.

### Space Complexity

- The space complexity is \(O(n)\), where \(n\) is the length of the modified string stored in memory.

## C++ Code Implementation

```cpp
class Solution {
public:
    int minLength(string s) {
        bool found; // To track if any pair was found and removed

        do {
            found = false; // Reset the flag for this traversal
            string newString; // Temporary string to store the modified result

            for (int i = 0; i < s.size(); ) {
                // Check for "AB" and "CD"
                if (i < s.size() - 1 && ((s[i] == 'A' && s[i + 1] == 'B') || (s[i] == 'C' && s[i + 1] == 'D')) ) {
                    found = true; // Set flag to true as we found a pair
                    i += 2; // Skip the next character as well
                } else {
                    newString += s[i]; // Keep the current character
                    i++;
                }
            }

            // Update the original string with the new one
            s = newString;
        } while (found); // Repeat until no pairs are found

        return s.size(); // Return the length of the modified string
    }
};
```
