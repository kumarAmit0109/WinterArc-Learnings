# Shortest Subarray to be Removed to Make Array Sorted

**Problem Link**: [Shortest Subarray to be Removed to Make Array Sorted](https://leetcode.com/problems/shortest-subarray-to-be-removed-to-make-array-sorted)

## Approach: Two-Pointer Method

### Idea

1. **Initial Observation**: The task is to find the shortest subarray that needs to be removed to make the array sorted in non-decreasing order.
2. **Algorithm**:
   - First, identify the longest sorted prefix from the start of the array.
   - Then, try to find a valid subarray by maintaining a two-pointer approach where one pointer starts from the beginning of the array and the other moves backward from the end.
   - If the elements at the two pointers can form a valid sorted subarray by removing the intermediate section, calculate the length of the subarray to be removed.

### Steps

1. **Step 1: Find the sorted suffix**: Find the largest sorted portion from the right side of the array, and let `j` be the index from where the sorted array stops.
2. **Step 2: Find valid pairs**: Use two pointers, one at the beginning and one at `j`, to check if the array can be sorted by removing a section between these pointers.
3. **Step 3: Compute the shortest subarray**: Try to minimize the length of the subarray to be removed.

### Time Complexity

- **O(n)**: Both pointers `i` and `j` traverse the array only once, making the algorithm run in linear time.

### Space Complexity

- **O(1)**: The solution uses a constant amount of extra space.

### Code Implementation

```cpp
class Solution {
public:
    int findLengthOfShortestSubarray(vector<int>& arr) {
        int n = arr.size();

        // Step 1: Find the j-th pointer from the right side
        int j = n - 1;
        while (j > 0 && arr[j] >= arr[j - 1]) {
            j--;
        }

        if (j == 0) { // Already sorted
             return 0;
        }

        int i = 0;
        int result = j; // Remove all elements to the left of index j

        // Step 2: Start finding correct i and j to minimize subarray to remove
        while (i < j && (i == 0 || arr[i] >= arr[i - 1])) {
            // Ensure arr[i] <= arr[j]
            while (j < n && arr[i] > arr[j]) {
                j++;
            }

            // Update result with the minimum subarray length to remove
            result = min(result, j - i - 1);
            i++;
        }

        return result;
    }
};
```
