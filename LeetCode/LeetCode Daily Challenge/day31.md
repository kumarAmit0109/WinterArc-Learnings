# Minimum Total Distance Traveled

**Problem Link**: [Minimum Total Distance Traveled](https://leetcode.com/problems/minimum-total-distance-traveled/description)

## Approach 1: Recursive Solution without Memoization

### Approach

1. **Sorting**:

   - Sort both the robots and factories based on their positions to facilitate efficient matching.

2. **Flatten Factory Positions**:

   - Expand each factory's position by its repair limit, creating a list of positions that represent each repair slot.

3. **Recursive Function**:

   - Define a recursive function `computeMinimumDistance` which attempts to assign each robot to a factory position while keeping track of the minimum total distance.
   - Use two base cases:
     - **Robot assignment complete**: Return `0` since no additional distance is incurred.
     - **Factories exhausted**: Return `LLONG_MAX` as no valid assignment can be made.
   - Consider two options for each robot:
     - Assign the robot to the current factory and compute the distance.
     - Skip the current factory and proceed to the next factory.

4. **Calculate the Minimum Distance**:
   - Return the minimum distance found between the two options.

### Time Complexity

- **O(2^n)**, due to the recursive calls without memoization.

### Space Complexity

- **O(n)** for recursion stack depth in the worst case.

### Code Implementation

```cpp
class Solution {
public:
    long long computeMinimumDistance(int robotIndex, int factoryIndex, const vector<int>& robots, const vector<int>& expandedPositions) {
        // Base case: All robots are assigned
        if (robotIndex >= robots.size()) {
            return 0;
        }

        // Base case: No more factories to assign
        if (factoryIndex >= expandedPositions.size()) {
            return LLONG_MAX;
        }

        // Option 1: Assign the current robot to the current factory
        long long distance = abs(robots[robotIndex] - expandedPositions[factoryIndex]);
        long long assignCurrent = LLONG_MAX;

        // Check for overflow before addition
        if (distance <= LLONG_MAX - computeMinimumDistance(robotIndex + 1, factoryIndex + 1, robots, expandedPositions)) {
            assignCurrent = distance + computeMinimumDistance(robotIndex + 1, factoryIndex + 1, robots, expandedPositions);
        }

        // Option 2: Skip the current factory and try the next one
        long long skipCurrent = computeMinimumDistance(robotIndex, factoryIndex + 1, robots, expandedPositions);

        // Return the minimum distance between both options
        return min(assignCurrent, skipCurrent);
    }

    long long minimumTotalDistance(vector<int>& robot, vector<vector<int>>& factory) {
        // Sort the robots and factories by position for efficient matching
        sort(robot.begin(), robot.end());
        sort(factory.begin(), factory.end());

        // Flatten factory positions based on repair limit
        vector<int> expandedPositions;
        for (const auto& fac : factory) {
            int factoryPos = fac[0];
            int factoryLimit = fac[1];
            expandedPositions.insert(expandedPositions.end(), factoryLimit, factoryPos);
        }

        // Start recursive calculation with initial robot and factory indices
        return computeMinimumDistance(0, 0, robot, expandedPositions);
    }
};
```

## Approach 2: Recursive Solution with Memoization (Dynamic Programming)

### Approach

1. **Sorting**:

   - Sort the `robots` and `factory` lists based on their positions for efficient matching.

2. **Flatten Factory Positions**:

   - Expand each factory’s position according to its repair limit to create an `expandedPositions` list. This allows each repair slot at each factory position to be accessed individually in the recursion.

3. **Recursive Function with Memoization**:
   - Define a recursive function `computeMinimumDistance` to attempt assignment of each robot to each factory position and calculate the minimum total distance.
   - Use two base cases:
     - **All robots assigned**: Return `0` as no additional distance is incurred.
     - **Factories exhausted**: Return `LLONG_MAX` since no further assignments are possible.
   - Use a 2D DP array `dp[robotIndex][factoryIndex]` to store computed minimum distances, avoiding redundant calculations.
   - For each robot, consider two options:
     - Assign it to the current factory position and compute the resulting distance.
     - Skip the current factory position and try the next one.
   - Store and reuse computed values from `dp` to optimize calculations.

### Time Complexity

- **O(n \* m)**, where `n` is the number of robots and `m` is the number of expanded factory positions.

### Space Complexity

- **O(n \* m)** for the memoization table.

### Code Implementation

```cpp
class Solution {
public:
    vector<vector<long long>> dp;

    long long computeMinimumDistance(int robotIndex, int factoryIndex, const vector<int>& robots, const vector<int>& expandedPositions) {
        // Base case: All robots are assigned
        if (robotIndex >= robots.size()) {
            return 0;
        }

        // Base case: No more factories to assign
        if (factoryIndex >= expandedPositions.size()) {
            return LLONG_MAX;
        }

        // Check if result is already computed
        if (dp[robotIndex][factoryIndex] != -1) {
            return dp[robotIndex][factoryIndex];
        }

        // Option 1: Assign the current robot to the current factory
        long long distance = abs(robots[robotIndex] - expandedPositions[factoryIndex]);
        long long assignCurrent = LLONG_MAX;

        long long nextAssignDistance = computeMinimumDistance(robotIndex + 1, factoryIndex + 1, robots, expandedPositions);
        if (nextAssignDistance != LLONG_MAX) {
            assignCurrent = distance + nextAssignDistance;
        }

        // Option 2: Skip the current factory and try the next one
        long long skipCurrent = computeMinimumDistance(robotIndex, factoryIndex + 1, robots, expandedPositions);

        // Store and return the minimum distance between both options
        return dp[robotIndex][factoryIndex] = min(assignCurrent, skipCurrent);
    }

    long long minimumTotalDistance(vector<int>& robot, vector<vector<int>>& factory) {
        // Sort robots and factories by position for efficient matching
        sort(robot.begin(), robot.end());
        sort(factory.begin(), factory.end());

        // Flatten factory positions based on repair limit
        vector<int> expandedPositions;
        for (const auto& fac : factory) {
            int factoryPos = fac[0];
            int factoryLimit = fac[1];
            expandedPositions.insert(expandedPositions.end(), factoryLimit, factoryPos);
        }

        // Initialize dp table with -1 to signify uncomputed results
        dp.assign(robot.size(), vector<long long>(expandedPositions.size(), -1));

        // Start the recursive calculation with memoization
        return computeMinimumDistance(0, 0, robot, expandedPositions);
    }
};
```
