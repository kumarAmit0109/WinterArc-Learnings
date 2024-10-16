# Problem: Separate Black and White Balls

[LeetCode Problem Link](https://leetcode.com/problems/separate-black-and-white-balls)

## Approach

1. **Goal**:

   - The goal is to separate the '0's and '1's such that all '0's are on one side and all '1's are on the other, with the minimum number of swaps.

2. **Optimal Strategy**:

   - Start iterating from the **right to left** to count the number of '0's encountered and calculate how many swaps are needed to move '1's over the '0's.

3. **Counting Swaps**:

   - For each '1', the number of swaps required to move it to the left of all the encountered '0's equals the current count of '0's.
   - For each '0', increase the count of zeros since they are already in their correct position when appearing from the right side.

4. **Final Result**:
   - The total swaps accumulated provide the minimum number of swaps required to rearrange the string.

### Time Complexity

- **Time**: \(O(n)\), where \(n\) is the length of the input string. We traverse the string once.
- **Space**: \(O(1)\), as no additional space is required beyond variables.

### C++ Code Implementation

```cpp
class Solution {
public:
    long long minimumSteps(string s) {
        long long swaps = 0; // Total swaps needed
        int zerosCount = 0;  // Count of '0's encountered from the right

        // Iterate from right to left
        for (int i = s.length() - 1; i >= 0; i--) {
            if (s[i] == '0') {
                zerosCount++; // Increment the zero count
            } else { // s[i] == '1'
                swaps += zerosCount; // Add the number of zeros to swaps
            }
        }

        return swaps; // Return the total minimum swaps
    }
};
```
