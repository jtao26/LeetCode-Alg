# Day11 栈与队列 Part2

## [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses)

- 使用栈来追踪当前的括号，并遍历字符串考虑以下多种情况
    - 当前遍历的字符串是`( or [ or {`
        - 将当前括号压入栈
    - 当前遍历的字符串是`) or ] or }`且栈中有元素
        - 比对当前括号与栈顶括号是否匹配
            - 匹配 → 溢出栈顶元素
            - 不匹配 → 返回`false`
    - 当前遍历的字符串是`) or ] or }`且栈为空
        - 返回`false`，说明我们没有准备好的左括号
- 最后判断栈是否为空
    - 栈为空，匹配了所有括号，返回`true`
    - 栈不为空，至少有一个括号没有被匹配上，返回`false`
- 解题代码
    
    ```cpp
    class Solution {
    public:
        bool isValid(string str) {
            stack<char> s;
            for(int i=0; i<str.length(); ++i) {
                if(str[i] == '(' || str[i] == '[' || str[i] == '{') {
                    s.push(str[i]);
                }
                else {
                    if(!s.empty()) {
                        if(str[i] == ')' && s.top() != '(')
                            return false;
                        else if(str[i] == ']' && s.top() != '[')
                            return false;
                        else if(str[i] == '}' && s.top() != '{')
                            return false;
                        else {
                            s.pop();
                        }
                    }
                    else {
                        return false;
                    }
                }
            }
    
            return s.empty();
        }
    };
    ```
    

## [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string)

- 如果当前栈为空或者栈顶元素不等于我们遍历的元素，入栈
- 否则出栈栈顶元素（栈不为空且栈顶元素和我们比较元素相同）
- 注意最后构建答案字符串时的时间开销
    - 答案要求保持原顺序输出，此时的构建顺序影响时间表现
        - 方式一，一步到位将栈顶元素添加到字符串之前
            
            ```cpp
            while(!s.empty()){
                ans = s.top() + ans;
                s.pop();
            }
            ```
            
        - 方式二，将栈顶元素添加到字符串后，然后反转字符串
            
            ```cpp
             while(!s.empty()){
                ans += s.top();
                s.pop();
            }
            reverse (ans.begin(), ans.end());
            ```
            
    - 方式一将导致时间上的巨大开销，原因在于每次`s.top() + ans`会创建一个临时字符串，然后再赋值给str，赋值的时候涉及到将所有的字符拷贝到新位置需要`O(n)`时间，遍历所有字符串，需要`O(n^2)`
    - 方式二直接在源字符串基础上添加新字符串，花费`O(1)`，没有新空间的开辟和拷贝（只有原字符串的扩容），完成所有字符需要`O(n)`，最后反转需要`O(n)`时间，故一共`O(n)`
- 解题代码
    
    ```cpp
    class Solution {
    public:
        string removeDuplicates(string str) {
            stack<char> s;
            for(char c : str) {
                if(s.empty() || c != s.top())
                    s.push(c);
                else {
                    s.pop();
                }
            }
            str = "";
            while(!s.empty()){
                str += s.top();        // O(1) 
                s.pop();
            }
            reverse (str.begin(), str.end());    // O(n)
            return str;
        }
    };
    ```
    

## [150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation)

- 每次遇到运算符，我们取栈顶两个元素，第二个元素作为运算符左侧的元素
- 运算后将结果压回栈内
- 解题代码
    
    ```cpp
    class Solution {
    public:
        int evalRPN(vector<string>& tokens) {
            stack<string> st;
    
            for(auto s : tokens) {
                if(s == "+" || s == "-" || s == "*" || s == "/") {
                    int a2 = stoi(st.top());
                    st.pop();
                    int a1 = stoi(st.top());
                    st.pop();
                    if(s == "+")
                        a2 = a1 + a2;
                    else if(s == "-")
                        a2 = a1 - a2;
                    else if(s == "*")
                        a2 = a1 * a2;
                    else
                        a2 = a1 / a2;
                    st.push(to_string(a2));
                }
                else{
                    st.push(s);
                }
            }
            return stoi(st.top());
        }
    };
    ```