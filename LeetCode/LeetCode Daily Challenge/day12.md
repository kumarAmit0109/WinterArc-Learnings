# Problem: Divide Intervals Into Minimum Number of Groups

[LeetCode Problem Link](https://leetcode.com/problems/divide-intervals-into-minimum-number-of-groups)

## Approach: Using Line Sweep

To find the minimum number of groups needed to divide intervals, we can use a line sweep approach that helps us determine the maximum number of overlapping intervals at any point in time.

### Idea Behind the Approach

1. **Event Creation**: For each interval, create two events: one for the start of the interval and one for the end. The start event signifies that an interval has begun and increases the need for a group, while the end event signifies that an interval has ended and decreases the need for a group.

2. **Sorting Events**: Sort all events. If two events have the same time, prioritize the end event over the start event. This ensures that when an interval ends and another starts at the same time, we correctly account for overlapping intervals.

3. **Counting Overlaps**: Traverse through the sorted events and maintain a counter for the current number of overlapping intervals (active groups). Update the maximum overlap count whenever a start event is encountered.

4. **Result**: The maximum overlap count gives us the minimum number of groups required.

### Why This Works

This approach works because the maximum number of overlapping intervals at any point in time directly correlates to the minimum number of groups needed. Each overlapping interval requires a separate group, and by counting overlaps as we process the events, we efficiently determine the peak demand for groups.

### Time Complexity

- The time complexity is \(O(N \log N)\), where \(N\) is the number of intervals. This is due to the sorting step.

### Space Complexity

- The space complexity is \(O(N)\) for storing the events.

### C++ Code Implementation

```cpp
class Solution {
public:
    int minGroups(vector<vector<int>>& intervals) {
        vector<pair<int, int>> events;

        // Create events for each interval
        for (const auto& interval : intervals) {
            events.emplace_back(interval[0], 1); // Start of interval
            events.emplace_back(interval[1] + 1, -1); // End of interval (+1 to mark end)
        }

        // Sort events: first by time, then by type (end before start)
        sort(events.begin(), events.end());

        int currentGroups = 0, maxGroups = 0;

        // Process events
        for (const auto& event : events) {
            currentGroups += event.second; // Update current groups based on event type
            maxGroups = max(maxGroups, currentGroups); // Track maximum groups needed
        }

        return maxGroups; // Minimum number of groups required
    }
};
```
