# Problem: Remove Sub-Folders from the Filesystem

[LeetCode Problem Link](https://leetcode.com/problems/remove-sub-folders-from-the-filesystem/)

## Approach

1. **Sort the Folders**:

   - Sort the folder paths in lexicographical order to ensure that sub-folders come immediately after their parent folders.

2. **Traverse the Sorted List**:

   - Initialize an empty string `previous` to track the last valid folder path.
   - Iterate through each `current` folder in the sorted list:
     - If `previous` is empty or `current` does not start with `previous + "/"`, then `current` is not a sub-folder of `previous`.
     - Add `current` to the result and update `previous`.

3. **Return the Result**:
   - Return the list of folders that are not sub-folders of any other folder.

### Time Complexity

- **O(n log n)**: The sorting step takes \(O(n \log n)\) time, where \(n\) is the number of folder paths.

### Space Complexity

- **O(n)**: The space required for the result vector, which stores the final list of folder paths.

### C++ Code Implementation

```cpp
class Solution {
public:
    vector<string> removeSubfolders(vector<string>& folder) {
        // Sort folders in lexicographical order
        sort(folder.begin(), folder.end());
        vector<string> result;

        // Traverse through the sorted folder list
        string previous = "";  // Track the last valid folder path
        for (const string& current : folder) {
            // Check if current folder is a sub-folder of the previous folder
            if (previous.empty() || current.find(previous + "/") != 0) {
                result.push_back(current);  // Add non-sub-folder to result
                previous = current;         // Update the previous folder
            }
        }

        return result;
    }
};
```
