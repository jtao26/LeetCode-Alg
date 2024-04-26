# Day10 栈与队列 Part1

## [232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks)

- 我们需要实现队列的基本行为
    - pop
    - push
    - empty
    - peak
- 根据栈先进后出的性质，如何让先入栈的元素像队列一样先出栈是难点
- 为了通过栈的行为来模拟队列，我们需要两个栈来实现队列先进先出的的效果
    - 一个是`入栈`，一个是`出栈`
    - 当元素进入队列时，我们将其压入`入栈`
    - 当需要元素出队列时
        - 如果此时`出栈`存在元素，我们直接获得`出栈`的第一个元素
        - 如果出栈为空，我们将所有入栈元素取出压入`出栈` ，然后返回第一个元素
            - 假设初始`入栈`为`[abc]`，随后调用了`pop`试图取出第一个元素，由于此时出栈为空，我们将所有元素压入`出栈`，出栈为`[cba]`，然后获得`a`作为队列首元素(假设右侧为栈顶)
            - 随后我们继续添加元素至队列，`入栈=[d]，出栈=[cb]`，随后再次调用`pop`，由于`出栈`有元素，直接返回出栈栈顶元素`b`，即为队列首元素
            - 此时`入栈=[d]，出栈=[c]`，重复…
    
    ```cpp
    class MyQueue {
    public:
        stack<int> in;
        stack<int> out;
        MyQueue() {
    
        }
        
        void push(int x) {
            in.push(x);
        }
        
        int pop() {
            if(!out.empty()){
                int val = out.top();
                out.pop();
                return val;
            }
            while(!in.empty()){
                out.push(in.top());
                in.pop();
            }
            int val = out.top();
            out.pop();
            return val;
        }
        
        int peek() {
            if(!out.empty()){
                return out.top();
            }
            while(!in.empty()){
                out.push(in.top());
                in.pop();
            }
            return out.top();
        }
        
        bool empty() {
            return in.empty() && out.empty();
        }
    };
    ```
    

## [225. Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues)

- 用队列实现栈可以用一个队列实现
- 基础原理是每当需要出栈时，我们遍历前n-1个元素，将其重新加入队列
- 遍历结束后队列头的元素就是栈顶的元素，可以直接出栈
- 如果只要获得栈顶元素而不出栈，直接调用back即可
    
    ```cpp
    class MyStack {
    public:
        queue<int> q;
        MyStack() {
    
        }
        
        void push(int x) {
            q.push(x);
        }
        
        int pop() {
            int size = q.size();
            for(int i=0; i<size-1; ++i) {
                q.push(q.front());
                q.pop();
            }
            int val = q.front();
            q.pop();
            return val;
        }
        
        int top() {
            return q.back();
        }
        
        bool empty() {
            return q.size() == 0;
        }
    };
    ```