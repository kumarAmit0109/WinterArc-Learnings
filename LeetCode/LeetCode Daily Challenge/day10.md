# Problem: Maximum Width Ramp

[LeetCode Problem Link](https://leetcode.com/problems/maximum-width-ramp/description/)

## Approach 1: Using Stack

To find the maximum width ramp efficiently, we can use a stack-based approach. This method allows us to maintain potential starting indices of the ramp while traversing the array in two passes:

1. **Identifying Potential Left Endpoints**:

   - In the first pass (left to right), we traverse the array and push indices onto a stack. We only push an index if the current number is less than the number at the index on top of the stack. This ensures that the stack maintains indices of elements in increasing order of their values.

2. **Calculating Maximum Width**:
   - In the second pass (right to left), we check for maximum width by iterating through the array from the end. For each element at index `j`, we check if it can form a ramp with any of the indices stored in the stack. If `nums[j] >= nums[indexStack.top()]`, we compute the width as `j - indexStack.top()`, updating our maximum width as we find valid ramps.

### Time Complexity

- The time complexity is \(O(N)\), where \(N\) is the number of elements in the input vector. Each element is pushed and popped from the stack at most once.

### Space Complexity

- The space complexity is \(O(N)\) in the worst case, where we may need to store all indices in the stack.

### C++ Code Implementation

```cpp
class Solution {
public:
    int maxWidthRamp(vector<int>& nums) {
        stack<int> indexStack; // Stack to store indices of the elements
        int size = nums.size(); // Get the size of the input vector
        int maxWidth = 0; // Variable to store the maximum width found

        // First pass: Store indices of potential left endpoints in the stack
        for (int i = 0; i < size; ++i) {
            // Push index to stack only if the current number is less than the number at the index on top of the stack
            if (indexStack.empty() || nums[indexStack.top()] > nums[i]) {
                indexStack.push(i); // Push current index onto the stack
            }
        }

        // Second pass: Check for maximum width by traversing from the end of the array
        for (int j = size - 1; j >= 0; --j) {
            // While there are indices in the stack and the current number is greater than or equal to the number at the index on top of the stack
            while (!indexStack.empty() && nums[j] >= nums[indexStack.top()]) {
                // Update maxWidth with the maximum of the current width found
                maxWidth = max(maxWidth, j - indexStack.top());
                indexStack.pop(); // Remove the top index from the stack
            }
        }

        return maxWidth; // Return the maximum width found
    }
};
```
