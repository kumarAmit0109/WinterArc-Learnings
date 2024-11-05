# 171. Postfix to Infix Conversion

### Problem Link

[GeeksforGeeks - Postfix to Infix Conversion (GeeksforGeeks)](https://www.geeksforgeeks.org/problems/postfix-to-infix-conversion/1)

## Approach: Using Stack for Operand Handling

### Idea/Intuition

To convert a postfix expression to infix notation, we utilize a stack to store operands. By iterating through the postfix expression, we can effectively combine operands with their respective operators to form the correct infix format.

### Algorithm

1. **Initialize a Stack**:

   - Create a stack to hold the operands.

2. **Iterate Over Characters**:

   - Traverse the postfix expression from the first character to the last:
     - If the character is an operand, push it onto the stack.
     - If the character is an operator, pop the top two operands from the stack, combine them with the operator in infix format (operand1 operator operand2), and push the resulting string back onto the stack.

3. **Final Result**:
   - The last remaining item in the stack is the desired infix expression.

### Time Complexity

- **O(n)**: Each character in the postfix expression is processed once.

### Space Complexity

- **O(n)**: Space used by the stack to store operands.

### Code

```cpp
string postToInfix(string exp) {
    stack<string> s;

    for (char ch : exp) {
        // If the character is an operand, push it to the stack
        if (isalnum(ch)) {
            s.push(string(1, ch));
        }
        // If the character is an operator
        else {
            // Pop two operands from the stack
            string op2 = s.top(); s.pop();
            string op1 = s.top(); s.pop();

            // Combine the operands with the operator in infix format
            string infix = "(" + op1 + ch + op2 + ")";
            s.push(infix);
        }
    }

    // The final expression in the stack is the infix expression
    return s.top();
}
```

# 172. Infix to Prefix Conversion

### Article Link

[Convert Infix to Prefix Notation (GeeksforGeeks)](https://www.geeksforgeeks.org/convert-infix-prefix-notation/)

## Approach: Using Stack for Operator Precedence and Associativity with Expression Reversal

### Idea/Intuition

To convert an infix expression to prefix notation, reverse the expression, convert it to postfix notation with modified parentheses, then reverse the postfix result to get the prefix expression.

### Algorithm

1. **Preprocessing**:

   - Remove any whitespace from the expression.

2. **Reverse Expression**:

   - Reverse the infix expression and swap each '(' with ')' and vice versa.

3. **Infix to Postfix Conversion**:

   - Use a stack to convert the modified infix expression to postfix notation.
   - Push operators onto the stack based on precedence and associativity.
   - Handle operands, operators, and parentheses appropriately, popping and appending operators based on precedence.

4. **Reverse Postfix**:
   - Reverse the resulting postfix expression to get the prefix expression.

### Time Complexity

- **O(n)**: Each character in the infix expression is processed multiple times for reversal, transformation, and stack operations.

### Space Complexity

- **O(n)**: Space used by the stack and output strings.

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
    return op == '^';  // Only '^' is right-associative
}

string infixToPostfix(string exp) {
    // Remove whitespace from the expression
    exp.erase(remove_if(exp.begin(), exp.end(), ::isspace), exp.end());

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
            if (!operators.empty()) {  // Check if there was a matching '('
                operators.pop(); // Pop '('
            }
        }
        // An operator is encountered
        else {
            while (!operators.empty() && operators.top() != '(' &&
                   ((isRightAssociative(ch) && precedence(ch) < precedence(operators.top())) ||
                    (!isRightAssociative(ch) && precedence(ch) <= precedence(operators.top())))) {
                postfix += operators.top();
                operators.pop();
            }
            operators.push(ch);
        }
    }

    // Pop all the operators left in the stack
    while (!operators.empty()) {
        if (operators.top() != '(') {  // Skip any unmatched '(' if present
            postfix += operators.top();
        }
        operators.pop();
    }

    return postfix;
}

string infixToPrefix(string infix) {
    // Remove whitespace from the input
    infix.erase(remove_if(infix.begin(), infix.end(), ::isspace), infix.end());

    // Step 1: Reverse the infix expression
    reverse(infix.begin(), infix.end());

    // Step 2: Replace '(' with ')' and vice versa
    for (char &ch : infix) {
        if (ch == '(') ch = ')';
        else if (ch == ')') ch = '(';
    }

    // Step 3: Convert the modified infix expression to postfix
    string postfix = infixToPostfix(infix);

    // Step 4: Reverse the postfix expression to get the prefix expression
    reverse(postfix.begin(), postfix.end());
    return postfix;
}

int main() {
    string infix = "3^(1+1)";
    cout << "Infix Expression: " << infix << endl;
    string prefix = infixToPrefix(infix);
    cout << "Prefix Expression: " << prefix << endl;

    // Test with whitespace
    string infix2 = "3 ^ (1 + 1)";
    cout << "\nInfix Expression with spaces: " << infix2 << endl;
    string prefix2 = infixToPrefix(infix2);
    cout << "Prefix Expression: " << prefix2 << endl;

    return 0;
}
```
