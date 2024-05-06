# Day21 二叉树 part5

## [513. Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value)

```cpp
class Solution {
public:
    int maxdepth = 0;
    int ans = 0;
    void find_depth(TreeNode* root, int depth) {
        if(!root)
            return;
        if(!root->left && !root->right){
            if(depth > maxdepth){
                ans = root->val;
                maxdepth = depth;
            }
        }
        find_depth(root->left, depth + 1);
        find_depth(root->right, depth + 1);
    }
    int findBottomLeftValue(TreeNode* root) {
        ans = root->val;
        find_depth(root, 0);
        return ans;
    }
};
```

## [112. Path Sum](https://leetcode.com/problems/path-sum)

```cpp
class Solution {
public:
    bool calcSum(TreeNode* root, int target, int sum) {
        if(!root->left && !root->right) {
            if((sum += root->val) == target){
                return true;
            }
            return false;
        }
        bool subAns = false;
        if(root->left)
            subAns = calcSum(root->left, target, sum + root->val);
        if(root->right)
            subAns = subAns || calcSum(root->right, target, sum + root->val);
        return subAns;
    }

    bool hasPathSum(TreeNode* root, int targetSum) {
        if(!root)
            return false;    
        return calcSum(root, targetSum, 0);
    }
};
```

## [113. Path Sum II](https://leetcode.com/problems/path-sum-ii)

```cpp
class Solution {
public:
    vector<vector<int>> ans;
    void rec_find(TreeNode* root, int target, vector<int>& v, int sum) {
        v.push_back(root->val);
        if(!root->left && !root->right){
            if(sum + root->val == target){
                ans.push_back(v);
                return;
            }
        }
        if(root->left) {
            rec_find(root->left, target, v, sum + root->val);
            v.pop_back();
        }
        if(root->right){
            rec_find(root->right, target, v, sum + root->val);
            v.pop_back();
        }
    }

    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        vector<int> v;
        if(root)
            rec_find(root, targetSum, v, 0);
        return ans;
    }
};
```

## [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal)

```cpp
class Solution {
public:
    TreeNode* recur(vector<int>& preorder, vector<int>& inorder) {
        if(preorder.size() == 0)
            return nullptr;
        
        int rootVal = preorder[0];
        TreeNode* root = new TreeNode(rootVal);

        if(preorder.size() == 1)
            return root;
        
        int idx;
        for(idx = 0; idx<inorder.size(); ++idx) {
            if(inorder[idx] == rootVal)
                break;
        }

        vector<int> leftInOrder(inorder.begin(), inorder.begin() + idx);
        vector<int> rightInOrder(inorder.begin() + idx + 1, inorder.end());

        for(int i=1; i<preorder.size(); ++i) {
            preorder[i-1] = preorder[i];
        }
        preorder.resize(preorder.size()-1);

        vector<int> leftPreOrder(preorder.begin(), preorder.begin() + leftInOrder.size());
        vector<int> rightPreOrder(preorder.begin() + leftInOrder.size(), preorder.end());

        root->left = recur(leftPreOrder, leftInOrder);
        root->right = recur(rightPreOrder, rightInOrder);
        return root;
    }

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return recur(preorder, inorder);
    }
};
```

## **[106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal)**

```cpp
class Solution {
public:

    TreeNode* recur(vector<int>& inorder, vector<int>& postorder) {
        if(postorder.size() == 0)
            return nullptr;
        
        int rootVal = postorder[postorder.size()-1];
        TreeNode* root = new TreeNode(rootVal);

        if(postorder.size() == 1)
            return root;

        // split inorder
        int inSplit = 0;
        for(int i=0; i<inorder.size(); ++i) {
            if(inorder[i] == root->val){
                inSplit = i;
                break;
            }
        }

        vector<int> leftInOrder(inorder.begin(), inorder.begin() + inSplit);
        vector<int> rightInOrder(inorder.begin() + inSplit + 1, inorder.end());

        postorder.resize(postorder.size()-1);
        vector<int> leftPostOrder(postorder.begin(), postorder.begin() + leftInOrder.size());
        vector<int> rightPostOrder(postorder.begin() + leftInOrder.size(), postorder.end());

        root->left = recur(leftInOrder, leftPostOrder);
        root->right = recur(rightInOrder, rightPostOrder);

        return root;
    }

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        return recur(inorder, postorder);
    }
};
```