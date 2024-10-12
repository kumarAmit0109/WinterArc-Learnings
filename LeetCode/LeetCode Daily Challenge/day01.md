# Problem: Check if Array Pairs are Divisible by K

[LeetCode Problem Link](https://leetcode.com/problems/check-if-array-pairs-are-divisible-by-k)

## Idea Behind the Approach

To determine if we can form pairs of integers from the array such that the sum of each pair is divisible by a given integer \( k \), we can leverage the properties of remainders.

### Key Observations

1. **Remainders**:

   - When an integer \( x \) is divided by \( k \), it yields a remainder in the range \([0, k-1]\).
   - For a pair of integers \( (a, b) \) to have their sum \( a + b \) divisible by \( k \), the following condition must hold:
     \[
     (a \% k + b \% k) \% k = 0
     \]
   - This implies that:
     - If \( a \% k = r \), then \( b \% k \) must equal \( (k - r) \% k \).

2. **Counting Frequencies**:

   - We can maintain a frequency count of each possible remainder when the integers in the array are divided by \( k \).
   - This allows us to know how many integers yield each remainder, enabling us to check if we can pair them appropriately.

3. **Special Cases**:
   - For remainder \( 0 \): All integers that are multiples of \( k \) must be paired with each other, so their count must be even.
   - For each remainder \( r \) (where \( 1 \leq r < k/2 \)), we need to ensure that the frequency of \( r \) matches the frequency of \( k - r \).
   - If \( k \) is even, the special case \( r = k/2 \) must also have an even count because those elements must pair with each other.

### Approach Summary

1. Create a frequency array of size \( k \) to count occurrences of each remainder.
2. Iterate through the array to fill the frequency array.
3. Check the conditions:
   - Ensure that the frequency of remainder \( 0 \) is even.
   - For each \( i \) from \( 1 \) to \( k/2 \), confirm that the frequency of \( i \) equals the frequency of \( k - i \).
4. If all conditions are satisfied, return `true`; otherwise, return `false`.

## C++ Code Implementation

```cpp

class Solution {
public:
    bool canArrange(vector<int>& arr, int k) {
        // Create a vector to store the frequency of remainders
        vector<int> freq(k, 0);

        // Count the frequency of each remainder
        for (int x : arr) {
            int remainder = (x % k + k) % k; // Ensure non-negative remainder
            freq[remainder]++;
        }

        // Check the conditions for valid pairs
        // Check for remainder 0
        if (freq[0] % 2 != 0) {
            return false;
        }

        // Check pairs for all other remainders
        for (int i = 1; i <= k / 2; ++i) {
            if (freq[i] != freq[k - i]) { // Check complementary pairs
                return false;
            }
        }

        return true;
    }
};
```
