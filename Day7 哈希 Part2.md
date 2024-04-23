# Day7 哈希表 Part2

## 454 4Sum II

### First Time Code

```cpp
// Nope 第一次没想出来，看了题解做的
```

---

- 本题的知识点在于如何灵活运用哈希表的特性，以及结合加法交换律
- 通过记录前两个数组 `nums1` 和 `nums2` 之和并记录到哈希表中，我们可以在计算后两个数组和时利用它
    - 这里要注意前两个数组是无所谓哪个数组的，即使前两个数组选 `nums1` 和 `nums4` 也是一样的，加法交换律
- 因此，对于 `nums1` 和 `nums2` 中出现的元素和，我们将和作为键添加到哈希表中，将出现的次数作为值存储
- 在计算后两个数组和时，因为总和为 0，我们遍历两数组中数字之和，在哈希表中寻找已经由先前遍历建立的条目，如果找到，则它们的和一定为 0。
- 由于可能出现多种组合，即键为 4 可以是 1+3 或 2+2，因此我们还要考虑数量。
- 最后的结果是这些数量的累加
    
    ```cpp
    class Solution {
    public:
        int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
            unordered_map<int, int> mp;
            for(int a : nums1) {
                for(int b : nums2) {
                    mp[a+b]++;
                }
            }
            int ans = 0;
            for(int c : nums3) {
                for(int d : nums4) {
                    ans += mp[0 - (c + d)];
                }
            }
            return ans;
        }
    };
    ```
    

## 383 Ransom Note

### First Time Code

```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        if(magazine.length() < ransomNote.length())
            return false;
        
        unordered_map<int, int> mp;
        for(int a : magazine) {
            ++mp[a];
        }
        for(int a : ransomNote) {
            --mp[a];
            if(mp[a] < 0)
                return false;
        }
        return true;
    }
};
```

---

- 通过哈希表记录可用的字母数量，然后遍历ransomNote需要的字母，如果数量小于0，则表示无法构建Note
- 可以用长度为26的数组优化，索引变为 `char - 'a'`
- 解决代码
    
    ```cpp
    class Solution {
    public:
        bool canConstruct(string ransomNote, string magazine) {
            if(magazine.length() < ransomNote.length())
                return false;
            
            vector<int> count(26, 0);
            for(char a : magazine) {
                count[a-'a']++;
            }
            for(char a : ransomNote) {
                count[a-'a']--;
                if(count[a-'a'] < 0)
                    return false;
            }
            return true;
        }
    };
    ```
    

## 15 3Sum

- 双指针法
    - 既然题目要求我们在数组中寻找满足和为 0 的元素，使用双指针法，我们能根据一定的规则移动双指针，从而测试不同组合
        - 首先需要对数组进行排序，否则无法使用双指针
        - 遍历数组，对于每个遍历到的元素，我们从该元素的下一个位置开始，使用双指针枚举他们的和，
            - 如果三数之和大于0，由于先前已经对数组进行排序，因此 right 需要左移，从而减小和
            - 如果小于 0，则需要 left 右移增大和
            - 如果为 0 则将组合录入最后的结果
                - 录入结果后，我们需要对结果进行去重，为何？
                    - 考虑这个例子 `[0,2,2,-2,-2]` 其中仅有 `[-2,0,2]` 一种解，当`i=0`时`left = 2, right = -2` 此时录入结果，不难注意到还有此时窗口还有元素可以考虑，因此我们缩小窗口，即 `++left` 和 `--right` 。此时组合仍为`[-2,0,2]` ，但因为我们不需要重复的答案，此答案应被舍弃。
                    - 不难发现此时对于`right`来说，当前的`2`和他左侧的`2`对于和的贡献是一样的，因此对于和来讲，`right`**指向当前位置还是它左边的位置是不重要的**，因此，我们将`right`左移。
                    - 一样的，对于`left`来说，当前`-2`和右侧`-2`也是一样的，我们右移`left` ，如此我们可以便对已经出现的结果去重，因为数组已经排序，后续不会再出现`-2`或者`2`
            - 除了上述之外，对于元素`i`也应当去重，理由相同。但此时我们不再检查`i`的下一个，而是检查`i`的上一个元素是否和当前`i`相同，为了避免漏掉潜在的结果，例如`[-1,-1,2]`
    - 本题的难点在于如何为结果去重，不仅要考虑外层 `i` 的循环去重，也要考虑内层 `left` 和 `right` 对应的元素的去重
