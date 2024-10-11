# Problem: Minimum Add to Make Parentheses Valid

[LeetCode Problem Link](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/description/)

## Approach 1

We can use a stack to track unmatched parentheses. The key idea is to traverse the string and push opening brackets `(` onto the stack. For closing brackets `)`, check if the stack contains an unmatched opening bracket `(`. If yes, pop the stack to match the parentheses. If not, count the unmatched closing brackets. At the end, the remaining unmatched opening brackets on the stack and the unmatched closing brackets need to be added to make the parentheses valid.

### Time Complexity

- The time complexity is \(O(N)\), where \(N\) is the length of the string `s`.

### Space Complexity

- The space complexity is \(O(N)\) in the worst case due to the stack usage.

## C++ Code Implementation

```cpp
class Solution {
public:
    int minAddToMakeValid(string s) {
        int count = 0;
        stack<char> st;

        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(') {
                st.push('('); // Push the opening bracket to the stack
            } else {
                if (!st.empty() && st.top() == '(') {
                    st.pop(); // Match the closing bracket with the top of the stack
                } else {
                    count++; // Increment for unmatched closing bracket
                }
            }
        }

        return st.size() + count; // Remaining unmatched opening brackets + unmatched closing brackets
    }
};
```

## Approach 2

In this approach, we track unmatched opening and closing brackets directly using two variables:

- `openingBracket`: to track unmatched opening brackets `(`.
- `closingBracket`: to track unmatched closing brackets `)`.

As we traverse the string:

1. For every opening bracket `(`, increment the `openingBracket` count.
2. For every closing bracket `)`, check if there's an unmatched opening bracket available (`openingBracket > 0`). If so, decrement the `openingBracket` count, as this `)` can match with an earlier `(`. If no unmatched opening bracket is available, increment the `closingBracket` count.

At the end, the sum of `openingBracket` and `closingBracket` gives the total number of parentheses to be added to make the string valid.

### Time Complexity

- The time complexity is \(O(N)\), where \(N\) is the length of the string `s`.

### Space Complexity

- The space complexity is \(O(1)\), since we only use two integer variables.

## C++ Code Implementation

```cpp
class Solution {
public:
    int minAddToMakeValid(string s) {
        int openingBracket = 0;  // Count of unmatched '('
        int closingBracket = 0;  // Count of unmatched ')'

        for (auto ch : s) {
            if (ch == '(') {
                openingBracket++;  // Increment for unmatched '('
            } else {
                if (openingBracket > 0) {
                    openingBracket--;  // Match this ')' with a previous '('
                } else {
                    closingBracket++;  // Increment for unmatched ')'
                }
            }
        }

        // The total number of additions needed is the sum of unmatched '(' and unmatched ')'
        return openingBracket + closingBracket;
    }
};
```
