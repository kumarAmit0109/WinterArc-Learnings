# Problem: Number of the Smallest Unoccupied Chair

[LeetCode Problem Link](https://leetcode.com/problems/the-number-of-the-smallest-unoccupied-chair/description)

## Approach 1: Using Sorting and Heaps

To determine which chair the target friend will sit on, we can follow these steps:

1. **Sort Arrival Times**:

   - First, we create a list of events that include the arrival and leaving times of each friend. Each event will be represented as a tuple of the form (arrival time, leaving time, index of the friend).
   - After constructing this list, we sort it based on the arrival times. This ensures that we process friends in the order they arrive.

2. **Use Heaps to Manage Chairs**:

   - We utilize a **min-heap** (priority queue) to keep track of available chairs. Initially, all chairs from `0` to `n-1` are considered available, so we push all these chair indices into the heap.
   - We also use another min-heap to track when chairs become free based on the leaving times of friends. Each entry in this heap will be a pair of (leaving time, chair number).

3. **Simulate the Arrival of Friends**:
   - For each friend arriving:
     - Before assigning a chair, we check the occupied chairs heap to free up chairs for friends who have already left. This is done by comparing the current arrival time with the leaving times of the friends in the occupied chairs heap. If a chair's leaving time is less than or equal to the current arrival time, it means the chair is now available, and we can add it back to the free chairs heap.
     - After freeing up chairs, we assign the smallest available chair (the top of the free chairs heap) to the arriving friend.
     - If the friend is the target friend, we return the chair number immediately.
     - Lastly, we mark this chair as occupied until the friend leaves by pushing the leaving time and chair number onto the occupied chairs heap.

### Time Complexity

- The time complexity is \(O(N \log N)\) due to the sorting of arrival times and the operations with heaps (insertion and removal from heaps).

### Space Complexity

- The space complexity is \(O(N)\) for storing the arrival times and managing the states of the chairs.

### C++ Code Implementation

```cpp
class Solution {
public:
    int smallestChair(vector<vector<int>>& times, int targetFriend) {
        int n = times.size();

        // Create a vector of (arrival, leaving, index) for each friend.
        vector<tuple<int, int, int>> events;
        for (int i = 0; i < n; ++i) {
            events.push_back({times[i][0], times[i][1], i});
        }

        // Sort events by arrival time.
        sort(events.begin(), events.end());

        // Min-heap to store free chairs.
        priority_queue<int, vector<int>, greater<int>> freeChairs;
        for (int i = 0; i < n; ++i) {
            freeChairs.push(i);
        }

        // Min-heap to track when chairs become free (leave_time, chair_number).
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> occupiedChairs;

        for (auto [arrival, leaving, idx] : events) {
            // Free up chairs for friends who have already left.
            while (!occupiedChairs.empty() && occupiedChairs.top().first <= arrival) {
                freeChairs.push(occupiedChairs.top().second);
                occupiedChairs.pop();
            }

            // Assign the smallest free chair.
            int assignedChair = freeChairs.top();
            freeChairs.pop();

            // If this is the target friend, return the chair number.
            if (idx == targetFriend) {
                return assignedChair;
            }

            // Mark this chair as occupied until the friend's leaving time.
            occupiedChairs.push({leaving, assignedChair});
        }

        return -1; // This should never be reached.
    }
};
```
