# Day19 二叉树 part3

## [881. Boats to Save People](https://leetcode.com/problems/boats-to-save-people/?envType=daily-question&envId=2024-05-04)

- 双指针

```cpp
class Solution {
public:
    static bool cmp(int a1, int a2){
        return a1 > a2;
    }

    int numRescueBoats(vector<int>& people, int limit) {
        sort(people.begin(), people.end(), cmp);

        int ans = 0;
        int right = people.size()-1, left = 0;
        int currWeight = 0;
        int count = 0;
        while(left <= right) {
            while(left < people.size() && left <= right && currWeight < limit) {
                currWeight += people[left];
                ++left;
                ++count;
            }
            while(count > 2 || currWeight > limit){
                --left;
                --count;
                currWeight -= people[left];
            }
            while(right >= 0 && left <= right && currWeight < limit) {
                currWeight += people[right];
                --right;
                ++count;
            }
            while(count > 2 || currWeight > limit){
                ++right;
                currWeight -= people[right];
                --count;
            }
            
            ++ans;
            count = 0;
            currWeight = 0;
        }
        return ans;
    }
};
```

## [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree)

- 递归解法，递归地反转每个子树，自下而上反转整棵树
    
    ```cpp
    class Solution {
    public:
        void reverse(TreeNode* root) {
            if(!root)
                return;
            reverse(root->left);
            reverse(root->right);
            TreeNode* tmp = root->left;
            root->left = root->right;
            root->right = tmp;
        }
    
        TreeNode* invertTree(TreeNode* root) {
            reverse(root);
            return root;
        }
    };
    ```
    

## [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree)

```cpp
class Solution {
public:
    bool compare(TreeNode* left, TreeNode* right) {
        if(!left && !right) return true;
        else if(!left && right)  return false;
        else if(left && !right)  return false;

        return left->val == right->val 
            && compare(left->left, right->right) 
            && compare(left->right, right->left);
    }

    bool isSymmetric(TreeNode* root) {
        return compare(root->left, root->right);
    }
};
```