# Problem: Divide Players Into Teams of Equal Skill

[LeetCode Problem Link](https://leetcode.com/problems/divide-players-into-teams-of-equal-skill)

## Approach

We are given an array `skill`, where we need to divide players into teams of two such that the total skill of each team is the same. The chemistry of a team is the product of the two players' skills, and we need to return the sum of the chemistry of all teams.

### Steps:

1. **Sort the Array**:

   - First, sort the `skill` array to make it easier to pair the weakest player with the strongest player.

2. **Use Two Pointers**:

   - We use a two-pointer approach. One pointer starts at the beginning (smallest skill), and the other at the end (largest skill). We pair these two players together.

3. **Check Skill Consistency**:

   - For each pair, we check if their total skill matches the target sum (the sum of the first and last player's skills). If all pairs have the same total skill, we compute their chemistry (product of their skills).

4. **Return Result**:
   - If all pairs meet the required conditions, return the total chemistry of all teams. Otherwise, return `-1` if it's not possible to divide the players into valid teams.

### Time Complexity

- **Sorting** the array takes \(O(n \log n)\), where \(n\) is the length of the `skill` array.
- **Two-pointer loop** runs in \(O(n)\), so the overall time complexity is \(O(n \log n)\).

### Space Complexity

- The space complexity is \(O(1)\), aside from the input array.

## C++ Code Implementation

```cpp

class Solution {
public:
    long long dividePlayers(vector<int>& skill) {
        sort(skill.begin(), skill.end());  // Step 1: Sort the array

        int n = skill.size();
        int targetSum = skill[0] + skill[n - 1];  // Step 2: Set the target sum
        long long totalChemistry = 0;

        // Step 3: Use two-pointer technique to pair players
        for (int i = 0; i < n / 2; ++i) {
            int currentSum = skill[i] + skill[n - 1 - i];  // Pair the weakest with the strongest

            if (currentSum != targetSum) {
                return -1;  // If any pair's sum doesn't match the target sum, return -1
            }

            // Calculate the chemistry (product of skills) and add to total chemistry
            totalChemistry += (long long)skill[i] * skill[n - 1 - i];
        }

        return totalChemistry;  // Return the total chemistry
    }
};
```
