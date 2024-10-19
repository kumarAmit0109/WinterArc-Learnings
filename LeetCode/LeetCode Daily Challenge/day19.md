# Find Kth Bit in Nth Binary String

[LeetCode Problem Link](https://leetcode.com/problems/find-kth-bit-in-nth-binary-string/)

## Approach 1: Recursive Construction + Linear Traversal

### Idea

1. Construct the binary string `Sn` recursively:
   - `S1 = "0"`
   - `Sn = Sn-1 + "1" + reverse(invert(Sn-1))`
2. After generating the full string `Sn`, find the k-th bit using linear traversal.

### Steps

1. Use recursion to build the string `Sn` by combining the previous string and its reverse-inverted version.
2. Once `Sn` is constructed, return the k-th bit using direct indexing.

### Time Complexity

- **O(2^n)**: Exponential time due to the recursive construction of the binary string.

### Space Complexity

- **O(2^n)**: The space required to store the binary string `Sn`.

### Code

```cpp
class Solution {
public:
    char findKthBit(int n, int k) {
        // Step 1: Construct Sn recursively
        string Sn = generateSn(n);

        // Step 2: Return the k-th bit (1-based index)
        return Sn[k - 1];
    }

private:
    // Helper function to generate Sn recursively
    string generateSn(int n) {
        if (n == 1) return "0";  // Base case: S1 is "0"

        string prev = generateSn(n - 1);  // Recursively generate Sn-1
        string inverted = invertAndReverse(prev);  // Invert and reverse Sn-1
        return prev + "1" + inverted;  // Construct Sn
    }

    // Helper function to invert and reverse a binary string
    string invertAndReverse(const string& s) {
        string result = s;
        for (char& c : result) {
            c = (c == '0') ? '1' : '0';  // Invert each bit
        }
        reverse(result.begin(), result.end());  // Reverse the string
        return result;
    }
};
```

## Approach 2: Optimal Recursive Approach (Without Full String Construction)

### Idea

1. Instead of constructing the entire string `Sn`, use **recursive logic** to determine the k-th bit directly.
2. The middle element of `Sn` is always `1`.
3. If `k` is in the first half, the k-th bit corresponds to the same position in `Sn-1`.
4. If `k` is in the second half, the k-th bit corresponds to the **inverted bit** from `Sn-1` at the mirrored position.

### Steps

1. If `k` is equal to the middle index, return `1`.
2. If `k` is in the first half, recursively find the k-th bit in `Sn-1`.
3. If `k` is in the second half, find the mirrored bit in `Sn-1` and invert it.

### Time Complexity

- **O(log N)**: Each recursive call reduces the problem size by half.

### Space Complexity

- **O(log N)**: Due to recursive stack calls.

### Code

```cpp
class Solution {
public:
    char findKthBit(int n, int k) {
        return findKth(n, k);  // Start recursive search
    }

private:
    // Helper function to determine the k-th bit recursively
    char findKth(int n, int k) {
        if (n == 1) return '0';  // Base case: S1 is "0"

        int mid = (1 << (n - 1));  // Middle index of Sn (2^(n-1))

        if (k == mid) return '1';  // If k is the middle element, return '1'
        else if (k < mid) return findKth(n - 1, k);  // Search in the first half
        else {
            // Search in the second half, invert the mirrored bit from Sn-1
            char bit = findKth(n - 1, mid - (k - mid));
            return bit == '0' ? '1' : '0';  // Invert the bit
        }
    }
};
```
