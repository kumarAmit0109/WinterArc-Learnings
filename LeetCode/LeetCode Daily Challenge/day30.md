# Minimum Number of Removals to Make Mountain Array

**Problem Link**: [Minimum Number of Removals to Make Mountain Array](https://leetcode.com/problems/minimum-number-of-removals-to-make-mountain-array/)

## Approach

### Prerequisite Knowledge for "Minimum Number of Removals to Make Mountain Array"

Before attempting the problem **Minimum Number of Removals to Make Mountain Array**, it is essential to have a solid understanding of the **Longest Increasing Subsequence** (LIS) concept.

### Key Points:

- The problem requires manipulating sequences and understanding their properties, which is a fundamental aspect of LIS.
- Familiarity with dynamic programming techniques used to solve the LIS problem will aid in efficiently tackling the mountain array problem.

### Approach 1: Recursive Backtracking with Check for Mountain Array

1. **Recursive Function**:

   - Define `removeAndCheck`, which takes a copy of the array `nums`, the current `removals` count, and the minimum removals found so far (`minRemovals`).
   - In each call:
     - If `nums` is a valid mountain array, update `minRemovals` with the minimum number of removals.
     - Otherwise, iterate through `nums`, removing each element one at a time, and recursively call `removeAndCheck` with an incremented `removals` count.

2. **Mountain Array Check**:

   - The function `isMountainArray` verifies if a given array is a mountain array:
     - A valid mountain array has a peak element that is not at the ends of the array.
     - The elements must strictly increase up to the peak and strictly decrease after the peak.

3. **Binary Search for Peak**:

   - Use binary search to find the peak element (local maximum).
   - Once the peak is located, check if the array elements are strictly increasing before the peak and strictly decreasing after it.

4. **Counting Removals**:
   - Start with `minRemovals` set to the size of the array (worst-case scenario).
   - Recursively update `minRemovals` as smaller counts of valid removals are found.

### Time Complexity

- **Exponential Complexity \(O(2^n)\)** due to the recursive removal of each element and verification.
- For each call, the `removeAndCheck` function potentially creates a new array by removing one element, leading to a high number of recursive calls.

### Space Complexity

- **O(n)** for the recursion stack, given the recursive depth in the worst case.

### Summary

- **Time Complexity**: \(O(2^n)\) (inefficient for larger inputs)
- **Space Complexity**: \(O(n)\)

### Code Implementation

```cpp
class Solution {
public:
    void removeAndCheck(vector<int> nums, int removals, int& minRemovals) {
        if (isMountainArray(nums)) {
            minRemovals = min(minRemovals, removals);
            return;
        }

        for (int i = 0; i < nums.size(); ++i) {
            vector<int> newNums = nums;
            newNums.erase(newNums.begin() + i); // Remove the element at index i
            removeAndCheck(newNums, removals + 1, minRemovals); // Increment removals
        }
    }

    bool isMountainArray(const vector<int>& arr) {
        if (arr.size() < 3)
            return false;

        int left = 0, right = arr.size() - 1;

        // Binary search to find the peak
        while (left < right) {
            int mid = left + (right - left) / 2;

            if (arr[mid] < arr[mid + 1]) {
                left = mid + 1; // Move right if we are on the increasing slope
            } else {
                right = mid; // Move left if we are on the decreasing slope or at the peak
            }
        }

        int peak = left;

        // Check if peak is not at the ends (valid mountain condition)
        if (peak == 0 || peak == arr.size() - 1)
            return false;

        // Verify strictly increasing from start to peak
        for (int i = 1; i <= peak; ++i) {
            if (arr[i] <= arr[i - 1])
                return false;
        }

        // Verify strictly decreasing from peak to end
        for (int i = peak + 1; i < arr.size(); ++i) {
            if (arr[i] >= arr[i - 1])
                return false;
        }

        return true;
    }

    int minimumMountainRemovals(vector<int>& nums) {
        int minRemovals = nums.size();
        removeAndCheck(nums, 0, minRemovals);
        return minRemovals;
    }
};
```

### Approach 2: Longest Increasing and Decreasing Subsequences

1. **Dynamic Programming Arrays**:

   - Use two arrays, `leftLIS` and `rightLIS`, to store the lengths of the longest increasing subsequence ending at each index and the longest decreasing subsequence starting at each index, respectively.

2. **Calculate Left LIS**:

   - For each index `i`, iterate through all previous indices `j < i`.
   - If `nums[i] > nums[j]`, update `leftLIS[i]` as `max(leftLIS[i], leftLIS[j] + 1)`.

3. **Calculate Right LIS**:

   - For each index `i`, iterate through all subsequent indices `j > i`.
   - If `nums[i] > nums[j]`, update `rightLIS[i]` as `max(rightLIS[i], rightLIS[j] + 1)`.

4. **Identify Valid Mountains**:

   - Iterate through each index `i` and check if `leftLIS[i] > 1` and `rightLIS[i] > 1`, confirming a valid peak.
   - For each valid peak, calculate the length of the mountain as `leftLIS[i] + rightLIS[i] - 1`, and update `longestMountain`.

5. **Calculate Minimum Removals**:
   - The minimum number of removals to form the longest mountain array is `n - longestMountain`.

### Time Complexity

- **O(n^2)** due to the nested loops in the calculation of `leftLIS` and `rightLIS`.

### Space Complexity

- **O(n)** for the `leftLIS` and `rightLIS` arrays.

### Summary

- **Time Complexity**: \(O(n^2)\)
- **Space Complexity**: \(O(n)\)

### Code Implementation

```cpp
class Solution {
public:
    int minimumMountainRemovals(vector<int>& nums) {
        int n = nums.size();
        vector<int> leftLIS(n, 1), rightLIS(n, 1);

        // Calculate leftLIS for LIS ending at each index
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (nums[i] > nums[j]) {
                    leftLIS[i] = max(leftLIS[i], leftLIS[j] + 1);
                }
            }
        }

        // Calculate rightLIS for LIS starting at each index
        for (int i = n - 2; i >= 0; --i) {
            for (int j = n - 1; j > i; --j) {
                if (nums[i] > nums[j]) {
                    rightLIS[i] = max(rightLIS[i], rightLIS[j] + 1);
                }
            }
        }

        int longestMountain = 0;
        for (int i = 1; i < n - 1; ++i) {
            if (leftLIS[i] > 1 && rightLIS[i] > 1) { // valid peak
                longestMountain = max(longestMountain, leftLIS[i] + rightLIS[i] - 1);
            }
        }

        return n - longestMountain;
    }
};
```

# Longest Increasing Subsequence

### Problem Link

[Longest Increasing Subsequence - LeetCode](https://leetcode.com/problems/longest-increasing-subsequence)

## Approach 1: Recursive Approach

### Algorithm

1. Use a helper function to recursively find the LIS for each subsequence starting from the `currentIndex`.
2. For each element, we have two choices:
   - **Exclude** the current element and move to the next index.
   - **Include** the current element if it is greater than the element at `prevIndex`, then move to the next index.
3. Return the maximum of including or excluding the current element.

### Time Complexity

- \( O(2^n) \), where \( n \) is the length of `nums`.

### Space Complexity

- \( O(n) \), for recursive stack depth.

### Code

```cpp
class Solution {
public:
    int lengthOfLISHelper(vector<int>& nums, int prevIndex, int currentIndex) {
        if (currentIndex == nums.size()) {
            return 0;
        }
        int exclude = lengthOfLISHelper(nums, prevIndex, currentIndex + 1);
        int include = 0;
        if (prevIndex == -1 || nums[currentIndex] > nums[prevIndex]) {
            include = 1 + lengthOfLISHelper(nums, currentIndex, currentIndex + 1);
        }
        return max(include, exclude);
    }

    int lengthOfLIS(vector<int>& nums) {
        return lengthOfLISHelper(nums, -1, 0);
    }
};
```

## Approach 2: Recursive with Memoization

### Algorithm

1. **Recursive Calls with Memoization**:
   - Same as the recursive approach, but add memoization by using a DP table to store results of subproblems, reducing redundant calculations.
   - For each element, make two recursive calls:
     - **Exclude** the current element and move to the next index.
     - **Include** the current element if it is greater than the element at `prevIndex`, then move to the next index.
   - Store the result for each `(currentIndex, prevIndex + 1)` combination in the DP table to avoid recomputation.
2. **Base Cases**:
   - If `currentIndex` is out of bounds, return 0.

### Time Complexity

- \( O(n^2) \), where \( n \) is the length of `nums`, due to memoization.

### Space Complexity

- \( O(n^2) \), for the DP table to store results of each subproblem.

### Code

```cpp
class Solution {
public:
    int lengthOfLISHelper(vector<int>& nums, int prevIndex, int currentIndex, vector<vector<int>>& dp) {
        if (currentIndex == nums.size()) {
            return 0;
        }
        if (dp[currentIndex][prevIndex + 1] != -1) {
            return dp[currentIndex][prevIndex + 1];
        }
        int exclude = lengthOfLISHelper(nums, prevIndex, currentIndex + 1, dp);
        int include = 0;
        if (prevIndex == -1 || nums[currentIndex] > nums[prevIndex]) {
            include = 1 + lengthOfLISHelper(nums, currentIndex, currentIndex + 1, dp);
        }
        return dp[currentIndex][prevIndex + 1] = max(include, exclude);
    }

    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> dp(n, vector<int>(n + 1, -1));
        return lengthOfLISHelper(nums, -1, 0, dp);
    }
};
```

## Approach 3: Dynamic Programming

### Algorithm

1. **Dynamic Programming Array Setup**:
   - Create a `dp` array where each entry `dp[i]` represents the length of the longest increasing subsequence ending at index `i`.
2. **Filling the DP Array**:
   - Initialize all values in `dp` to 1, as each element can be an LIS of length 1 by itself.
   - For each element `nums[i]`, look at all previous elements `nums[j]` (where `j < i`):
     - If `nums[i] > nums[j]`, update `dp[i]` to be the maximum of `dp[i]` and `dp[j] + 1`.
   - Track the overall maximum length found in the `dp` array.
3. **Return the Maximum**:
   - The longest increasing subsequence length is the maximum value in `dp`.

### Time Complexity

- \( O(n^2) \), where \( n \) is the length of `nums`, due to the nested loops.

### Space Complexity

- \( O(n) \), for the `dp` array to store LIS lengths up to each index.

### Code

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) return 0;

        // DP array to store the LIS ending at each index
        vector<int> dp(n, 1);

        int ans = 1;
        // Fill the DP array
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                    ans = max(ans, dp[i]);
                }
            }
        }

        return ans;
    }
};
```