- 题解代码
    
    ```cpp
    class Solution {
    public:
        vector<vector<int>> threeSum(vector<int>& nums) {
            sort(nums.begin(), nums.end());
            if (nums[0] > 0)
                return {};
            vector<vector<int>> ans;
            for (int i = 0; i < nums.size(); ++i) {
                if (i > 0 && nums[i] == nums[i - 1])
                    continue;
    
                int left = i + 1, right = nums.size() - 1;
                while (right > left) {
                    if (nums[i] + nums[left] + nums[right] > 0) {
                        --right;
                    } else if (nums[i] + nums[left] + nums[right] < 0) {
                        ++left;
                    } else {
                        ans.push_back({nums[i], nums[left], nums[right]});
                        while (right > left && nums[right] == nums[right - 1])
                            --right;
                        while (right > left && nums[left] == nums[left + 1])
                            ++left;
                        ++left;
                        --right;
                    }
                }
            }
            return ans;
        }
    };
    ```
    

## 18 4Sum

- 基于三数之和的思想，求解四数和则需要我们使用两个for循环固定前两个数字，并通过双指针搜寻后两个数字
- 这种方式能够成功地将复杂度为`O(n^4)`的算法降低为`O(n^3)`
    - 在进行for循环时要处理剪枝和去重操作
    - 在三数和中，我们在 nums[i] > 0 时从循环中退出，因为排序后的数组无论如何在 nums[i] > 0 后一定无法找到符合条件的组合
    - 在四数和中当 `nums[i] > target` 时，我们无法因此退出，因为存在负数的可能，因此我们需要追加判断`nums[i] >= 0` ，确保不会出现负数相加和更小的情况。
        - 如`target = -5`, `nums = [-4, -1, 0, 0]` ，此时nums是一个满足条件的组合，但满足`nums[i] > target`
    - 后续的去重操作与三数和一样，我们检查`nums[i] == nums[i-1]` 来跳过重复的元素
    - 第二层循环中的基本操作不变，不过此时我们将 `nums[i]+nums[j]` 看作一个整体，判断其是否大于`target`且大于 `0` ，从而进行剪枝
- 另外一个需要注意的点是由于最后的四数和可能溢出`int`，需要转为`long`比较
    
    ```cpp
    class Solution {
    public:
        vector<vector<int>> fourSum(vector<int>& nums, int target) {
            sort(nums.begin(), nums.end());
            vector<vector<int>> ans;
            for (int i = 0; i < nums.size(); ++i) {
                if (nums[i] > target && nums[i] >= 0) {
                    break;
                }
                if (i > 0 && nums[i] == nums[i - 1]) {
                    continue;
                }
                for (int j = i + 1; j < nums.size(); ++j) {
                    if (nums[i] + nums[j] > target && nums[i] + nums[j] >= 0) {
                        break;
                    }
                    if (j > i + 1 && nums[j] == nums[j - 1]) {
                        continue;
                    }
                    int left = j + 1, right = nums.size() - 1;
                    while (right > left) {
                        if ((long) nums[i] + nums[j] + nums[left] + nums[right] > target) {
                            --right;
                        } else if ((long) nums[i] + nums[j] + nums[left] + nums[right] < target) {
                            ++left;
                        } else {
                            ans.push_back(
                                {nums[i], nums[j], nums[left], nums[right]});
                            while (right > left && nums[left] == nums[left + 1]) {
                                ++left;
                            }
                            while (right > left && nums[right] == nums[right - 1]) {
                                --right;
                            }
                            ++left;
                            --right;
                        }
                    }
                }
            }
            return ans;
        }
    };
    ```