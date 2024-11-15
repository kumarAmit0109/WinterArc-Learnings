# Minimum Array End

**Problem Link**: [Minimum Array End](https://leetcode.com/problems/minimum-array-end/description/)

## Approach: Bitwise OR Operation

### Idea

1. Start with `ans = x` (initialize with the given number `x`).
2. Iterate from 1 to `n-1` to simulate the operation for `n` array elements.
3. In each iteration:
   - Increment `ans` by 1.
   - Apply the bitwise OR operation with `x`.
4. Return the final value of `ans`.

### Algorithm

1. Initialize `ans` as `x`.
2. Loop from `1` to `n-1`:
   - Update `ans = (ans + 1) | x`.
3. Return `ans`.

### Time Complexity

- **O(n)**: The loop iterates `n-1` times.

### Space Complexity

- **O(1)**: No additional space is used.

### Code Implementation

```cpp
class Solution {
public:
    long long minEnd(int n, int x) {
        long long ans = x;
        for (int i = 1; i < n; i++) {
            ans = (ans + 1) | x;
        }
        return ans;
    }
};
```
