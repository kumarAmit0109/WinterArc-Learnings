# Find if Array Can Be Sorted

**Problem Link**: [Find if Array Can Be Sorted](https://leetcode.com/problems/find-if-array-can-be-sorted/)

## Approach: Bubble Sort with Custom Swap Condition

### Idea

1. Perform a bubble sort on the array.
2. Allow swaps only if:
   - Two adjacent elements are not in order.
   - They have the same number of set bits (`__builtin_popcount`).
3. If any pair cannot be swapped to meet the sorting condition, return `false`.

### Algorithm

1. Initialize `n` as the size of the array and set `swapped` to `true`.
2. For each pass:
   - Reset `swapped` to `false`.
   - Iterate through the array and compare adjacent elements.
   - If elements are out of order and cannot be swapped based on the set bits condition, return `false`.
   - Otherwise, perform the swap and set `swapped = true`.
3. If no swaps are made in a pass, the array is already sorted; break the loop.
4. Return `true` if the array is sorted successfully.

### Time Complexity

- **O(n²)** in the worst case due to the nested loops of the bubble sort.

### Space Complexity

- **O(1)**, as no additional space is used apart from variables.

### Code Implementation

```cpp
class Solution {
public:
    bool canSortArray(vector<int>& nums) {
        int n = nums.size();
        bool swapped = true;

        for (int i = 0; i < n; i++) {
            swapped = false;

            for (int j = 0; j < n - i - 1; j++) { // In every pass, the max element bubbles up to the rightmost index
                if (nums[j] <= nums[j + 1]) { // No swap required
                    continue;
                } else { // Swap is required
                    if (__builtin_popcount(nums[j]) == __builtin_popcount(nums[j + 1])) {
                        swap(nums[j], nums[j + 1]);
                        swapped = true;
                    } else {
                        // Not able to swap, hence can't sort it
                        return false;
                    }
                }
            }

            if (swapped == false) { // No swapping was done in the pass, hence array was already sorted
                break; // No more passes are required
            }
        }

        return true;
    }
};
```
