# Maximum Matrix Sum

**Problem Link**: [Maximum Matrix Sum](https://leetcode.com/problems/maximum-matrix-sum/description/)

---

## Approach: Summation with Adjustment for Negatives

### Idea

1. **Absolute Values**:

   - To maximize the sum of the matrix, we focus on absolute values of the elements.

2. **Key Observations**:

   - If the count of negative numbers in the matrix is even, we can pair them, making all values positive without any adjustment.
   - If the count of negative numbers is odd, one number will remain negative. In this case, we adjust by subtracting twice the smallest absolute value from the total sum.

3. **Steps**:

   - Compute the total sum using the absolute values of the elements.
   - Track the smallest absolute value to handle adjustments.
   - Count the number of negative values in the matrix.

4. **Final Adjustment**:
   - If the count of negatives is odd, subtract `2 * minElement` from the sum.

### Algorithm

1. Initialize variables:

   - `sum` to store the total sum of absolute values.
   - `minElement` to track the smallest absolute value.
   - `negativeCount` to count the negative elements.

2. Iterate through the matrix:

   - Add the absolute value of each element to `sum`.
   - Update `minElement` with the smallest absolute value.
   - Increment `negativeCount` for each negative element.

3. Adjust the sum:

   - If `negativeCount % 2 != 0`, subtract `2 * minElement`.

4. Return the adjusted sum.

### Time Complexity

- **O(m \* n)**: Single pass through the matrix.

### Space Complexity

- **O(1)**: Constant space usage.

### Code

```cpp
class Solution {
public:
    long long maxMatrixSum(vector<vector<int>>& matrix) {
        long long sum = 0;
        int minElement = INT_MAX;
        int negativeCount = 0;

        // Iterate through the matrix to calculate sum, minimum element, and count negatives
        for (const auto& row : matrix) {
            for (int num : row) {
                sum += abs(num);
                if (num < 0) negativeCount++;
                minElement = min(minElement, abs(num));
            }
        }

        // If the count of negatives is odd, subtract twice the smallest absolute value
        if (negativeCount % 2 != 0) {
            sum -= 2 * minElement;
        }

        return sum;
    }
};
```
