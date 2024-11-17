# Find the Power of K-Size Subarrays I

**Problem Link**: [Find the Power of K-Size Subarrays I](https://leetcode.com/problems/find-the-power-of-k-size-subarrays-i)

## Approach 1: Brute Force with Sliding Windows

### Idea

1. Traverse the array to form all subarrays of size `k`.
2. Check if each subarray has consecutive increasing numbers.
3. If consecutive, return the last element of the subarray; otherwise, return `-1`.

### Algorithm

1. Traverse the array from index `0` to `n-k`.
2. For each window:
   - Check if the elements are consecutive using a helper function.
   - Store the result in the `results` array.
3. Return the `results` array.

### Time Complexity

- **O(n \* k)**: For each of the `n-k` subarrays, check `k` elements.

### Space Complexity

- **O(k)**: Temporary storage for the sliding window.

### Code

```cpp
class Solution {
public:
    int checkSorted(const vector<int>& window) {
        for (int i = 1; i < window.size(); ++i) {
            if (window[i] != window[i - 1] + 1) {
                return -1; // Not consecutive
            }
        }
        return window.back(); // Return the maximum element
    }

    vector<int> resultsArray(vector<int>& nums, int k) {
        vector<int> results;

        // Traverse the array to create sliding windows of size k
        for (int i = 0; i <= nums.size() - k; ++i) {
            vector<int> window(nums.begin() + i, nums.begin() + i + k);
            results.push_back(checkSorted(window));
        }

        return results;
    }
};
```

## Approach 2: Optimized Sliding Window

### Idea

1. Use a sliding window to check consecutive elements efficiently.
2. Maintain a count of consecutive elements within the current window.
3. If the count reaches `k`, store the last element of the window in the result; otherwise, store `-1`.

### Algorithm

1. Initialize the result array with `-1` and a `count` variable to track consecutive numbers.
2. Preprocess the first window:
   - Traverse the first `k` elements to update `count`.
3. Check the first window:
   - If `count == k`, set the result for the first window.
4. Slide the window through the array:
   - Update `count` by checking the new element entering the window.
   - If the count satisfies `k`, update the result for the current window.
5. Return the result array.

### Time Complexity

- **O(n)**: Single pass through the array with constant operations for each step.

### Space Complexity

- **O(1)**: No additional space except for the result array.

### Code

```cpp
class Solution {
public:
    vector<int> resultsArray(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> result(n - k + 1, -1); // Initialize result array with -1
        int count = 1; // Count of consecutive elements

        // Preprocess the first window
        for (int i = 1; i < k; i++) {
            if (nums[i] == nums[i - 1] + 1) {
                count++;
            } else {
                count = 1;
            }
        }

        // Check the first window
        if (count == k) {
            result[0] = nums[k - 1];
        }

        // Sliding window for the rest of the array
        int i = 1, j = k;
        while (j < n) {
            // Check the new element entering the window
            if (nums[j] == nums[j - 1] + 1) {
                count++;
            } else {
                count = 1;
            }

            // If count >= k, store the last element of the window
            if (count >= k) {
                result[i] = nums[j];
            }

            i++;
            j++;
        }

        return result;
    }
};
```
