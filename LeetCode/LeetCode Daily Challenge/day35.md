# String Compression III

**Problem Link**: [String Compression III](https://leetcode.com/problems/string-compression-iii/description/)

## Approach: Character Counting

### Idea

1. **Initialize Variables**:

   - Create an empty string `comp` to store the compressed result.
   - Get the size of `word` and initialize an index `i`.

2. **Count Consecutive Characters**:

   - Use a while loop to iterate through `word`. For each character, count its consecutive occurrences (up to 9) and update the index accordingly.

3. **Build Compressed String**:
   - For each character counted, append the count and the character to `comp`.

### Algorithm

1. Initialize an empty string `comp` and set `i` to 0.
2. Iterate while `i < n`:
   - Store the current character `c`.
   - Count consecutive occurrences of `c` (up to 9).
   - Append `count` and `c` to `comp`.
3. Return the compressed string `comp`.

### Time Complexity

- **O(n)**, where `n` is the length of the string `word`.

### Space Complexity

- **O(n)**, for storing the compressed string.

### Code Implementation

```cpp
class Solution {
public:
    string compressedString(string word) {
        string comp = "";
        int n = word.size();
        int i = 0;

        while (i < n) {
            char c = word[i];
            int count = 0;

            // Count up to 9 consecutive occurrences of the same character
            while (i < n && word[i] == c && count < 9) {
                count++;
                i++;
            }

            // Append the count and character to the result
            comp += to_string(count) + c;
        }

        return comp;
    }
};
```
