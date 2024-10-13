# Problem: Smallest Range Covering Elements from K Lists

[LeetCode Problem Link](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/description)

## Approach: Using Min-Heap to Track the Smallest Range

### Idea Behind the Approach

1. **Using a Min-Heap**:

   - We use a min-heap (priority queue) to keep track of the smallest element across the lists. Each element in the heap is stored along with its list and index to know where it came from.

2. **Tracking the Range**:

   - Start by inserting the first element from each list into the min-heap.
   - As we extract the minimum element from the heap (`minElement`), we compare the range `[minElement, maxElement]` with the best range found so far (`ansStart, ansEnd`). If it is smaller, update the answer.

3. **Expanding the Range**:

   - After popping the smallest element, insert the next element from the same list to maintain continuity.
   - Update the `maxElement` to reflect the new maximum in the heap.

4. **Termination Condition**:
   - The loop terminates when we run out of elements in any list. This is because a valid range requires at least one element from each list.

### Why This Works

This approach works by expanding the range incrementally, always trying to keep the range as tight as possible. Since we always push the next element from the list of the popped element, the heap allows us to maintain the smallest available element at every step. This ensures that the solution covers elements from all lists with the smallest possible range.

### Time Complexity

- **Time Complexity**: \(O(N log K)\), where \(N\) is the total number of elements and \(K\) is the number of lists.
- **Space Complexity**: \(O(K)\) for storing elements in the heap.

### C++ Code Implementation

```cpp
class Solution {
public:
    vector<int> smallestRange(vector<vector<int>>& nums) {
        using pii = pair<int, pair<int, int>>; // {value, {listIndex, elementIndex}}
        priority_queue<pii, vector<pii>, greater<pii>> minHeap;

        int maxElement = INT_MIN;
        int ansStart = 0, ansEnd = INT_MAX;

        // Insert the first element from each list into the min-heap
        for (int i = 0; i < nums.size(); ++i) {
            minHeap.push({nums[i][0], {i, 0}});
            maxElement = max(maxElement, nums[i][0]);
        }

        // Process elements until we run out of elements in any list
        while (!minHeap.empty()) {
            auto [minElement, indices] = minHeap.top();
            minHeap.pop();

            int listIndex = indices.first;
            int elementIndex = indices.second;

            // Update the answer if we found a smaller range
            if (maxElement - minElement < ansEnd - ansStart) {
                ansStart = minElement;
                ansEnd = maxElement;
            }

            // If we have more elements in the current list, insert the next one
            if (elementIndex + 1 < nums[listIndex].size()) {
                int nextElement = nums[listIndex][elementIndex + 1];
                minHeap.push({nextElement, {listIndex, elementIndex + 1}});
                maxElement = max(maxElement, nextElement);
            } else {
                // If any list is exhausted, we cannot form a valid range anymore
                break;
            }
        }

        return {ansStart, ansEnd};
    }
};
```
