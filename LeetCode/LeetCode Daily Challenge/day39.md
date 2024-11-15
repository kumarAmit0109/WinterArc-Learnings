# Maximum XOR for Each Query

**Problem Link**: [Maximum XOR for Each Query](https://leetcode.com/problems/maximum-xor-for-each-query/)

## Approach 1: Brute Force Subarray XOR Calculation

### Idea

1. For each query, calculate the XOR of the subarray from `nums[0]` to `nums[i-1]`.
2. Find `k` such that `k ^ subarrayXOR = maxXOR`, where `maxXOR = (1 << maximumBit) - 1`.
3. Push `k` into the result.

### Algorithm

1. Compute `maxXOR = (1 << maximumBit) - 1`.
2. Iterate `i` from `nums.size()` down to 1:
   - Calculate the XOR of the subarray `nums[0]` to `nums[i-1]`.
   - Find `k = maxXOR ^ subarrayXOR` and append it to the result.
3. Return the result.

### Time Complexity

- **O(n²)**: Nested loops for subarray XOR calculation.

### Space Complexity

- **O(n)**: For storing the result.

### Code Implementation

```cpp
class Solution {
public:
    vector<int> getMaximumXor(vector<int>& nums, int maximumBit) {
        int maxXOR = (1 << maximumBit) - 1;  // Calculate 2^maximumBit - 1
        vector<int> ans;

        for (int i = nums.size(); i > 0; --i) {
            int subarrayXOR = 0;
            // Calculate XOR for the subarray nums[0] to nums[i-1]
            for (int j = 0; j < i; ++j) {
                subarrayXOR ^= nums[j];
            }
            // Find the maximum possible k
            int k = maxXOR ^ subarrayXOR;
            ans.push_back(k);
        }

        return ans;
    }
};
```

## Approach 2: Optimized Using Total XOR

### Idea

1. Precompute the XOR of the entire array (`totalXOR`).
2. For each query:
   - Calculate `k = maxXOR ^ totalXOR` directly.
   - Update `totalXOR` by removing the last element of `nums`.
3. Append `k` to the result in reverse order.

### Algorithm

1. Compute `maxXOR = (1 << maximumBit) - 1` and initialize `totalXOR` as the XOR of all elements in `nums`.
2. Reserve space for the result to optimize memory allocation.
3. Iterate `i` from `nums.size() - 1` to 0:
   - Calculate `k = maxXOR ^ totalXOR`.
   - Append `k` to the result.
   - Update `totalXOR` by removing the last element (`totalXOR ^= nums[i]`).
4. Return the result.

### Time Complexity

- **O(n)**: Single traversal of `nums` for XOR calculations.

### Space Complexity

- **O(n)**: For storing the result.

### Code Implementation

```cpp
class Solution {
public:
    vector<int> getMaximumXor(vector<int>& nums, int maximumBit) {
        int maxXOR = (1 << maximumBit) - 1;  // Calculate 2^maximumBit - 1
        int totalXOR = 0;

        // Calculate the XOR of all elements in nums
        for (int num : nums) {
            totalXOR ^= num;
        }

        vector<int> result;
        result.reserve(nums.size());  // Reserve space for efficiency

        // Iterate through nums in reverse to handle each query
        for (int i = nums.size() - 1; i >= 0; --i) {
            result.push_back(maxXOR ^ totalXOR);  // Calculate k for the current state
            totalXOR ^= nums[i];  // Update totalXOR by removing the last element
        }

        return result;
    }
};
```
