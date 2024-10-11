# Problem: Make Sum Divisible by P

[LeetCode Problem Link](https://leetcode.com/problems/make-sum-divisible-by-p/description/)

## Approach

To solve the problem of making the sum of an array divisible by \( p \), we can break down the approach into the following steps:

1. **Calculate Total Sum**:

   - First, compute the total sum of the elements in the array.

2. **Find Remainder**:

   - Determine the remainder of the total sum when divided by \( p \). If the remainder is zero, the sum is already divisible by \( p \), and we can return 0.

3. **Prefix Sum with HashMap**:

   - Use a hashmap to keep track of the prefix sums modulo \( p \).
   - As we iterate through the array, we calculate the current prefix sum.
   - For each prefix sum, calculate the target prefix sum that we need to find in the hashmap to determine the subarray that, when removed, would make the sum divisible by \( p \).

4. **Determine Minimum Length**:

   - If the target prefix sum exists in the hashmap, calculate the length of the subarray to remove and update the minimum length accordingly.

5. **Return Result**:
   - If no valid subarray is found, return -1. Otherwise, return the length of the minimum subarray.

## Time Complexity

- The time complexity of this approach is \( O(n) \), where \( n \) is the number of elements in the input array, since we traverse the array once.

## Space Complexity

- The space complexity is \( O(n) \) due to the hashmap used to store prefix sums.

## C++ Code Implementation

```cpp
#include <vector>
#include <unordered_map>
using namespace std;

class Solution {
public:
    int minSubarray(vector<int>& nums, int p) {
        long long totalSum = 0;
        for (int num : nums) {
            totalSum += num; // Calculate the total sum of the array
        }

        long long remainder = totalSum % p; // Find the remainder
        if (remainder == 0) return 0; // Already divisible by p

        unordered_map<long long, int> prefixSum; // To store prefix sums
        prefixSum[0] = -1; // Initialize with prefix sum 0 at index -1
        long long currSum = 0; // Current prefix sum
        int minLength = nums.size(); // Initialize to maximum length

        // Iterate through the array to find the minimum length subarray
        for (int i = 0; i < nums.size(); ++i) {
            currSum += nums[i]; // Update the current prefix sum
            long long target = (currSum - remainder + p) % p; // Calculate the target prefix sum

            // Check if the target prefix sum exists in the map
            if (prefixSum.find(target) != prefixSum.end()) {
                minLength = min(minLength, i - prefixSum[target]); // Update minimum length
            }
            prefixSum[currSum % p] = i; // Store the current prefix sum
        }

        return minLength == nums.size() ? -1 : minLength; // Return result
    }
};
```
