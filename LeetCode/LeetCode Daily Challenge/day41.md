# Shortest Subarray with OR at Least K II

**Problem Link**: [Shortest Subarray with OR at Least K II](https://leetcode.com/problems/shortest-subarray-with-or-at-least-k-ii/)

## Approach 1: Brute Force with Sliding Window

### Idea

1. Use a sliding window approach with two pointers `left` and `right`.
2. For each position of `right`, calculate the bitwise OR for the subarray `[left, right]` by iterating backward from `right`.
3. If the OR value meets or exceeds `k`, update the minimum subarray length and move the `left` pointer.

### Algorithm

1. Initialize `minLength` to an impossible large value (`n + 1`).
2. Iterate `right` over the array:
   - Calculate the OR of the subarray `[left, right]`.
   - If the OR is greater than or equal to `k`, update `minLength` and adjust the `left` pointer.
3. Return `minLength` if valid, otherwise `-1`.

### Time Complexity

- **O(n²)**: Nested iteration to compute OR for each subarray.

### Space Complexity

- **O(1)**: No additional space used.

### Code Implementation

```cpp
class Solution {
public:
    int minimumSubarrayLength(vector<int>& nums, int k) {
        int n = nums.size();
        int minLength = n + 1;  // Set initially to an impossible large value
        int left = 0;           // Left pointer for sliding window

        for (int right = 0; right < n; ++right) {
            int currentOR = 0; // Reset currentOR when adjusting window

            // Calculate OR for the current subarray [left, right]
            for (int i = right; i >= left; --i) {
                currentOR |= nums[i];

                // Check if current OR meets or exceeds k
                if (currentOR >= k) {
                    minLength = min(minLength, right - i + 1);
                    left = i + 1;  // Move left pointer to the next position
                    break;
                }
            }
        }

        return minLength == n + 1 ? -1 : minLength;
    }
};
```

## Approach 2: Optimized with Bit Manipulation

### Idea

1. Use a sliding window approach with a 32-bit array `bitPresence` to track the presence of each bit in the current window.
2. Maintain the current OR value (`currentOR`) and efficiently adjust it when shrinking the window.
3. Shrink the window from the left while the current OR meets or exceeds `k` and update the minimum subarray length.

### Algorithm

1. Initialize `bitPresence` (32 bits) to track the number of elements contributing to each bit in the current window.
2. Set `currentOR` to 0 and `minLen` to `INT_MAX`.
3. Iterate `right` over the array:
   - Update `currentOR` and `bitPresence` for `nums[right]`.
   - While `currentOR >= k`, shrink the window from the left:
     - Update `minLen` with the current window size.
     - Adjust `bitPresence` and recalculate `currentOR`.
     - Move the `left` pointer.
4. Return `minLen` if valid, otherwise `-1`.

### Time Complexity

- **O(n \* 32)**: Each adjustment involves recalculating OR for 32 bits.

### Space Complexity

- **O(1)**: Using a fixed-size array for `bitPresence`.

### Code Implementation

```cpp
class Solution {
public:
    int minimumSubarrayLength(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> bitPresence(32, 0);
        int left = 0, currentOR = 0, minLen = INT_MAX;

        for (int right = 0; right < n; ++right) {
            currentOR |= nums[right];

            // Update the bit counts for the current number
            for (int bit = 0; bit < 32; ++bit) {
                if (nums[right] & (1 << bit)) {
                    bitPresence[bit]++;
                }
            }

            // Shrink the window from the left while current OR meets or exceeds k
            while (left <= right && currentOR >= k) {
                minLen = min(minLen, right - left + 1);

                // Recalculate the OR for the window by adjusting bit counts
                int newOR = 0;
                for (int bit = 0; bit < 32; ++bit) {
                    if (nums[left] & (1 << bit)) {
                        bitPresence[bit]--;
                    }
                    if (bitPresence[bit] > 0) {
                        newOR |= (1 << bit);
                    }
                }

                currentOR = newOR;  // Update current OR
                ++left;  // Move the left pointer
            }
        }

        return minLen == INT_MAX ? -1 : minLen;
    }
};
```
