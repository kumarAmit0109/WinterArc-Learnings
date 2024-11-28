# Take K of Each Character from Left and Right

**Problem Link**: [Take K of Each Character from Left and Right](https://leetcode.com/problems/take-k-of-each-character-from-left-and-right/)

## Approach 1: Recursive Backtracking

### Idea

1. Use recursion to explore all possible ways of removing characters from the left or right.
2. Track the frequency of each character (`'a'`, `'b'`, `'c'`) in the remaining string.
3. Stop recursion if:
   - The frequency of all characters is at least `k`.
   - The range of the string becomes invalid.
4. At each step, calculate the number of minutes required and minimize it globally.

### Algorithm

1. Start with the entire string and recursively try two options:
   - Remove a character from the left and update the frequency.
   - Remove a character from the right and update the frequency.
2. Stop the recursion if:
   - The frequency of `'a'`, `'b'`, and `'c'` are all at least `k` (update the result with the minimum minutes).
   - The range of the string becomes invalid (i.e., left pointer crosses the right pointer).
3. Return the minimum number of minutes required, or `-1` if no valid configuration is found.

### Time Complexity

- **O(2^n)**: For each character, two choices are made, leading to exponential complexity.

### Space Complexity

- **O(n)**: For the recursion stack.

### Code

```cpp
class Solution {
public:
    int result = INT_MAX;

    void solve(string &s, int k, int i, int j, int minutes, vector<int> freq) {
        // Base case: If the frequency of each character is at least k
        if (freq[0] >= k && freq[1] >= k && freq[2] >= k) {
            result = min(result, minutes);
            return;
        }

        // If the range is invalid, stop recursion
        if (i > j) return;

        // Option 1: Remove from the left
        vector<int> freqLeft = freq;
        freqLeft[s[i] - 'a']++;
        solve(s, k, i + 1, j, minutes + 1, freqLeft);

        // Option 2: Remove from the right
        vector<int> freqRight = freq;
        freqRight[s[j] - 'a']++;
        solve(s, k, i, j - 1, minutes + 1, freqRight);
    }

    int takeCharacters(string s, int k) {
        vector<int> freq(3, 0); // Frequency of 'a', 'b', and 'c'
        int i = 0, j = s.size() - 1;
        int minutes = 0;

        solve(s, k, i, j, minutes, freq);

        return result == INT_MAX ? -1 : result;
    }
};
```

## Approach 2: Sliding Window

### Idea

1. Use a sliding window to find the largest valid substring where removing the remaining characters ensures at least `k` occurrences of `'a'`, `'b'`, and `'c'`.
2. Track the counts of each character in the window using a frequency array.
3. Adjust the window dynamically:
   - Shrink the window if removing characters violates the condition of having at least `k` occurrences of each character.
   - Expand the window otherwise, while keeping track of the maximum valid window size.
4. The result is the total string length minus the size of the largest valid window.

### Algorithm

1. Count the total occurrences of `'a'`, `'b'`, and `'c'` in the string.
2. If any character occurs less than `k` times, return `-1`.
3. Initialize the sliding window with two pointers, `i` and `j`.
4. Traverse the string:
   - Add the character at `j` to the window and reduce its frequency in the count array.
   - If the window becomes invalid (any count is less than `k`), move the left pointer (`i`) to shrink the window, restoring the character frequencies.
   - Update the size of the largest valid window.
5. Return the total length of the string minus the largest valid window size.

### Time Complexity

- **O(n)**: Each character is added to and removed from the window at most once.

### Space Complexity

- **O(1)**: Fixed space for the frequency array.

### Code

```cpp
class Solution {
public:
    int takeCharacters(string s, int k) {
        int n = s.length();
        vector<int> count(3, 0); // Vector to store counts of 'a', 'b', 'c'

        // Count total frequency of 'a', 'b', and 'c'
        for (char c : s) {
            count[c - 'a']++;
        }

        // If any character appears less than k times, return -1
        if (count[0] < k || count[1] < k || count[2] < k) {
            return -1;
        }

        // Initialize sliding window variables
        int i = 0, j = 0, notDeletedWindowSize = 0;

        while (j < n) {
            // Decrease the count as characters are considered in the window
            count[s[j] - 'a']--;

            // Shrink the window if any count is less than k
            while (i <= j && (count[0] < k || count[1] < k || count[2] < k)) {
                count[s[i] - 'a']++;
                i++;
            }

            // Calculate the maximum size of the valid window
            notDeletedWindowSize = max(notDeletedWindowSize, j - i + 1);
            j++;
        }

        // The result is the total length minus the largest valid window size
        return n - notDeletedWindowSize;
    }
};
```
