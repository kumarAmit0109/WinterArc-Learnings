# Minimized Maximum of Products Distributed to Any Store

**Problem Link**: [Minimized Maximum of Products Distributed to Any Store](https://leetcode.com/problems/minimized-maximum-of-products-distributed-to-any-store)

## Approach 1: Brute-Force Checking for All Max Products

### Idea

1. **Key Insight**: The goal is to minimize the maximum number of products any store receives. To achieve this, we attempt to assign quantities in such a way that no store exceeds a specified maximum number of products.
2. **Brute-Force Method**:
   - Try all possible values for the maximum products per store, ranging from 1 to the maximum element in `quantities`.
   - For each value of \( \text{maxProducts} \), calculate how many stores are required to distribute all products such that no store gets more than \( \text{maxProducts} \).
   - If we find a valid distribution, return the smallest \( \text{maxProducts} \).

### Algorithm

1. **Iterate Over All Possible Max Products**:
   - For each possible value of \( \text{maxProducts} \) from 1 to the maximum quantity, check if it is possible to distribute the quantities such that no store receives more than \( \text{maxProducts} \).
2. **Helper Function**:
   - Calculate how many stores are needed for the given \( \text{maxProducts} \) and check if it exceeds \( n \).

### Time Complexity

- **O(m \cdot k)**:
  - We iterate over all possible \( \text{maxProducts} \) (from 1 to \( k \), where \( k \) is the maximum element in `quantities`).
  - For each \( \text{maxProducts} \), we loop through the `quantities` array to calculate the number of stores.

### Space Complexity

- **O(1)**: No extra space is used.

### Code Implementation

```cpp
class Solution {
public:
    bool canDistribute(vector<int>& quantities, int n, int maxProducts) {
        int storesNeeded = 0;
        for (int quantity : quantities) {
            storesNeeded += (quantity + maxProducts - 1) / maxProducts; // Divide and round up
            if (storesNeeded > n) return false; // More stores than available
        }
        return true;
    }

    int minimizedMaximum(int n, vector<int>& quantities) {
        int maxQuantity = *max_element(quantities.begin(), quantities.end());

        // Check all possible maxProducts values from 1 to maxQuantity
        for (int maxProducts = 1; maxProducts <= maxQuantity; ++maxProducts) {
            if (canDistribute(quantities, n, maxProducts)) {
                return maxProducts; // First valid maxProducts found
            }
        }

        return maxQuantity; // In case no valid maxProducts found, return maxQuantity
    }
};
```

## Approach 2: Binary Search on Maximum Products per Store

### Idea

1. **Key Insight**: The problem is to minimize the maximum number of products any store receives. This is equivalent to finding the smallest \( \text{maxProducts} \) such that all quantities can be distributed across \( n \) stores.
2. **Binary Search**:
   - Use binary search on \( \text{maxProducts} \), ranging from 1 to \( \max(\text{quantities}) \).
   - For a mid value, check if it is possible to distribute the quantities with \( \text{maxProducts} \) using a helper function.
3. **Helper Function**:
   - Simulate the distribution of products. Calculate the number of stores required for a given \( \text{maxProducts} \).
   - If the number of stores exceeds \( n \), the distribution is invalid.

### Algorithm

1. **Binary Search**:
   - Initialize \( \text{left} = 1 \) and \( \text{right} = \max(\text{quantities}) \).
   - Perform binary search:
     - Calculate \( \text{mid} \) as the potential \( \text{maxProducts} \).
     - Use the helper function to check feasibility.
     - If feasible, update \( \text{result} \) and move to a smaller range.
     - Otherwise, increase \( \text{left} \).
2. **Helper Function**:
   - For each quantity in `quantities`, compute the number of stores required as \( \lceil \frac{\text{quantity}}{\text{maxProducts}} \rceil \).
   - Sum the stores and check if it exceeds \( n \).

### Time Complexity

- **O(m \cdot \log k)**:
  - Binary search on \( \text{maxProducts} \) range (**O(\log k)**, where \( k \) is \( \max(\text{quantities}) \)).
  - Helper function iterates through \( \text{quantities} \) (**O(m)**, where \( m \) is the size of `quantities`).

### Space Complexity

- **O(1)**: No extra space is used.

### Code Implementation

```cpp
class Solution {
public:
    bool canDistribute(vector<int>& quantities, int n, int maxProducts) {
        int storesNeeded = 0;
        for (int quantity : quantities) {
            // Calculate the number of stores needed for the current quantity
            storesNeeded += (quantity + maxProducts - 1) / maxProducts;
            if (storesNeeded > n) return false; // More stores than available
        }
        return true;
    }

    int minimizedMaximum(int n, vector<int>& quantities) {
        int left = 1, right = *max_element(quantities.begin(), quantities.end());
        int result = right;

        // Binary search to find the minimum possible maxProducts
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (canDistribute(quantities, n, mid)) {
                result = mid; // Successful distribution, try a smaller max
                right = mid - 1;
            } else {
                left = mid + 1; // Unsuccessful, increase max
            }
        }

        return result;
    }
};
```
