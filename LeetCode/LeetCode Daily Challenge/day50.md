# Maximum Sum of Distinct Subarrays with Length K

**Problem Link**: [Maximum Sum of Distinct Subarrays with Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

## Approach: Sliding Window with Frequency Map

### Idea

1. Use a sliding window to maintain a subarray of size `k` while ensuring all elements in the subarray are distinct.
2. Use a frequency map to track the occurrence of elements in the current window.
3. Adjust the window size dynamically:
   - Shrink the window if duplicates are found or the window size exceeds `k`.
   - Compute the sum of elements in valid subarrays and update the maximum sum.

### Algorithm

1. Initialize `maxSum` to store the maximum sum and `currentSum` to track the sum of the current window.
2. Use an unordered map `freq` to track the frequency of elements in the current window.
3. Traverse the array using two pointers:
   - Add the current element to the frequency map and update `currentSum`.
   - If the window has duplicates or exceeds size `k`, remove elements from the left until the window is valid.
   - If the window size equals `k` and all elements are distinct, update `maxSum` with `currentSum`.
4. Return `maxSum`.

### Time Complexity

- **O(n)**: Each element is added and removed from the window at most once.

### Space Complexity

- **O(k)**: Space for the frequency map to store up to `k` elements.

### Code

```cpp
class Solution {
public:
    long long maximumSubarraySum(vector<int>& nums, int k) {
        long long maxSum = 0, currentSum = 0;
        unordered_map<int, int> freq; // To track the frequency of elements in the current window
        int left = 0;

        for (int right = 0; right < nums.size(); ++right) {
            freq[nums[right]]++;
            currentSum += nums[right];

            // If the window size exceeds k or there are duplicate elements, shrink the window
            while (freq[nums[right]] > 1 || (right - left + 1) > k) {
                freq[nums[left]]--;
                if (freq[nums[left]] == 0) {
                    freq.erase(nums[left]);
                }
                currentSum -= nums[left];
                left++;
            }

            // Check if the current window has exactly k distinct elements
            if ((right - left + 1) == k) {
                maxSum = max(maxSum, currentSum);
            }
        }

        return maxSum;
    }
};
```
