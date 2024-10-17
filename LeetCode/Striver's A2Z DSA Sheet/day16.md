# 76. Rotate String

### Problem Link

[Rotate String - LeetCode](https://leetcode.com/problems/rotate-string/description/)

## Approach 1: Shift Characters One-by-One

### Idea

1. Use a **loop to shift characters** of string `s` to the left, one position at a time.
2. After each shift, **check if the modified string matches the `goal`**.
3. If a match is found, **return true**; otherwise, **return false** after all shifts.

### Time Complexity

- **O(n²)** in the worst case, where `n` is the length of `s`.  
  Each shift takes `O(n)` time, and we perform this shift `n` times.

### Space Complexity

- **O(1)**, no extra space used apart from the input.

### Code

```cpp
void shift(string &s) {
    int n = s.length();
    char firstChar = s[0];
    for (int i = 1; i < n; i++) {
        s[i - 1] = s[i];
    }
    s[n - 1] = firstChar;
}

bool rotateString(string s, string goal) {
    string str = s;
    for (int i = 0; i < s.length(); i++) {
        shift(str);
        if (str == goal) {
            return true;
        }
    }
    return false;
}
```

## Approach 2: Circular Comparison Using Matching Indices

### Idea

1. **Store all indices** where the first character of `goal` matches any character in `s`.
2. For each matching index, **compare `s` and `goal` in a circular manner**.
3. If all characters match for any such index, **return true**.
4. If no match is found after checking all possible circular shifts, **return false**.

### Time Complexity

- **O(n²)** in the worst case, where `n` is the length of `s`.  
  We compare each circular shift with `goal`, which takes `O(n)` time.

### Space Complexity

- **O(n)**, for storing the matching indices.

### Code

```cpp
bool rotateString(string s, string goal) {
    int m = s.length(), n = goal.length();
    if (m != n) return false;

    vector<int> presentAt;
    for (int i = 0; i < m; i++) {
        if (goal[0] == s[i]) {
            presentAt.push_back(i);
        }
    }

    if (presentAt.empty()) return false;

    for (int idx : presentAt) {
        int j = 0, k = idx;
        while (j < n && goal[j] == s[k]) {
            j++;
            k = (k + 1) % m;
        }
        if (j == n) return true;
    }

    return false;
}
```

## Approach 3: String Concatenation and Substring Search

### Idea

1. **Concatenate** the original string `s` with itself.
2. **Check if the target `goal`** is a substring of this concatenated string.
3. **Return true** if found, otherwise **return false**.

### Time Complexity

- **O(n)**, where `n` is the length of `s`.

### Space Complexity

- **O(n)**, for the concatenated string.

### Code

```cpp
bool rotateString(string s, string goal) {
    if (s.length() != goal.length()) return false;
    return (s + s).find(goal) != string::npos;
}
```

# 77. Valid Anagram

### Problem Link

[Valid Anagram - LeetCode](https://leetcode.com/problems/valid-anagram/)

## Approach 1: Sorting Both Strings

### Idea

1. **Sort both strings** `s` and `t`.
2. **Compare each character** of the sorted strings.
3. If all characters match, **return true**. Otherwise, **return false**.

### Time Complexity

- **O(n log n)** due to the sorting operation, where `n` is the length of the strings.

### Space Complexity

- **O(1)** if sorting is done in-place, otherwise **O(n)**.

### Code

```cpp
bool isAnagram(string s, string t) {
    if (s.length() != t.length()) return false;
    sort(s.begin(), s.end());
    sort(t.begin(), t.end());
    return s == t;
}
```

## Approach 2: Frequency Count

### Idea

1. Create two **frequency maps (arrays)** of size 26 to store the frequency of characters in both `s` and `t`.
2. **Fill the frequency maps** by iterating over both strings.
3. Compare both frequency maps; if they are identical, **return true**. Otherwise, **return false**.

### Time Complexity

- **O(n)** where `n` is the length of the strings.

### Space Complexity

- **O(1)** as the size of the frequency maps is fixed (26).

### Code

```cpp
bool isAnagram(string s, string t) {
    if (s.length() != t.length()) return false;

    vector<int> m1(26, 0), m2(26, 0);

    for (char c : s) {
        m1[c - 'a']++;
    }
    for (char c : t) {
        m2[c - 'a']++;
    }

    for (int i = 0; i < 26; i++) {
        if (m1[i] != m2[i]) {
            return false;
        }
    }
    return true;
}
```

# 78. Sort Characters By Frequency

### Problem Link

[Sort Characters By Frequency - LeetCode](https://leetcode.com/problems/sort-characters-by-frequency/)

## Approach: Max-Heap (Priority Queue)

### Idea

1. **Count the frequency** of each character in the string using a hashmap (`map<char, int>`).
2. **Push all characters** with their frequencies into a **max-heap** (priority queue) to sort them by frequency in descending order.
3. **Build the result string** by extracting elements from the max-heap and appending the characters according to their frequency.

### Time Complexity

- **O(n log k)** where `n` is the length of the string and `k` is the number of unique characters.

### Space Complexity

- **O(n + k)**: `n` for the result string and `k` for the hashmap and priority queue.

### Code

```cpp
string frequencySort(string s) {
    // Step 1: Count frequency of each character
    map<char, int> freq;
    for (char c : s) {
        freq[c]++;
    }

    // Step 2: Use a max-heap (priority queue) to sort characters by frequency
    priority_queue<pair<int, char>> maxHeap;
    for (auto& [ch, count] : freq) {
        maxHeap.push({count, ch});
    }

    // Step 3: Build the result string based on frequency
    string ans;
    while (!maxHeap.empty()) {
        auto [count, ch] = maxHeap.top();
        maxHeap.pop();
        ans.append(count, ch);  // Append `count` occurrences of `ch`
    }

    return ans;
}
```
