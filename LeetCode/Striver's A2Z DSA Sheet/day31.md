# 151. Subsets

### Problem Link

[LeetCode - Subsets](https://leetcode.com/problems/subsets/description/)

## Approach: Bit Manipulation

### Algorithm

1. Initialize an empty vector `allSubsets` to store all possible subsets.
2. Calculate the total number of subsets, which is \(2^{\text{nums.size()}}\).
3. Loop through all possible subset IDs (0 to `numOfSubsets - 1`).
   - For each subset ID, initialize an empty subset and a bitmask set to the subset ID.
   - For each bit in the bitmask, if it is set, add the corresponding element from `nums` to the subset.
4. Add each subset to `allSubsets`.
5. Return `allSubsets`.

### Time Complexity

- **O(n \* 2^n)**: We generate \(2^n\) subsets, and for each subset, we potentially include each of the \(n\) elements.

### Space Complexity

- **O(n \* 2^n)**: Space required to store all subsets.

### Code

```cpp
vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> allSubsets;
    int numOfSubsets = pow(2, nums.size());

    for (int subsetID = 0; subsetID < numOfSubsets; subsetID++) {
        vector<int> subset;
        int bitmask = subsetID;

        for (int i = 0; bitmask; i++, bitmask >>= 1) {
            if (bitmask & 1) {
                subset.push_back(nums[i]);
            }
        }

        allSubsets.push_back(subset);
    }

    return allSubsets;
}
```

# XOR Pattern for Numbers from 1 to N

This pattern allows you to calculate the XOR of all numbers from 1 to \( N \) without looping, which is efficient for large values of \( N \). The XOR operation has a periodic pattern that repeats every 4 numbers, which simplifies the calculation.

## Explanation of XOR Pattern

The pattern for XOR from 1 to \( N \) depends on the remainder when \( N \) is divided by 4. Let's break it down into four cases:

### Cases for XOR Pattern:

1. **When N mod 4 = 0:**

   - **Formula:** XOR(1 to N) = N
   - **Explanation:** If N is divisible by 4, the XOR of all numbers from 1 to N is simply N itself.
   - **Example:** For N = 4, XOR(1, 2, 3, 4) = 4.

2. **When N mod 4 = 1:**

   - **Formula:** XOR(1 to N) = 1
   - **Explanation:** If the remainder when N is divided by 4 is 1, the XOR result of all numbers from 1 to N will be 1.
   - **Example:** For N = 5, XOR(1, 2, 3, 4, 5) = 1.

3. **When N mod 4 = 2:**

   - **Formula:** XOR(1 to N) = N + 1
   - **Explanation:** If the remainder when N is divided by 4 is 2, the XOR result of all numbers from 1 to N will be N + 1.
   - **Example:** For N = 6, XOR(1, 2, 3, 4, 5, 6) = 7 (which is N + 1 = 6 + 1).

4. **When N mod 4 = 3:**

   - **Formula:** XOR(1 to N) = 0
   - **Explanation:** If the remainder when N is divided by 4 is 3, the XOR result of all numbers from 1 to N will be 0.
   - **Example:** For N = 7, XOR(1, 2, 3, 4, 5, 6, 7) = 0.

### Why This Pattern Occurs

The periodic nature of XOR in binary is what leads to this 4-cycle pattern. The XOR operation flips bits between 0 and 1. By observing the results for small values of \( N \) (e.g., 1, 2, 3, and 4), we can see that the result restarts in the same sequence every 4 numbers. This makes it possible to use modular arithmetic to quickly determine the XOR result.

### Practical Application

Using this pattern, you can calculate the XOR of numbers from 1 to \( N \) in constant time \( O(1) \) by simply checking \( N \mod 4 \) and applying the appropriate case. This is much faster than looping through each number and performing iterative XOR, especially for large \( N \) values.

This pattern is particularly useful in competitive programming and problems that involve bit manipulation, where calculating cumulative XOR in an optimized way is crucial.

# 152. L to R XOR

### Problem Link

[Naukri - L to R XOR](https://www.naukri.com/code360/problems/l-to-r-xor_8160412?)

## Approach 1: Brute Force XOR from L to R

### Algorithm

1. Initialize `ans` to `0`.
2. Traverse from `L` to `R`, and for each number `i`, update `ans` by performing `ans = ans ^ i`.
3. Return `ans` as the result after the loop.

### Time Complexity

- **O(R - L + 1)**: Linear in the range `[L, R]`.

### Space Complexity

- **O(1)**: Constant space usage.

### Code

```cpp
int findXOR(int L, int R) {
    int ans = 0;
    for (int i = L; i <= R; i++) {
        ans ^= i;
    }
    return ans;
}
```

## Approach 2: Pattern-Based XOR Using Properties

### Algorithm

1. Define a helper function `xorToN(N)` that calculates the XOR from `1` to `N` using specific rules based on the value of `N % 4`:
   - If `N % 4 == 0`, return `N`.
   - If `N % 4 == 1`, return `1`.
   - If `N % 4 == 2`, return `N + 1`.
   - If `N % 4 == 3`, return `0`.
2. To find the XOR from `L` to `R`, calculate `xorToN(R) ^ xorToN(L - 1)`, which gives the desired result without looping through all numbers.

### Time Complexity

- **O(1)**: The solution calculates XOR in constant time due to direct arithmetic operations based on patterns.

### Space Complexity

- **O(1)**: Constant space usage.

### Code

```cpp
int xorToN(int N) {
    if (N % 4 == 0) return N;
    if (N % 4 == 1) return 1;
    if (N % 4 == 2) return N + 1;
    return 0;
}

int findXOR(int L, int R) {
    return xorToN(R) ^ xorToN(L - 1);
}
```
