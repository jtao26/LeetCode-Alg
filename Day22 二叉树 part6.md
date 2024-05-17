# Day22 二叉树 part6

## [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree)

```cpp
class Solution {
public:
    int findMaxIdx(vector<int>& arr) {
        int idx = 0, maxVal = -1;
        for(int i=0; i<arr.size(); ++i) {
            if(arr[i]>maxVal) {
                maxVal = arr[i];
                idx = i;
            }

        }
        return idx;
    }

    TreeNode* build(vector<int> arr) {
        if(arr.size() == 0)
            return nullptr;
        
        int idx = findMaxIdx(arr);

        TreeNode* root = new TreeNode(arr[idx]);

        if(arr.size() == 1)
            return root;

        vector<int> left(arr.begin(), arr.begin() + idx);
        vector<int> right(arr.begin()+ idx+1, arr.end());

        root->left = build(left);
        root->right = build(right);

        return root;
        
    }

    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        return build(nums);
    }
};
```

## [617. Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees)

```cpp
class Solution {
public:
    int findMaxIdx(vector<int>& arr) {
        int idx = 0, maxVal = -1;
        for(int i=0; i<arr.size(); ++i) {
            if(arr[i]>maxVal) {
                maxVal = arr[i];
                idx = i;
            }

        }
        return idx;
    }

    TreeNode* build(vector<int> arr) {
        if(arr.size() == 0)
            return nullptr;
        
        int idx = findMaxIdx(arr);

        TreeNode* root = new TreeNode(arr[idx]);

        if(arr.size() == 1)
            return root;

        vector<int> left(arr.begin(), arr.begin() + idx);
        vector<int> right(arr.begin()+ idx+1, arr.end());

        root->left = build(left);
        root->right = build(right);

        return root;
        
    }

    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        return build(nums);
    }
};
```

## [700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree)

```cpp
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        TreeNode* curr = root;
        while(curr){
            if(val > curr->val) {
                curr = curr->right;
            }
            else if(val < curr->val) {
                curr = curr->left;
            }
            else{
                break;
            }
        }
        return curr;
    }
};
```

## [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree)

```cpp
class Solution {
public:
    vector<int> arr;

    void mid_traverse(TreeNode* root) {
        if(!root)
            return;
        mid_traverse(root->left);
        arr.emplace_back(root->val);
        mid_traverse(root->right);
        return;
    }

    bool isValidBST(TreeNode* root) {
        mid_traverse(root);
        if(arr.size() <= 1)
            return true;
        for(int i=1; i<arr.size(); ++i){
            if(arr[i] <= arr[i-1])
                return false;
        }
        return true;
    }
};
```