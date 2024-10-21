# Parsing a Boolean Expression

[LeetCode Problem Link](https://leetcode.com/problems/parsing-a-boolean-expression/description/)

## Approach: Using Stack to Evaluate the Expression

### Idea:

1. Use a **stack** to process the boolean expression character by character.
2. Whenever you encounter an **opening parenthesis or any character** (like `t`, `f`, `!`, `&`, `|`), push it onto the stack.
3. As soon as you encounter a **closing parenthesis**:

   - Start **popping elements** from the stack until you encounter the **opening parenthesis** `'('`.
   - Collect the popped elements in a **vector**.
   - Once the opening parenthesis is found, **pop once more** to get the **operator** (like `!`, `&`, or `|`).
   - Apply the operator on the collected elements to get the result.
   - **Push the result** back onto the stack.

4. Repeat this process until the entire expression is traversed, and the **final answer will remain on the top of the stack**.

### Time Complexity

- **O(n)**: We traverse the expression once and perform constant-time operations (pushing, popping) for each character.

### Space Complexity

- **O(n)**: In the worst case, the stack may hold all characters from the expression.

### Code

```cpp
class Solution {
public:
    bool parseBoolExpr(string expression) {
        stack<char> st;  // Stack to process the expression

        // Traverse each character of the expression
        for (char ch : expression) {
            if (ch == ')') {
                // Vector to collect elements inside the current parentheses
                vector<char> elements;

                // Pop elements until '(' is found
                while (st.top() != '(') {
                    elements.push_back(st.top());
                    st.pop();
                }

                // Pop the '(' character
                st.pop();

                // Pop the operator (!, &, or |)
                char op = st.top();
                st.pop();

                // Compute the result based on the operator
                bool result;
                if (op == '!') {
                    // NOT operator: Invert the single element
                    result = (elements[0] == 'f');
                } else if (op == '&') {
                    // AND operator: All elements must be 't'
                    result = all_of(elements.begin(), elements.end(), [](char c) { return c == 't'; });
                } else if (op == '|') {
                    // OR operator: At least one element must be 't'
                    result = any_of(elements.begin(), elements.end(), [](char c) { return c == 't'; });
                }

                // Push the result back onto the stack as 't' or 'f'
                st.push(result ? 't' : 'f');
            } else if (ch != ',') {
                // Push any other character (except ',') onto the stack
                st.push(ch);
            }
        }

        // Final result will be on the top of the stack
        return st.top() == 't';
    }
};
```

## Approach 2: Optimized Stack-Based Evaluation

### Idea

1. Use a **stack** to process the expression character by character.
2. When encountering a **closing parenthesis (`')'`)**, pop elements from the stack until reaching the corresponding **opening parenthesis (`'('`)**.
3. Compute the result for the popped elements directly:
   - **OR (`|`)**: At least one `true` value (`'t'`).
   - **AND (`&`)**: All values must be `true`.
   - **NOT (`!`)**: Invert the value.
4. Push the result back into the stack and continue until the entire string is processed.

### Time Complexity

- **O(n)**: Where `n` is the length of the expression. Each character is processed once.

### Space Complexity

- **O(n)**: Due to the stack storing intermediate characters.

### Code

```cpp
class Solution {
public:
    bool parseBoolExpr(string expression) {
        stack<char> st;

        for (char ch : expression) {
            if (ch == ')') {
                vector<char> elements;

                // Pop elements until '(' is found
                while (st.top() != '(') {
                    elements.push_back(st.top());
                    st.pop();
                }
                st.pop(); // Pop the '('

                // Get the operator for this group
                char operation = st.top();
                st.pop();

                // Compute the result based on the operation
                bool result;
                if (operation == '&') {
                    result = all_of(elements.begin(), elements.end(), [](char c) { return c == 't'; });
                } else if (operation == '|') {
                    result = any_of(elements.begin(), elements.end(), [](char c) { return c == 't'; });
                } else { // operation == '!'
                    result = elements[0] == 'f';
                }

                // Push the result back as 't' or 'f'
                st.push(result ? 't' : 'f');
            } else if (ch != ',') {
                st.push(ch); // Push all other characters
            }
        }

        // Final result will be on top of the stack
        return st.top() == 't';
    }
};
```
