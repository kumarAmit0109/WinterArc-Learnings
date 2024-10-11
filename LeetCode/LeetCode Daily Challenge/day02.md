# Problem: Rank Transform of an Array

[LeetCode Problem Link](https://leetcode.com/problems/rank-transform-of-an-array/description/)

## Approach

To transform an array into its rank representation, we can utilize an ordered map. Here’s a detailed explanation of the approach:

1. **Ordered Map Creation**:
   - Create an ordered map to store unique elements of the array along with their indices. The ordered nature of the map ensures that the elements are sorted.
2. **Rank Count Initialization**:

   - Initialize a rank counter, starting from zero.

3. **Traverse the Map**:

   - Iterate through the ordered map and assign a rank to each unique element.
   - In each iteration, update the rank count and store the current rank in the map.

4. **Generate Result Vector**:

   - Create an answer vector of the same size as the input array.
   - Traverse the original array and replace each element with its corresponding rank from the map.

5. **Return the Result**:
   - Finally, return the answer vector containing the rank representation of the original array.

## Time Complexity

- The time complexity of this approach is \( O(nlog n) \), where \( n \) is the number of elements in the input array. This is due to the insertion of elements into the ordered map, which takes \( O(log n) \) time for each insertion, and we do this for all \( n \) elements.

## Space Complexity

- The space complexity is \( O(n) \) because we use an ordered map to store up to \( n \) unique elements and an additional array of size \( n \) for the result.

## C++ Code Implementation

```cpp
class Solution {
public:
    vector<int> arrayRankTransform(vector<int>& arr) {
        // Create an ordered map to store the element with its index
        map<int, int> rankMap;

        // Fill the map with unique elements from arr
        for (int num : arr) {
            rankMap[num] = 0; // Initialize the rank as 0
        }

        // Traverse the map to assign ranks
        int rankCount = 1; // Start ranking from 1
        for (auto& pair : rankMap) {
            pair.second = rankCount; // Assign the rank
            rankCount++; // Increment the rank for the next element
        }

        // Create the answer vector and fill it with ranks based on original arr
        vector<int> ans(arr.size());
        for (int i = 0; i < arr.size(); ++i) {
            ans[i] = rankMap[arr[i]]; // Get the rank from the map
        }

        return ans; // Return the final ranks
    }
};
```
