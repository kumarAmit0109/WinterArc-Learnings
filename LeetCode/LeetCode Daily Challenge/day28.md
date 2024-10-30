# Longest Square Streak in an Array

**Problem Link**: [Longest Square Streak in an Array](https://leetcode.com/problems/longest-square-streak-in-an-array/)

## Approach 1:

### Approach: Hash Map and Sorting

1. **Sort the Array**:
   - Sort the `nums` array to ensure that all values are processed in increasing order, which allows each square to build upon previous values.
2. **Hash Map for Streak Length**:
   - Use a hash map (`streakLength`) to store the longest square streak ending at each number.
3. **Square Streak Calculation**:
   - For each number `val` in `nums`, calculate its integer square root `root`.
   - If `val` is a perfect square (i.e., `root * root == val`) and `root` exists in the map, then:
     - Set `streakLength[val]` to `streakLength[root] + 1`.
   - Otherwise, initialize `streakLength[val]` to 1.
4. **Track Maximum Streak**:
   - Update `maxStreak` with the maximum streak length recorded in `streakLength`.
5. **Return Result**:
   - If `maxStreak` is less than 2, return -1; otherwise, return `maxStreak`.

### Time Complexity

- **O(n log n)**: Sorting the array takes \(O(n \log n)\), and iterating through the sorted array takes \(O(n)\).

### Space Complexity

- **O(n)**: Space used by the hash map `streakLength`.

### Code

```cpp
class Solution {
public:
    int longestSquareStreak(vector<int>& nums) {
        unordered_map<int, int> streakLength;
        sort(nums.begin(), nums.end());

        int maxStreak = 0;

        for (int& val : nums) {
            int root = static_cast<int>(sqrt(val)); // Calculate the integer square root

            // Check if val is a perfect square and if its root exists in the map
            if (root * root == val && streakLength.find(root) != streakLength.end()) {
                streakLength[val] = streakLength[root] + 1;
            } else {
                streakLength[val] = 1;
            }

            // Update the maximum streak length
            maxStreak = max(maxStreak, streakLength[val]);
        }

        // Return the result based on the max streak length found
        return maxStreak < 2 ? -1 : maxStreak;
    }
};
```

## Approach 2:

1. **Use a Set for Unique Numbers**: Store the numbers from `numList` in an unordered set for quick access.
2. **Iterate through the List**: For each number in `numList`, initialize a streak counter and set the current value to that number.
3. **Count Streaks**:
   - While the squared value of `currentVal` exists in the set, increment the streak counter.
   - If the square of `currentVal` exceeds \(10^5\), break out of the loop to avoid unnecessary computations.
   - Update `currentVal` to its square.
4. **Update Maximum Streak**: After processing each number, update the maximum streak if the current one is larger.
5. **Return Result**: If the maximum streak is less than 2, return -1; otherwise, return the maximum streak.

### Time Complexity

- **O(n \* k)**, where \(n\) is the number of elements in `numList`, and \(k\) is the maximum length of streaks formed (limited by squaring).

### Space Complexity

- **O(n)**, for storing unique numbers in the set.

### Code Implementation

```cpp
class Solution {
public:
    int longestSquareStreak(vector<int>& numList) {
        unordered_set<int> uniqueNums(numList.begin(), numList.end());

        int maxStreak = 0;

        for(int& num : numList) {
            int streakCount = 0;
            long long currentVal = num;

            while(uniqueNums.find(currentVal) != uniqueNums.end()) {
                streakCount++;

                if(currentVal * currentVal > 1e5) {
                    break;
                }

                currentVal = currentVal * currentVal; // square the current value
            }

            maxStreak = max(maxStreak, streakCount);
        }

        return maxStreak < 2 ? -1 : maxStreak;
    }
};
```

## Significance of Condition in Longest Square Streak Algorithm

### Overview

In the `longestSquareStreak` function, the condition `if(currentVal * currentVal > 1e5)` plays a critical role in ensuring the algorithm's robustness and efficiency. This document outlines the significance of this condition and its relation to the constraints of the problem.

### Significance of the Condition

1. **Preventing Overflow**:

   - The condition safeguards against integer overflow by limiting the calculations of squares. If `currentVal` becomes too large, squaring it could exceed the bounds of typical integer types, leading to incorrect results or undefined behavior.

2. **Limiting Computation**:

   - The problem constraints may imply a maximum length for the longest square streak. By breaking the loop when the square exceeds \(10^5\), we avoid unnecessary calculations that do not contribute to finding the valid streak.

3. **Performance Optimization**:

   - This check helps to limit the number of iterations, allowing the algorithm to run more efficiently. Large squares are unlikely to be present in the set of unique numbers, making further checks redundant.

4. **Ensuring Valid Results**:
   - Given that input values are likely bounded (e.g., from 1 to 100), squaring values beyond \(10^5\) is not meaningful. This condition ensures that only valid numbers are considered in the streak.

### Relation to Problem Constraints

- The maximum length of the longest square streak is influenced by the input constraints. For example, if the numbers in `numList` are limited to a range where squaring can lead to a maximum of 5 valid streaks, this condition helps efficiently identify when to stop further calculations.

### Best Case Complexity

- In the best-case scenario, if the input numbers allow for quick detection of valid streaks, the time complexity is significantly reduced. Even though the theoretical complexity is \(O(n \times k)\), where \(k\) is the maximum streak length, the practical implications of the condition help in bounding computational efforts.

### Conclusion

The condition `if(currentVal * currentVal > 1e5)` is essential for optimizing the performance of the longest square streak algorithm. It ensures that the calculations remain valid, efficient, and constrained within the problem's limits.
