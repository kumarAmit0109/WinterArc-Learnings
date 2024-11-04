# Delete Characters to Make Fancy String

**Problem Link**: [Delete Characters to Make Fancy String](https://leetcode.com/problems/delete-characters-to-make-fancy-string/description/)

## Approach: Iterative Check for Consecutive Characters

### Approach

1. **Initialize**:

   - Create an empty string `temp` to store the resulting fancy string.
   - Use an integer `count` to track consecutive occurrences of each character, initialized to 1.

2. **Iterate through the string**:

   - For each character, check if it is the same as the previous character.
   - If it is, increment `count`. If it's not, reset `count` to 1.
   - Only add the character to `temp` if `count` is less than 3 (i.e., there are fewer than three consecutive occurrences).

3. **Return Result**:
   - Return `temp` as the fancy string.

### Time Complexity

- **O(n)**, where `n` is the length of the string, as we iterate through the string once.

### Space Complexity

- **O(n)**, as we use an additional string `temp` to store the result.

### Code Implementation

```cpp
class Solution {
public:
    string makeFancyString(string s) {
        string temp;
        int count = 1; // Track consecutive occurrences

        for (int i = 0; i < s.size(); i++) {
            if (i > 0 && s[i] == s[i - 1]) {
                count++;
            } else {
                count = 1;
            }

            // Only add the character if it doesn't exceed two consecutive occurrences
            if (count < 3) {
                temp.push_back(s[i]);
            }
        }

        return temp;
    }
};
```
