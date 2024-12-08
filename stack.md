## 六、栈

### 6.1 有效的括号

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。
3. 每个右括号都有一个对应的相同类型的左括号

#### 1

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> sk;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(' || s[i] == '[' || s[i] == '{') {
                sk.push(s[i]);
            }
            if (s[i] == ')') {
                if (sk.empty() || sk.top() != '(') {
                    return false;
                } else {
                    sk.pop();
                }
            }
            if (s[i] == '}') {
                if (sk.empty() || sk.top() != '{') {
                    return false;
                } else {
                    sk.pop();
                }
            }
            if (s[i] == ']') {
                if (sk.empty() || sk.top() != '[') {
                    return false;
                } else {
                    sk.pop();
                }
            }
        }
        return sk.empty() ? true : false;
    }
};
```

### 6.2 逆波兰表达式求值

#### 1

```c++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> s;
        for (int i = 0; i < tokens.size(); i++) {
            if (tokens[i] == "+") {
                int num1 = s.top();
                s.pop();
                int num2 = s.top();
                s.pop();
                s.push(num1 + num2);
            } else if (tokens[i] == "*") {
                int num1 = s.top();
                s.pop();
                int num2 = s.top();
                s.pop();
                s.push(num1 * num2);
            } else if (tokens[i] == "-") {
                int num1 = s.top();
                s.pop();
                int num2 = s.top();
                s.pop();
                s.push(num2 - num1);
            } else if (tokens[i] == "/") {
                int num1 = s.top();
                if (num1 == 0) {
                    return 0;
                }
                s.pop();
                int num2 = s.top();
                s.pop();
                s.push(num2 / num1);
            } else {
                int num = stoi(tokens[i]);//将字符串转换为数字型
                s.push(num);
            }
        }
        return s.top();
    }
};
```

### 6.3 中缀表达式转后缀表达式

```c++
/*
    思路
    1. 遇到数字, 拼串
    2. 遇到 + - * /
    - 优先级高于栈顶运算符 入栈
    - 否则将栈中高级或平级运算符出栈拼串, 本运算符入栈
    3. 遍历完成, 栈中剩余运算符出栈拼串
    - 先出栈,意味着优先运算
    4. 带 ()
    - 左括号直接入栈,左括号优先级设为0
    - 右括号要将栈中直至左括号为止的运算符出栈拼串
*/

int priority(char c) {
    switch (c) {
        case '*':
        case '/':
            return 2;
        case '+':
        case '-':
            return 1;
        case '(':
            return 0;
        default:
            return -1;
    }
}

string infixToSuffix(string &infix) {
    stack<char> stk;
    string suffix;
    for (char c: infix) {
        switch (c) {
            case '*':
            case '+':
            case '/':
            case '-':
                if (stk.empty()) {
                    stk.push(c);
                } else {
                    if (priority(c) > priority(stk.top())) {
                        stk.push(c);
                    } else {
                        while (!stk.empty() && priority(stk.top()) >= priority(c)) {
                            suffix.push_back(stk.top());//将符号添加到字符串上
                            stk.pop();
                        }
                        stk.push(c);
                    }
                }
                break;
            case '(':
                stk.push(c);
                break;
            case ')': {
                while (stk.top() != '(') {
                    cout << stk.top();
                    suffix.push_back((stk.top()));
                    stk.pop();
                }
                stk.pop();
            }
                break;
            default: {
                suffix.push_back(c);
            }
        }
    }
    while (!stk.empty()) {
        suffix.push_back(stk.top());
        stk.pop();
    }
    return suffix;
}

int main() {
//    string infix = "(a+b*c-d)*e";
    string infix1="(a-b)/(c+d)";
    const string &suffix = infixToSuffix(infix1);
    cout << suffix << endl;
}
```



