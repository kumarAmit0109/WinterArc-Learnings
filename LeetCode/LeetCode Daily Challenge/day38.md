# Largest Combination With Bitwise AND Greater Than Zero

**Problem Link**: [Largest Combination With Bitwise AND Greater Than Zero](https://leetcode.com/problems/largest-combination-with-bitwise-and-greater-than-zero)

## Approach 1: Bit Manipulation and Counting Set Bits

### Idea

1. Iterate over all possible bit positions (0 to 23) since the maximum value of any number in the input is limited.
2. For each bit position:
   - Count the number of elements in the array that have the bit set at the current position.
3. The largest combination corresponds to the maximum count of numbers having a bit set at any single position.

### Algorithm

1. Initialize `maxCount` to 0.
2. Iterate over bit positions from 0 to 23:
   - For each bit position, initialize a counter `count`.
   - For every number in the array, check if the bit at the current position is set using `(num & (1 << bit))`.
   - Increment `count` for each number with the bit set.
   - Update `maxCount` with the maximum of `maxCount` and `count`.
3. Return `maxCount`.

### Time Complexity

- **O(24 \* n) = O(n)**, where `n` is the size of the input array, as we iterate over 24 bits for each element.

### Space Complexity

- **O(1)**, as no additional space is used apart from variables.

### Code Implementation

```cpp
class Solution {
public:
    int largestCombination(vector<int>& candidates) {
        int maxCount = 0;
        for (int bit = 0; bit < 24; ++bit) {
            int count = 0;
            for (int num : candidates) {
                if (num & (1 << bit)) {
                    count++;
                }
            }
            maxCount = max(maxCount, count);
        }
        return maxCount;
    }
};
```
