# Day9 字符串 Part2

## KMP 算法

- KMP算法能够根据已经匹配的字符串找到下一个匹配点，而无需从头开始重新遍历
    - 原因在于KMP算法使用了最长相同前缀字符串，在遇到匹配失败时能够快速找到下一个匹配位置
- 计算前缀表 `next`
    - 给定一个字符串 `ABCDABD` ，计算其前缀表
        
        ```cpp
        void getNext(int* next, string str){
            // 初始化，其中 i 代表前缀长度，同时也是前缀末尾位置
            int i = 0;
            next[0] = 0;
            
            // 初始化j，j 代表后缀的结束位置
            for(int j=1; j<str.length(); ++j) {
        			  // 前后缀不相等，此时后退 i，到位置 next[i-1]
        			  while(i > 0 && s[i] != s[j]){
        					  i = next[i-1];
        			  }
        			  // 前后缀相等，此时相同前后缀长度+1，即 i+1
        			  if(s[i] == s[j]){
        					  ++i;
        			  }
        			  next[j] = i;
            }
        }
        ```
        
- 利用已经计算好的前缀表，我们可以使用KMP算法开始匹配字符串了
    
    ```cpp
    int strStr(string haystack, string needle) {
            if(needle.length() > haystack.length()) {
                return -1;
            }
            vector<int> next(needle.length());
            getNext(&next[0], needle);
    
            // 匹配时模板字符串的指针位置
            int j=0;
            // 遍历字符串
            for(int i=0; i<haystack.length(); ++i) {
    		        // 遇到当前字符串字符和模板不一致，根据前缀数组回退
                while(j>0 && haystack[i] != needle[j]) {
                    j = next[j-1];
                }
                // 如果回退后相等了，我们继续增加模板的指针位置
                if(haystack[i] == needle[j]) {
                    ++j;
                }
                // 匹配结束，返回字符串的起始位置
                if(j == needle.length()) {
                    return (i - needle.length() + 1);
                }
            }
            return -1;
        }
    ```