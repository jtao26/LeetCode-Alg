# Day13 栈与队列 Part3

## [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum)

- 本题的暴力解法：对于每个位置的每个窗口，遍历所有元素找出最大值 - 100%超时
    - 代码实现
        
        ```cpp
        vector<int> maxSlidingWindow(vector<int>& nums, int k) {
                vector<int> ans;
                for(int i=0; i<nums.size() - k + 1; ++i) {
                    int max_val = INT_MIN;
                    for(int j=0; j<k; ++j) {
                        max_val = max(max_val, nums[i+j]);
                    }
                    ans.push_back(max_val);
                }
                return ans;
            }
        ```
        
- 使用单调队列解法：
    - 实现一个单调队列，其中元素是从左到右严格递减的
    - 窗口每次移动都要检查：
        - 移出窗口的元素是否为队列头元素，如果是，队列要pop front
        - 加入窗口的元素是否能直接呆在末尾（即不破坏左至右严格递减）
            - 如果新元素大于队列头元素，意味着其为当前最大元素，pop front掉所有队列中的元素，只保留新元素
            - 如果其不大于队头元素，那么从队尾开始，所有比其小的元素都会被pop back，直至队尾元素比新元素大，然后再添加新元素
        - 移出添加结束后，此时队列仍是一个严格递减的元素，我们将队列头的最大值push进结果数组
    - 代码实现
        
        ```cpp
        vector<int> maxSlidingWindow(vector<int>& nums, int k) {
                deque<int> q;
                vector<int> ans;
                for(int i=0; i<k; ++i) {
                    if(q.empty())
                        q.push_back(nums[i]);
                    else{
                        if(nums[i] < q.front()){
                            while(q.back() < nums[i]) {
                                q.pop_back();
                            }
                            q.push_back(nums[i]);
                        }
                        else if(nums[i] == q.front()) {
                            while(!q.empty() && q.back() < nums[i])
                                q.pop_back();
                            q.push_back(nums[i]);
                        }
                        else {
                            q.push_back(nums[i]);
                            while(q.front() != nums[i])
                                q.pop_front();
                        }
                    }
                }
        
                ans.push_back(q.front());
        
                for(int i=1; i<nums.size() - k + 1; ++i) {
                    int rmIdx = i - 1;
                    int addIdx = i + k - 1;
                    if(nums[rmIdx] == q.front()) {
                        q.pop_front();
                    }
                    if(nums[addIdx] < q.front()){
                        while(!q.empty() && q.back() < nums[addIdx]) {
                            q.pop_back();
                        }
                        q.push_back(nums[addIdx]);
                    }
                    else if(nums[addIdx] == q.front()) {
                        while(!q.empty() && q.back() < nums[addIdx])
                            q.pop_back();
                        q.push_back(nums[addIdx]);
                    }
                    else {
                        q.push_back(nums[addIdx]);
                        while(q.front() != nums[addIdx])
                            q.pop_front();
                    }
                    ans.push_back(q.front());
                }
                
                return ans;
            }
        ```
        
- 卡哥解法：实现一个针对本题的单调队列数据结构
    
    ```cpp
    private:
        class MyQueue { //单调队列（从大到小）
        public:
            deque<int> que; // 使用deque来实现单调队列
            // 每次弹出的时候，比较当前要弹出的数值是否等于队列出口元素的数值，
            // 如果相等则弹出。
            // 同时pop之前判断队列当前是否为空。
            void pop(int value) {
                if (!que.empty() && value == que.front()) {
                    que.pop_front();
                }
            }
            // 如果push的数值大于入口元素的数值，那么就将队列后端的数值弹出，
            // 直到push的数值小于等于队列入口元素的数值为止。
            // 这样就保持了队列里的数值是单调从大到小的了。
            void push(int value) {
                while (!que.empty() && value > que.back()) {
                    que.pop_back();
                }
                que.push_back(value);
    
            }
            // 查询当前队列里的最大值 直接返回队列前端也就是front就可以了。
            int front() {
                return que.front();
            }
        };
    public:
        vector<int> maxSlidingWindow(vector<int>& nums, int k) {
            MyQueue que;
            vector<int> result;
            for (int i = 0; i < k; i++) { // 先将前k的元素放进队列
                que.push(nums[i]);
            }
            result.push_back(que.front()); // result 记录前k的元素的最大值
            for (int i = k; i < nums.size(); i++) {
                que.pop(nums[i - k]); // 滑动窗口移除最前面元素
                que.push(nums[i]); // 滑动窗口前加入最后面的元素
                result.push_back(que.front()); // 记录对应的最大值
            }
            return result;
        }
    ```
    

## [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements)

- 感觉是很常见的一道题，也是存在真实需求的一道题，值得好好熟悉思路和方法
- 我们的目的是在所有的元素中求最常出现的k个元素
- 计算元素出现的次数我们常常用哈希表 unordered_map 记录次数
- 在记录各元素次数后，我们通过 **小顶堆** 来求解本题，原因是我们维护大小为k的小顶堆，每一次将一个元素压入，我们都需要移出一个元素。但我们只能移出堆顶元素，如果此时是大顶堆，我们就会把我们想要保留的最常出现元素移出。
- **因此，利用反向思维，只要我们移除了n个元素中n-k个出现次数最少的元素，那么剩下的k个就是出现最多的元素**
- 解题代码
    
    ```cpp
    class Solution {
    public:
        class mycomparison {
        public:
            bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs) {
                return lhs.second > rhs.second;
            }
        };
    
        vector<int> topKFrequent(vector<int>& nums, int k) {
            unordered_map<int, int> mp;
            for(int i=0; i<nums.size(); ++i) {
                mp[nums[i]]++;
            }
    
            priority_queue<pair<int, int>, vector<pair<int,int>>, mycomparison> min_heap;
    
            for(auto m : mp) {
                min_heap.push(m);
                if(min_heap.size() > k)
                    min_heap.pop();
            }
            
            vector<int> ans;
            int size = min_heap.size();
            for(int i=0; i<size; ++i) {
                ans.push_back(min_heap.top().first);
                min_heap.pop();
            }
            return ans;
        }
    };
    ```
    
- 需要学习的知识点：
    - 为什么小顶堆大顶堆的排序函数写成一个clas