# Problem: Longest Happy String

[LeetCode Problem Link](https://leetcode.com/problems/longest-happy-string/description)

## Approach

1. **Greedy Strategy**:
   - Always try to add the character with the highest remaining count to the string, as long as it doesn’t violate the condition of having more than two consecutive occurrences of the same character.

2. **Maintaining Balance**:
   - If adding the character with the maximum frequency would result in three consecutive occurrences, switch to the next most frequent character.

3. **Using Priority Queue**:
   - A max-heap (priority queue) helps keep track of the characters with the highest counts efficiently.
   - Each heap entry stores the **remaining count** and the corresponding **character**.

4. **Adding Characters to the String**:
   - If the most frequent character can’t be added due to consecutive constraints, we temporarily use the next most frequent character from the heap.

5. **Time Complexity**:
   - **Time**: \(O(n \log 3)\) ≈ \(O(n)\), where \(n\) is the total number of characters. The heap size is fixed at 3.
   - **Space**: \(O(1)\), since the heap contains at most 3 elements.

### C++ Code Implementation

```cpp
class Solution {
public:
    string longestDiverseString(int a, int b, int c) {
        // Max-heap to store the remaining counts of characters ('a', 'b', 'c')
        priority_queue<pair<int, char>> maxHeap;
        
        if (a > 0) maxHeap.push({a, 'a'});
        if (b > 0) maxHeap.push({b, 'b'});
        if (c > 0) maxHeap.push({c, 'c'});

        string result;

        // Greedily construct the string
        while (!maxHeap.empty()) {
            auto [count1, char1] = maxHeap.top(); maxHeap.pop();

            // Try to add char1 to the result
            if (result.size() >= 2 && result.back() == char1 && result[result.size() - 2] == char1) {
                // If adding char1 creates three consecutive characters
                if (maxHeap.empty()) break; // No other characters available

                auto [count2, char2] = maxHeap.top(); maxHeap.pop();

                // Add char2 instead
                result += char2;
                if (--count2 > 0) maxHeap.push({count2, char2});

                // Push char1 back for future use
                maxHeap.push({count1, char1});
            } else {
                // Add char1 to the result
                result += char1;
                if (--count1 > 0) maxHeap.push({count1, char1});
            }
        }

        return result;
    }
};
```