# Count Number of Maximum Bitwise-OR Subsets

[LeetCode Problem Link](https://leetcode.com/problems/count-number-of-maximum-bitwise-or-subsets)

## Approach: Iterative Subset Generation

### Idea

1. **Enumerate all possible subsets iteratively** using bit masking. Each integer from `0` to `(2^n - 1)` represents a subset.
2. Calculate the **maximum bitwise-OR** of the entire array to identify the target value.
3. For each subset, compute the bitwise-OR of its elements and count how many subsets match the maximum OR value.

### Time Complexity

- **O(n \* 2^n)**: We generate all subsets and compute the bitwise-OR for each.

### Space Complexity

- **O(1)**: Only a few extra variables are used.

### Code

```cpp
class Solution {
public:
    int countMaxOrSubsets(vector<int>& nums) {
        int n = nums.size();
        int maxOr = 0, count = 0;

        // Calculate the maximum bitwise-OR of the entire array
        for (int num : nums) {
            maxOr |= num;
        }

        // Iterate over all possible subsets using bit masking
        for (int mask = 0; mask < (1 << n); ++mask) {
            int currentOr = 0;

            // Calculate the bitwise-OR for the current subset
            for (int i = 0; i < n; ++i) {
                if (mask & (1 << i)) {
                    currentOr |= nums[i];
                }
            }

            // If the subset's OR matches the maximum OR, increase the count
            if (currentOr == maxOr) {
                ++count;
            }
        }

        return count;
    }
};
```
