# Day2 数组 Part2

- 今日主要包含双指针和滑动窗口
    - **构造滑动窗口需要确定以下三点：**
        - 窗口内包含什么
        - 何时移动窗口的起始位置
        - 何时移动窗口的结束位置

## 977 Squares of a Sorted Array

### First time code:

- 先平方再排序，非常基本的解题思路
- 由于使用了sort() 时间为 *O(nlogn)*

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        for(int i=0; i<nums.size(); ++i) {
            nums[i] *= nums[i];
        }
        sort(nums.begin(), nums.end());
        return nums;
    }
};
```

---

- 双指针可以将时间降为 *O(n)*
    - 因为原始数组是有序的，因此在原数组平方后最大值只会出现在数组头尾
        - e.g. [ -3, -2, 0, 2, 4] 平方后最大值源自尾部 4, 为 4 * 4 ⇒ 16
        - e.g. [ -5, -2, 0, 2, 4] 平方后最大值源自头部 -5, 为 -5 * -5 ⇒ 25
    - 根据这一特性，我们可以用双指针从数组两侧向中间迭代，不断寻找最大值填充进结果数组
        - 初始化  k = nums.size(), left = 0, right = k - 1, 和结果数组ans
        - 当满足 left <= right 时
            - 如果 left 指向的元素平方后大于 right 元素的平方
                - 将 left 的元素更新给 ans[k-1]
                - 右移 left，++left;
            - 如果 left 指向的元素平方后小于等于 right 元素的平方
                - 将 right 的元素更新给 ans[k-1]
                - 左移 right，--right;
            - 减小 k, --k;
        - 循环跳出后left > right，此时结果数组 ans 已经填充完毕，返回 ans
    - 双指针代码
        
        ```cpp
        class Solution {
        public:
            vector<int> sortedSquares(vector<int>& nums) {
                int size = nums.size();
                vector<int> ans(size, 0);
                int left = 0, right = size - 1;
                while(left <= right) {
                    if(nums[left] * nums[left] > nums[right] * nums[right]) {
                        ans[size-1] = nums[left] * nums[left];
                        ++left;
                    }
                    else {
                        ans[size-1] = nums[right] * nums[right];
                        --right;
                    }
                    --size;
                }
                return ans;
            }
        };
        ```
        

## 209 Minimum Size Subarray Sum

### First Time Code:

- 通过双指针形成滑动窗口解题
    - 维护一个 currSum 每次窗口向右扩展后如果和大于等于target
        - 移动左指针缩小窗口大小，动态更新最小长度 ans
    
    ```cpp
    class Solution {
    public:
        int minSubArrayLen(int target, vector<int>& nums) {
            int currSum = 0;
            int ans = nums.size() + 1;
            for(int left = 0, right = 0; right < nums.size(); ++right) {
                currSum += nums[right];
                if(currSum >= target) {
                    ans = min(ans, right - left + 1);
                    while(left < right) {
                        currSum -= nums[left];
                        ++left;
                        if(currSum >= target)
                            ans = min(ans, right - left + 1);
                        else
                            break;
                    }
                }
            }
            if(ans != nums.size() + 1)
                return ans;
            return 0;
        }
    };
    ```
    

---

- **确定构造滑动窗口以下三点：**
    - 窗口内包含什么：满足和 ≥ s 的长度最小的**连续子数组**
    - 何时移动窗口的起始位置：如果当前窗口的值 ≥ s，起始位置就要向前移动，因为我们在寻找最小窗口
    - 何时移动窗口的结束位置：在每次for循环向后移动窗口结束位置，将新元素加入窗口内
- **起始位置如何移动：**
    - 每当 currSum 超过我们的 target 值后，我们就会尝试缩小窗口
    
    ```
    while(currSum >= target) {
        ans = min(ans, right - left + 1);
        currSum -= nums[left];
        ++left;
    }
    ```
    
- 滑动窗口解决方法
    
    ```cpp
    class Solution {
    public:
        int minSubArrayLen(int target, vector<int>& nums) {
            int currSum = 0;
            int ans = nums.size() + 1;
            for(int left = 0, right = 0; right < nums.size(); ++right) {
                currSum += nums[right];
                while(currSum >= target) {
                    ans = min(ans, right - left + 1);
                    currSum -= nums[left];
                    ++left;
                }
            }
            if(ans != nums.size() + 1)
                return ans;
            return 0;
        }
    };
    ```
    
- 虽然在 for 循环内存在一个 while 循环，但时间复杂度为 *O(n)*
    - 对于每个元素来讲，算法最多操作它们两次，加入窗口和移出窗口各一次，最多有 2 * n 次执行，即为 *O(n)*
    

## 59 Spiral Matrix II

- 螺旋矩阵难点在于如何规划遍历二维矩阵的顺序
- 遍历时要主意 Day1 中提到的矩阵不变量原则：**左闭右闭**或者**左闭右开**
- 本题应使用左闭右开：即首先我们要处理每一行(列)的第一个元素，而不处理最后一个元素。最后一个元素留给下一列(行)处理
- 每走一圈，我们需要依次完成四条边的数字，以第一次为例：
    - 上边：处理元素 [0][0, n-1) ，第一行最后一个元素不处理，方向从左向右
    - 右边：处理元素 [0, n-1)[n-1]，最后一列最后一行元素不处理，方向从上到下
    - 下边：处理 [n-1](0, n-1]，第一列最后一行不处理，方向从右向左
    - 左边：处理 [n-1, 0)[0]，第一列第一行不处理，因为处理上边时已经搞定，方向从下到上
- 除去基本的绕圈模拟，在进入内层圈的时候，每条边的终点比外圈不同，每向内进一层圈，就多一层外圈已经被处理好，意味着 offset 应相应增加
- 最后，如果形成的矩阵为基数，则中心元素尚未被赋值，需要额外对其进行最后赋值
- 坚持循环不变量和数组的定义，模拟遍历每条边就会相对简单
- 算法如下
    
    ```cpp
    class Solution {
    public:
        vector<vector<int>> generateMatrix(int n) {
            vector<vector<int>> m(n, vector<int>(n, -1));
            int startX=0, startY=0;
            int i = 0, j = 0;
            int offset = 1;
            int count = 1;
            int loop = 1;
            while(loop <= n / 2) {
    		        // process up
                for(j=startY; j<n-offset; ++j) {
                    m[startX][j] = count++;
                }
                // process right
                for(i=startX; i<n-offset; ++i) {
                    m[i][j] = count++;
                }
                // process down
                for(;j>startY; --j) {
                    m[i][j] = count++;
                }
                // process left
                for(;i>startX; --i) {
                    m[i][j] = count++;
                }
                // reset starting point of X and Y
                ++startX;
                ++startY;
                // going inner loop, offset increase
                ++offset;
                ++loop;
            }
    				
            if(n % 2 == 1)
                m[n/2][n/2] = count;
                
            return m;
        }
    };
    ```