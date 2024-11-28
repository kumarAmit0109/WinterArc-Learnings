# Defuse the Bomb

**Problem Link**: [Defuse the Bomb](https://leetcode.com/problems/defuse-the-bomb/)

## Approach: Circular Array Traversal

### Idea

1. Depending on the value of `k`, compute the sum of `k` consecutive elements:
   - For `k > 0`, sum the next `k` elements in a circular manner.
   - For `k < 0`, sum the previous `|k|` elements in a circular manner.
   - For `k == 0`, set all elements in the result to `0`.
2. Use modular arithmetic to handle the circular traversal of the array.

### Algorithm

1. Initialize the `result` array with zeros.
2. If `k == 0`, return the `result` array.
3. Traverse the input array:
   - For each index `i`, calculate the sum of the required elements using modular arithmetic.
   - Update the `result` array at index `i` with the computed sum.
4. Return the `result` array.

### Time Complexity

- **O(n \* |k|)**: For each of the `n` elements, compute the sum of `|k|` elements.

### Space Complexity

- **O(n)**: Space for the `result` array.

### Code

```cpp
class Solution {
public:
    vector<int> decrypt(vector<int>& code, int k) {
        int n = code.size();
        vector<int> result(n, 0);

        if (k == 0) {
            return result; // All elements replaced with 0 when k == 0
        }

        for (int i = 0; i < n; ++i) {
            int sum = 0;
            if (k > 0) {
                // Sum of next k elements
                for (int j = 1; j <= k; ++j) {
                    sum += code[(i + j) % n];
                }
            } else {
                // Sum of previous k elements
                for (int j = 1; j <= -k; ++j) {
                    sum += code[(i - j + n) % n];
                }
            }
            result[i] = sum;
        }

        return result;
    }
};
```
