# Day8 字符串 Part1

## 344 Reverse String

### First Time Code

```jsx
class Solution {
public:
    void reverseString(vector<char>& s) {
        char *left = &s[0], *right = &s[s.size()-1];
        for(int i=0; i<s.size()/2; ++i) {
            char tmp = *right;
            *right = *left;
            *left = tmp;
            ++left;
            --right;
        }
        return;
    }
};
```

---

- 字符串反转可以通过双指针从两侧向中间收敛，每收敛一次就互换字符串

## 541 Reverse String II

### First Time Code

```jsx
class Solution {
public:
    string reverseStr(string s, int k) {
        if (k == 1)
            return s;
        if (s.length() <= k) {
            reverse(s.begin(), s.end());
            return s;
        }
        string sub = s;
        string ans = "";
        int i = 0;
        while (sub.length() >= k) {
            string s1 = sub.substr(0, k);
            if (i % 2 == 0) {
                reverse(s1.begin(), s1.end());
            }
            ans += s1;
            sub = sub.substr(k);
            ++i;
        }     
        if (i % 2 == 0) {
            reverse(sub.begin(), sub.end());
        }
        ans += sub;
        return ans;
    }
};
```

- 主要的思路是对整个字符串进行处理，反转前 `k` 个并原封不动将后 k 个放入答案字符串

---

- 利用 `for` 循环的特性，我们可以每次递增 `2k` 步，然后反转当前位置往后的 `k` 个字符
- 非常简洁地解决了递增 `2k` 步的问题
- 解题代码
    
    ```jsx
    class Solution {
    public:
        string reverseStr(string s, int k) {
            for(int i=0; i<s.length(); i += 2*k) {
                if(s.length() - i >= k)
                    reverse(s.begin() + i, s.begin() + i + k);
                else {
                    reverse(s.begin() + i, s.end());
                }
            }
            return s;
        }
    };
    ```
    

## Karma 替换数字

### First Time Code

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string str;
    cin >> str;
    
    string ans = "";
    for(int i=0; i<str.length(); ++i) {
        if(str[i] >= '0' && str[i] <= '9')
            ans += "number";
        else
            ans += str[i];
    }
    cout << ans << endl;
    return 0;
}
```

- 使用额外空间时比较简单的一道题

---

- 如果不使用辅助空间，我们需要对给出的字符串进行变更，此时问题就变得困难了
- 在不使用辅助空间的情况下，需要预先扩容至最终大小，然后从后向前地填充字符
- 解题代码
    
    ```cpp
    #include <iostream>
    using namespace std;
    int main() {
        string s;
        while (cin >> s) {
            int sOldIndex = s.size() - 1;
            int count = 0; // 统计数字的个数
            for (int i = 0; i < s.size(); i++) {
                if (s[i] >= '0' && s[i] <= '9') {
                    count++;
                }
            }
            // 扩充字符串s的大小，也就是将每个数字替换成"number"之后的大小
            s.resize(s.size() + count * 5);
            int sNewIndex = s.size() - 1;
            // 从后往前将数字替换为"number"
            while (sOldIndex >= 0) {
                if (s[sOldIndex] >= '0' && s[sOldIndex] <= '9') {
                    s[sNewIndex--] = 'r';
                    s[sNewIndex--] = 'e';
                    s[sNewIndex--] = 'b';
                    s[sNewIndex--] = 'm';
                    s[sNewIndex--] = 'u';
                    s[sNewIndex--] = 'n';
                } else {
                    s[sNewIndex--] = s[sOldIndex];
                }
                sOldIndex--;
            }
            cout << s << endl;       
        }
    }
    ```
    

## [151 Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string)

### First Time Code

```cpp
class Solution {
public:
    string reverseWords(string s) {
        string ans = "";
        for (int i = 0; i < s.length(); ++i) {
            if (s[i] == ' ') {
                if (ans != "" && ans[0] != ' ') {
                    ans = s[i] + ans;
                }
            } else {
                int k = i + 1;
                while (k < s.length() && s[k] != ' ') {
                    ++k;
                }
                string word = s.substr(i, k - i);
                ans = " " + word + ans;
                i = k;
            }
        }
        if (ans[0] == ' ')
            ans = ans.substr(1);
        return ans;
    }
};
```

- 解这道题的思路在于如何处理字符串和添加空格

---

- 优化的代码依旧是十分简洁，不使用额外的存储空间，使用 `O(1)` 空间解决问题
    - 首先去除字符串前后的多余空格
    - 然后反转整个字符串
    - 最后针对字符串中的每个单词，反转该单词
- 解题代码
    
    ```cpp
    class Solution {
    public:
        void reverse(string& s, int start, int end){ //翻转，区间写法：左闭右闭 []
            for (int i = start, j = end; i < j; i++, j--) {
                swap(s[i], s[j]);
            }
        }
    
        void removeExtraSpaces(string& s) {//去除所有空格并在相邻单词之间添加空格, 快慢指针。
            int slow = 0;   //整体思想参考https://programmercarl.com/0027.移除元素.html
            for (int i = 0; i < s.size(); ++i) { //
                if (s[i] != ' ') { //遇到非空格就处理，即删除所有空格。
                    if (slow != 0) s[slow++] = ' '; //手动控制空格，给单词之间添加空格。slow != 0说明不是第一个单词，需要在单词前添加空格。
                    while (i < s.size() && s[i] != ' ') { //补上该单词，遇到空格说明单词结束。
                        s[slow++] = s[i++];
                    }
                }
            }
            s.resize(slow); //slow的大小即为去除多余空格后的大小。
        }
    
        string reverseWords(string s) {
            removeExtraSpaces(s); //去除多余空格，保证单词之间之只有一个空格，且字符串首尾没空格。
            reverse(s, 0, s.size() - 1);
            int start = 0; //removeExtraSpaces后保证第一个单词的开始下标一定是0。
            for (int i = 0; i <= s.size(); ++i) {
                if (i == s.size() || s[i] == ' ') { //到达空格或者串尾，说明一个单词结束。进行翻转。
                    reverse(s, start, i - 1); //翻转，注意是左闭右闭 []的翻转。
                    start = i + 1; //更新下一个单词的开始下标start
                }
            }
            return s;
        }
    };
    ```
    

## Karma 右旋转字符串

### First Time Code

```cpp
#include <iostream>
#include <string>

using namespace std;

int main() {
    int k;
    string str;
    cin >> k;
    cin >> str;
    
    if(k > str.length())
        k = k % str.length();
    
    string sub1 = str.substr(0, str.length() - k);
    string sub2 = str.substr(str.length() - k);
    
    cout << sub2 + sub1 << endl;
    return 0;
}
```

---

- 与力扣问题相同：[189. Rotate Array](https://leetcode.com/problems/rotate-array)
- 利用上一次 O(1) 的思想，我们可以对字符串进行整体反转
    - 此时后k位来到前方
    - 然后我们分别旋转前 `k` 个数和后 `n-k` 个数，得到最后的答案
- 解题代码
    
    ```cpp
    #include <iostream>
    #include <string>
    #include <algorithm>
    
    using namespace std;
    
    int main() {
        int k;
        string str;
        cin >> k;
        cin >> str;
        
        if(k > str.length())
            k = k % str.length();
        
        reverse(str.begin(), str.end());
        reverse(str.begin(), str.begin() + k);
        reverse(str.begin() + k, str.end());
        
        cout << str << endl;
        return 0;
    }
    ```