# Problem: Permutation in String

[LeetCode Problem Link](https://leetcode.com/problems/permutation-in-string/description)

## Approach

We need to determine if one string (`s1`) is a permutation of another string (`s2`) or if any substring of `s2` is a permutation of `s1`. To achieve this, we can use the sliding window technique combined with character counting.

### Steps:

1. **Character Counting**:

   - Create an array `count1` of size 26 (for each lowercase letter) to store the frequency of characters in `s1`.
   - Iterate through `s1` and populate `count1`.

2. **Initial Window Setup**:

   - Create another array `count2` to count the characters in the first `windowSize` (length of `s1`) of `s2`.
   - Compare `count1` and `count2`. If they are equal, `s1` is a permutation of the first substring of `s2`.

3. **Sliding Window**:

   - Slide the window across `s2`. For each new character added to the window, increment its count in `count2`.
   - Remove the character that is sliding out of the window from `count2`.
   - After updating `count2`, check if it matches `count1`.

4. **Return Result**:
   - If at any point `count1` equals `count2`, return `true`. If the loop completes without finding a match, return `false`.

### Time Complexity

- The time complexity is \(O(n + m)\), where \(n\) is the length of `s1` and \(m\) is the length of `s2`, as we are iterating through both strings.

### Space Complexity

- The space complexity is \(O(1)\) since the size of the character count arrays is fixed at 26, irrespective of the input size.

## C++ Code Implementation

```cpp

class Solution {
public:
    bool checkEqual(int a[26], int b[26]) {
        for (int i = 0; i < 26; i++) {
            if (a[i] != b[i]) {
                return false;
            }
        }
        return true;
    }

    bool checkInclusion(string s1, string s2) {
        int count1[26] = {0};
        for (int i = 0; i < s1.length(); i++) {
            int index = s1[i] - 'a';
            count1[index]++;
        }

        int i = 0;
        int windowSize = s1.length(), size = s2.length();
        int count2[26] = {0};

        // Running for the first window
        while (i < windowSize && i < size) {
            int index = s2[i] - 'a';
            count2[index]++;
            i++;
        }

        if (checkEqual(count1, count2)) {
            return true;
        }

        // Running for other windows
        while (i < size) {
            int index = s2[i] - 'a';
            count2[index]++;

            char oldChar = s2[i - windowSize];
            index = oldChar - 'a';
            count2[index]--;
            if (checkEqual(count1, count2)) {
                return true;
            }
            i++;
        }
        return false;
    }
};
```
