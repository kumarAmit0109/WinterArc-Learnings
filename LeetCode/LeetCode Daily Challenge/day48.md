# Shortest Subarray with Sum at Least K

**Problem Link**: [Shortest Subarray with Sum at Least K](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/)

## Approach: Prefix Sum with Monotonic Deque

### Idea

1. Use a prefix sum to calculate the cumulative sum up to each index efficiently.
2. Utilize a monotonic deque to find the shortest subarray satisfying the condition `prefixSum[j] - prefixSum[i] >= k`.

### Algorithm

1. Compute the prefix sum for the input array.
2. Traverse the prefix sum array:
   - Check if the difference between the current prefix sum and the smallest prefix sum in the deque is greater than or equal to `k`. If so, update the minimum length and remove the front element of the deque.
   - Remove elements from the deque's back if they are greater than or equal to the current prefix sum to maintain a non-decreasing order.
   - Add the current index to the deque.
3. If no valid subarray is found, return `-1`; otherwise, return the minimum length.

### Time Complexity

- **O(n)**: Each element is pushed and popped from the deque at most once.

### Space Complexity

- **O(n)**: Space for the prefix sum array and deque.

### Code

```cpp
class Solution {
public:
    int shortestSubarray(vector<int>& nums, int k) {
        int n = nums.size();
        vector<long long> prefixSum(n + 1, 0);
        for (int i = 0; i < n; ++i) {
            prefixSum[i + 1] = prefixSum[i] + nums[i];
        }

        deque<int> dq;
        int minLength = INT_MAX;

        for (int i = 0; i <= n; ++i) {
            // Check if we can form a valid subarray
            while (!dq.empty() && prefixSum[i] - prefixSum[dq.front()] >= k) {
                minLength = min(minLength, i - dq.front());
                dq.pop_front();
            }

            // Maintain increasing order of prefix sums in the deque
            while (!dq.empty() && prefixSum[i] <= prefixSum[dq.back()]) {
                dq.pop_back();
            }

            dq.push_back(i);
        }

        return minLength == INT_MAX ? -1 : minLength;
    }
};
```
