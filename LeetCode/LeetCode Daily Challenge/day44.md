# Count the Number of Fair Pairs

**Problem Link**: [Count the Number of Fair Pairs](https://leetcode.com/problems/count-the-number-of-fair-pairs)

## Approach 1: Brute-Force

### Idea

1. Iterate through all possible pairs `(i, j)` where `i < j`.
2. Compute the sum of each pair.
3. Check if the sum lies within the range `[lower, upper]`.
4. Count the number of such valid pairs.

### Algorithm

1. Initialize `count` to `0`.
2. For each index `i` in the array:
   - For each index `j` where `j > i`:
     - Compute `sum = nums[i] + nums[j]`.
     - If `lower <= sum <= upper`, increment `count`.
3. Return `count`.

### Time Complexity

- **O(n²)**: For each element, iterate through the remaining elements to check pairs.

### Space Complexity

- **O(1)**: No additional space is used apart from variables.

### Code Implementation

```cpp
class Solution {
public:
    long long countFairPairs(vector<int>& nums, int lower, int upper) {
        long long count = 0;
        int n = nums.size();

        // Brute-force approach: Check all pairs
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                int sum = nums[i] + nums[j];
                if (sum >= lower && sum <= upper) {
                    count++;
                }
            }
        }

        return count;
    }
};
```

## Approach 2: Binary Search

### Idea

1. Sort the array to efficiently locate pairs using binary search.
2. For each element `nums[i]`, use binary search to find:
   - The smallest index `left` such that `nums[i] + nums[left] >= lower`.
   - The largest index `right` such that `nums[i] + nums[right] <= upper`.
3. Count the number of valid pairs between `left` and `right` for each `i`.

### Algorithm

1. Sort the array `nums`.
2. Initialize `count` to `0`.
3. For each index `i` in the array:
   - Use binary search to find `left` (first index satisfying `nums[i] + nums[left] >= lower`).
   - Use binary search to find `right` (last index satisfying `nums[i] + nums[right] <= upper`).
   - If `left <= right`, add `(right - left + 1)` to `count`.
4. Return `count`.

### Time Complexity

- **O(n log n)**:
  - Sorting the array takes **O(n log n)**.
  - For each element, binary search is performed twice, taking **O(log n)**, resulting in **O(n log n)** overall for the loop.

### Space Complexity

- **O(1)**: No additional space is used apart from variables.

### Code Implementation

```cpp
class Solution {
public:
    long long countFairPairs(vector<int>& nums, int lower, int upper) {
        sort(nums.begin(), nums.end());
        long long count = 0;
        int n = nums.size();

        for (int i = 0; i < n; i++) {
            int start = i + 1;
            int end = n - 1;

            // Find the first position where sum is >= lower
            while (start <= end) {
                int mid = start + (end - start) / 2;
                if (nums[i] + nums[mid] >= lower) {
                    end = mid - 1;
                } else {
                    start = mid + 1;
                }
            }
            int left = start;

            start = i + 1;
            end = n - 1;

            // Find the last position where sum is <= upper
            while (start <= end) {
                int mid = start + (end - start) / 2;
                if (nums[i] + nums[mid] <= upper) {
                    start = mid + 1;
                } else {
                    end = mid - 1;
                }
            }
            int right = end;

            if (left <= right) {
                count += right - left + 1;
            }
        }

        return count;
    }
};
```

## Approach 3: Using `lower_bound` and `upper_bound`

### Idea

1. Sort the array `nums` to use binary search efficiently.
2. For each element `nums[i]`, calculate the range of valid pairs that satisfy:
   - \( \text{lower} \leq \text{nums[i]} + \text{nums[j]} \leq \text{upper} \)
3. Use:
   - `lower_bound` to find the first valid index \( x \) for \( \text{nums[j]} \geq \text{lower} - \text{nums[i]} \).
   - `upper_bound` to find the first invalid index \( y \) for \( \text{nums[j]} > \text{upper} - \text{nums[i]} \).
4. Add the count of valid indices \( (y - x) \) to the result.

### Algorithm

1. Sort `nums`.
2. Initialize `result` to `0`.
3. For each index \( i \) in `nums`:
   - Use `lower_bound` to find the first valid index for \( \text{nums[j]} \geq \text{lower} - \text{nums[i]} \).
   - Use `upper_bound` to find the first invalid index for \( \text{nums[j]} > \text{upper} - \text{nums[i]} \).
   - Add \( (y - x) \) to `result`.
4. Return `result`.

### Time Complexity

- **O(n log n)**:
  - Sorting the array takes **O(n log n)**.
  - For each element, `lower_bound` and `upper_bound` are used, which take **O(log n)** for each iteration, resulting in **O(n log n)** for the loop.

### Space Complexity

- **O(1)**: No additional space is used.

### Code Implementation

```cpp
class Solution {
public:
    long long countFairPairs(vector<int>& nums, int lower, int upper) {
        int n = nums.size();

        // Sort the array to allow binary search
        sort(nums.begin(), nums.end());

        long long result = 0;

        // Loop through each element as the first element of the pair
        for (int i = 0; i < n; i++) {
            // Find the index of the first valid element >= (lower - nums[i])
            int x = lower_bound(nums.begin() + i + 1, nums.end(), lower - nums[i]) - nums.begin();

            // Find the index of the first invalid element > (upper - nums[i])
            int y = upper_bound(nums.begin() + i + 1, nums.end(), upper - nums[i]) - nums.begin();

            // Add the count of valid pairs for the current `i`
            result += (y - x);
        }

        return result;
    }
};
```
