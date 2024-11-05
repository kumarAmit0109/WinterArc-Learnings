# 166. Min Stack

### Problem Link

[LeetCode - Min Stack](https://leetcode.com/problems/min-stack/description/)

## Approach 1: Two Stack Approach

### Idea/Intuition

Use two stacks:

1. The main stack (`st1`) holds all values pushed.
2. The auxiliary stack (`st2`) holds the minimum values corresponding to each element in `st1`.

### Algorithm

1. **Push Operation**:

   - Push the value onto `st1`.
   - If `st2` is empty, push the value onto `st2`.
   - If the top of `st2` is greater than the value, push the value onto `st2`; otherwise, push the current minimum from `st2`.

2. **Pop Operation**:

   - Pop from both `st1` and `st2`.

3. **Top Operation**:

   - Return the top of `st1`.

4. **GetMin Operation**:
   - Return the top of `st2`.

### Time Complexity

- **O(1)** for `push`, `pop`, `top`, and `getMin`.

### Space Complexity

- **O(n)** for the two stacks.

### Code

```cpp
class MinStack {
public:
    stack<int> st1;
    stack<int> st2;

    MinStack() {}

    void push(int val) {
        st1.push(val);
        if (st2.empty() || st2.top() >= val) {
            st2.push(val);
        } else {
            st2.push(st2.top());
        }
    }

    void pop() {
        st1.pop();
        st2.pop();
    }

    int top() {
        return st1.top();
    }

    int getMin() {
        return st2.top();
    }
};
```

## Approach 2: Single Stack with Pair of Values

### Idea/Intuition

Use a single stack of pairs to store each element along with the minimum value at the time the element is pushed. This way, every stack entry has both the element and the minimum up to that point, making `getMin()` efficient.

### Algorithm

1. **Push Operation**:

   - If the stack is empty, push a pair with the value and itself as the minimum.
   - Otherwise, push a pair with the value and the current minimum (minimum of the top pair's min value and the current value).

2. **Pop Operation**:

   - Simply pop from the stack.

3. **Top Operation**:

   - Return the first element of the top pair in the stack.

4. **GetMin Operation**:
   - Return the second element of the top pair in the stack.

### Time Complexity

- **O(1)** for `push`, `pop`, `top`, and `getMin`.

### Space Complexity

- **O(n)**: Space used by the stack for storing pairs.

### Code

```cpp
class MinStack {
public:
    stack<pair<int, int>> st; // Pair of (value, min)

    MinStack() {}

    void push(int val) {
        if (st.empty()) {
            st.push({val, val});
        } else {
            st.push({val, min(val, st.top().second)});
        }
    }

    void pop() {
        if (!st.empty()) {
            st.pop();
        }
    }

    int top() {
        return st.empty() ? -1 : st.top().first;
    }

    int getMin() {
        return st.empty() ? -1 : st.top().second;
    }
};
```

## Approach 3: Single Stack with Encoded Minimum Value

### Idea/Intuition

This approach uses a single stack to store values with an encoded minimum. When pushing values, if the new value is less than the current minimum, we push an encoded value to the stack and update the minimum. This encoded value helps us retrieve the previous minimum when popping.

### Algorithm

1. **Push Operation**:

   - If the stack is empty, push the value and set it as the minimum.
   - If the value is greater than or equal to the minimum, push it normally.
   - If the value is less than the minimum, push an encoded value (`2 * val - min`) and update the minimum to the new value.

2. **Pop Operation**:

   - If the top of the stack is greater than or equal to the minimum, pop it normally.
   - If the top of the stack is less than the minimum, it means it was an encoded value. Update the minimum to `2 * min - top`, then pop the encoded value.

3. **Top Operation**:

   - If the top of the stack is greater than or equal to the minimum, return it as the top.
   - If the top is less than the minimum, return the current minimum as the top element.

4. **GetMin Operation**:
   - Return the current minimum.

### Time Complexity

- **O(1)** for `push`, `pop`, `top`, and `getMin`.

### Space Complexity

- **O(n)**: Space used by the stack.

### Code

```cpp
class MinStack {
public:
    stack<long> st;
    long min;

    MinStack() {}

    void push(int val) {
        long x = (long)val;
        if (st.empty()) {
            st.push(x);
            min = x;
        } else if (x >= min) {
            st.push(x);
        } else {
            st.push(2 * x - min); // Encode the minimum
            min = x;
        }
    }

    void pop() {
        if (st.empty()) return;

        if (st.top() >= min) {
            st.pop();
        } else {
            min = 2 * min - st.top(); // Decode previous minimum
            st.pop();
        }
    }

    int top() {
        if (st.empty()) return -1;

        return st.top() >= min ? (int)st.top() : (int)min;
    }

    int getMin() {
        return (int)min;
    }
};
```

# 167. Infix to Postfix

### Problem Link

[Naukri - Infix to Postfix](https://www.naukri.com/code360/problems/day-23-:-infix-to-postfix-_1382146)

## Approach: Using Stack for Operator Precedence and Associativity

### Idea/Intuition

Convert an infix expression to postfix notation by utilizing a stack to handle operator precedence and associativity. Operators are pushed onto the stack based on precedence and associativity, while operands are directly added to the output.

### Algorithm

1. **Operand Handling**:

   - If the character is an operand (letter or digit), add it directly to the postfix result.

2. **Operator Handling**:

   - If the character is an operator, pop operators from the stack to the postfix result as long as they have higher or equal precedence (considering right or left associativity).
   - Push the current operator onto the stack.

3. **Parentheses Handling**:

   - If the character is '(', push it to the stack.
   - If the character is ')', pop from the stack until a '(' is encountered, then discard '('.

4. **End of Expression**:
   - Pop and add all remaining operators from the stack to the postfix result.

### Time Complexity

- **O(n)**: Each character in the infix expression is processed once.

### Space Complexity

- **O(n)**: Space used by the stack.

### Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    if (op == '^') return 3;
    return 0;
}

bool isRightAssociative(char op) {
    return op == '^';
}

string infixToPostfix(string exp) {
    string postfix;
    stack<char> operators;

    for (char ch : exp) {
        // If the character is an operand, add it to the result
        if (isalnum(ch)) {
            postfix += ch;
        }
        // If the character is '(', push it to the stack
        else if (ch == '(') {
            operators.push(ch);
        }
        // If the character is ')', pop and output from the stack until '(' is encountered
        else if (ch == ')') {
            while (!operators.empty() && operators.top() != '(') {
                postfix += operators.top();
                operators.pop();
            }
            if (!operators.empty()) {
                operators.pop(); // Pop '('
            }
        }
        // An operator is encountered
        else {
            // Modified condition for operator precedence comparison
            while (!operators.empty() && operators.top() != '(' &&
                   ((isRightAssociative(ch) && precedence(ch) <= precedence(operators.top())) ||
                    (!isRightAssociative(ch) && precedence(ch) <= precedence(operators.top())))) {
                postfix += operators.top();
                operators.pop();
            }
            operators.push(ch);
        }
    }

    // Pop all the remaining operators from the stack
    while (!operators.empty()) {
        if (operators.top() != '(') {
            postfix += operators.top();
        }
        operators.pop();
    }

    return postfix;
}
```

# 168. Prefix to Infix Conversion

### Problem Link

[GeeksforGeeks - Prefix to Infix Conversion](https://www.geeksforgeeks.org/problems/prefix-to-infix-conversion/1)

## Approach: Using Stack for Operand and Operator Handling

### Idea/Intuition

To convert a prefix expression to infix notation, we will use a stack to hold operands. By reversing the prefix expression, we can process it from left to right, ensuring that operands are combined in the correct order with the appropriate operators.

### Algorithm

1. **Reverse the Prefix Expression**:

   - This allows us to process the expression from left to right.

2. **Iterate Over Characters**:

   - For each character:
     - If it's an operand (alphanumeric), push it onto the stack.
     - If it's an operator, pop the top two operands from the stack, create a new infix expression by placing the operator between the two operands, and push the new expression back onto the stack.

3. **Final Result**:
   - The last expression remaining in the stack is the desired infix expression.

### Time Complexity

- **O(n)**: Each character in the prefix expression is processed once.

### Space Complexity

- **O(n)**: Space used by the stack to store operands.

### Code

```cpp
string preToInfix(string pre_exp) {
    stack<string> s;

    // Reverse the prefix expression to process from left to right
    reverse(pre_exp.begin(), pre_exp.end());

    for (char ch : pre_exp) {
        // If the character is an operand, push it to the stack
        if (isalnum(ch)) {
            s.push(string(1, ch));
        }
        // If the character is an operator
        else {
            // Pop two operands from the stack
            string op1 = s.top(); s.pop();
            string op2 = s.top(); s.pop();

            // Form the infix expression with parentheses and push back to the stack
            string expr = "(" + op1 + ch + op2 + ")";
            s.push(expr);
        }
    }

    // The final expression in the stack is the infix expression
    return s.top();
}
```

# 169. Prefix to Postfix Conversion

### Problem Link

[GeeksforGeeks - Prefix to Postfix Conversion](https://www.geeksforgeeks.org/problems/prefix-to-postfix-conversion/1)

## Approach: Using Stack for Operand and Operator Handling

### Idea/Intuition

To convert a prefix expression to postfix notation, we utilize a stack to store operands. By iterating through the prefix expression from right to left, we can effectively combine operands with their respective operators.

### Algorithm

1. **Initialize a Stack**:

   - Create a stack to hold the operands.

2. **Iterate Over Characters**:

   - Traverse the prefix expression from the last character to the first:
     - If the character is an operator, pop the top two operands from the stack, concatenate them with the operator in postfix format (operand1 operand2 operator), and push the result back onto the stack.
     - If the character is an operand, push it onto the stack.

3. **Final Result**:
   - The last remaining item in the stack is the desired postfix expression.

### Time Complexity

- **O(n)**: Each character in the prefix expression is processed once.

### Space Complexity

- **O(n)**: Space used by the stack to store operands.

### Code

```cpp
bool isOperator(char c) {
    return (c == '+' || c == '-' || c == '*' || c == '/' ||
            c == '^' || c == '(' || c == ')');
}

string preToPost(string pre_exp) {
    // Create a stack to store operands
    stack<string> s;

    // Get length of expression
    int length = pre_exp.length();

    // Reading from right to left
    for(int i = length - 1; i >= 0; i--) {
        // If symbol is an operator
        if(isOperator(pre_exp[i])) {
            // Pop two operands from stack
            string op1 = s.top(); s.pop();
            string op2 = s.top(); s.pop();

            // Concatenate operands and operator
            string temp = op1 + op2 + pre_exp[i];

            // Push back string temp to stack
            s.push(temp);
        }
        // If symbol is an operand
        else {
            // Push the operand to the stack
            s.push(string(1, pre_exp[i]));
        }
    }

    // Stack now contains the postfix expression
    return s.top();
}
```

# 170. Postfix to Prefix Conversion

### Problem Link

[GeeksforGeeks - Postfix to Prefix Conversion](https://www.geeksforgeeks.org/problems/postfix-to-prefix-conversion/1)

## Approach: Using Stack for Operand and Operator Handling

### Idea/Intuition

To convert a postfix expression to prefix notation, we utilize a stack to store operands. By iterating through the postfix expression from left to right, we can effectively combine operands with their respective operators in the correct prefix format.

### Algorithm

1. **Initialize a Stack**:

   - Create a stack to hold the operands.

2. **Iterate Over Characters**:

   - Traverse the postfix expression from the first character to the last:
     - If the character is an operator, pop the top two operands from the stack, concatenate the operator with the operands in prefix format (operator operand1 operand2), and push the result back onto the stack.
     - If the character is an operand, push it onto the stack.

3. **Final Result**:
   - The last remaining item in the stack is the desired prefix expression.

### Time Complexity

- **O(n)**: Each character in the postfix expression is processed once.

### Space Complexity

- **O(n)**: Space used by the stack to store operands.

### Code

```cpp
bool isOperator(char c) {
    return (c == '+' || c == '-' || c == '*' || c == '/' ||
            c == '^' || c == '(' || c == ')');
}

string postToPre(string post_exp) {
    // Create a stack to store operands
    stack<string> s;

    // Get length of expression
    int length = post_exp.length();

    // Reading from left to right
    for(int i = 0; i < length; i++) {
        // If symbol is an operator
        if(isOperator(post_exp[i])) {
            // Pop two operands from stack
            string op2 = s.top(); s.pop();
            string op1 = s.top(); s.pop();

            // Concatenate operator with operands
            // Note: In prefix, operator goes first
            string temp = post_exp[i] + op1 + op2;

            // Push string temp back to stack
            s.push(temp);
        }
        // If symbol is an operand
        else {
            // Push the operand to the stack
            s.push(string(1, post_exp[i]));
        }
    }

    // Stack now contains the prefix expression
    return s.top();
}
```
