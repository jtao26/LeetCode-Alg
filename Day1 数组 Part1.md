# Day1 算法训练营 数组 Part1

- 数组是存放在连续内存空间上的相同类型数据的集合
    - 数组下标都是从0开始的。
    - 数组内存空间的地址是连续的
    - 二维数组在内存空间中的地址是连续的
- 删除或者增添数组元素的时候，**需要移动**其他元素的地址
- 在C++中，vector是容器，它的底层实现是数组

## 704 Binary Search

### First time code:

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        int mid;
        while(left <= right) {
            mid = left + (right - left) / 2;
            if(nums[mid] == target) {
                return mid;
            }
            else if(nums[mid] < target) {
                left = mid + 1;
            }
            else {
                right = mid - 1;
            }
        }
        return -1;
    }
};
```

---

- 二分搜索在循环搜索的过程中需要注意的内容：
    - 坚持搜索区间的定义**(左闭右闭)、(左闭右开)**
    - 根据对应的区间定义更新 left / right 的值
    - 如果是**左闭右闭**，那么对于区间的定义：
        - 潜在的目标元素如果存在，一定在 [left, right] 中，且区间中都是尚未检查的元素
        - 每次与mid值比较且没有查找到目标元素时，剩余的二分区间一定不包含mid位置
            - e.g. mid > target ⇒ 因此下一个迭代的区间是 [left, mid-1]，因为mid已经确认不包含我们需要的目标元素
            - e.g. mid < target ⇒ 下一个迭代的区间是 [mid+1, right]
        - 每次循环判断时，**while 的条件判断**需要写成 **left <= right**
            - 由于我们的区间定义意味着所有区间内的元素都是未检查的元素(可能为目标元素)
            - 当区间长度为2时，剩余2个元素尚未被检查 ⇒ 同理长度为1时仍需要检查
            - 因此此时 **left <= right** 保证了最后一次循环会检查 left == right 位置的元素
        - **左闭右闭答案写法:**
            
            ```cpp
            class Solution {
            public:
                int search(vector<int>& nums, int target) {
                    int left = 0, right = nums.size() - 1;
                    int mid;
                    while(left <= right) {
                        mid = left + (right - left) / 2;
                        if(nums[mid] == target) {
                            return mid;
                        }
                        else if(nums[mid] < target) {
                            left = mid + 1;
                        }
                        else {
                            right = mid - 1;
                        }
                    }
                    return -1;
                }
            };
            ```
            
    - 如果是**左闭右开**，那么对于区间的定义：
        - 目标如果存在，一定在 [left, right) 中，且区间中都是尚未检查的元素
            - **注意此时该区间不包含 right**
        - 每次与mid值比较且没有查找到目标元素时，剩余的二分区间由区间定义意义
            - e.g. mid > target ⇒ 下一个迭代的区间是 [left, mid)
            - e.g. mid < target ⇒ 下一个迭代的区间是 [mid+1, right)
                - **此时使用 mid+1 因为我们是左闭右开区间**
        - 每次循环判断时，**while 的条件判断**需要写成 **left < right**
            - 由于我们的区间定义意味着区间的最后一个元素：right 所在的元素是被检查过且不是目标值的
            - 当区间长度为2时，只有1个元素尚未被检查 ⇒ 此时 left < right 应该是最后一次检查
        - 注意 right 的初始值定义为 nums.size() 而不是 size() - 1，因为我们不包含右边界
            - 此时可能会造成疑问，因为 right 超出了数组的大小， 然而只要把这种初始情况看作是已经经历过一次比较后的中间状态，就能够理解了（想象right此时有一个幻影数，因为是右开，所以我们不在乎它）
        - **左闭右开答案写法:**
            
            ```cpp
            class Solution {
            public:
                int search(vector<int>& nums, int target) {
                    int left = 0, right = nums.size();
                    int mid;
                    while(left < right) {
                        mid = left + (right - left) / 2;
                        if(nums[mid] == target) {
                            return mid;
                        }
                        else if(nums[mid] < target) {
                            left = mid + 1;
                        }
                        else {
                            right = mid;
                        }
                    }
                    return -1;
                }
            };
            ```
            

## 27 Remove Element

### First time code:

- Brute Force
    
    ```cpp
    class Solution {
    public:
        int removeElement(vector<int>& nums, int val) {
            int cnt = 0;
            for(int i=0 ;i<nums.size(); ++i) {
                if(nums[i] == val) {
                    nums.erase(nums.begin() + i);
                    nums.push_back(val+1);
                    ++cnt;
                    --i;
                }
            }
            return nums.size() - cnt;
        }
    };
    ```
    
- Two Pointers
    
    ```cpp
    class Solution {
    public:
        int removeElement(vector<int>& nums, int val) {
            if(nums.size() == 0)
                return 0;
            int left = 0, right = nums.size() - 1;
            while(left < right) {
                if(nums[left] == val) {
                    nums[left] = nums[right];
                    nums[right] = val;
                    --right;
                    continue;
                }
                ++left;
            }
            if(nums[right] != val)
                return right + 1;
            return right;
        }
    };
    ```
    

---

- 解题思路：双指针（快慢指针）
    - 快指针用来寻找新数组所需要的元素
    - 慢指针用来寻找快指针找到的元素的位置
- 通过快指针指向的数来判断是否是新数组需要的数字
    - 如果是需要的数字，替换该数字到慢指针的位置
    - 如果不是，快指针继续向前
- 双指针答案
    
    ```cpp
    class Solution {
    public:
        int removeElement(vector<int>& nums, int val) {
            int slow = 0;
            for(int fast = 0; fast < nums.size(); ++fast) {
                if(nums[fast] != val) {
                    nums[slow] = nums[fast];
                    ++slow;
                }
            }
            return slow;
        }
    };
    ```