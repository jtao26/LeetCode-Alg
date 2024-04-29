# Day14 二叉树 part1

## 二叉树的种类

- 满二叉树：叶子节点在同一层上，除叶子节点外的所有节点都有两个子节点
    - 一共有2^k-1个节点
- 完全二叉树：除了底层外，其余所有层都被节点填满，且最底层节点尽可能靠左
- 二叉搜索树：每个节点有对应的值，左子树的所有节点小于根节点，右子树的所有节点大于根节点的值，左、右子树也为二叉搜索树
- 平衡二叉搜索树(AVL树)：具有二叉搜索树的所有性质，另外其左右两个子树的高度差不超过1，并且左右子树也是平衡二叉搜索树

## 二叉树的存储方式

- 链式存储
    - 树的节点在内存中是不连续分布的，节点之间靠指针链接
- 顺序存储
    - 树的节点是连续分布的，以数组形式存储节点
        - 假设父节点下表为i，则它的左子树是i*2+1，右子树是i*2+2

## 二叉树的遍历方式

- 深度优先DFS
    - 前序遍历（递归法，迭代法）
    - 中序遍历（递归法，迭代法）
    - 后序遍历（递归法，迭代法）
- 广度优先BFS
    - 层次遍历（迭代法）

## 二叉树的定义

```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *l, TreeNode *r) : val(x), left(l), right(r) {}
};
```

## [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal)

```cpp
class Solution {
public:
    void preOrder(TreeNode* root, vector<int>& vec) {
        if(!root)
            return;
        vec.push_back(root->val);
        preOrder(root->left, vec);
        preOrder(root->right, vec);
    }

    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ans;
        preOrder(root, ans);
        return ans;
    }
};
```

## [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal)

```cpp
class Solution {
public:
    void post(TreeNode *root, vector<int>& vec) {
        if(!root)
            return;
        post(root->left, vec);
        post(root->right, vec);
        vec.push_back(root->val);
    }

    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> ans;
        post(root, ans);
        return ans;
    }
};
```

## [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal)

- 递归遍历

```cpp
class Solution {
public:
    void inOrder(TreeNode* root, vector<int>& ans) {
        if(!root)
            return;
        inOrder(root->left, ans);
        ans.push_back(root->val);
        inOrder(root->right, ans);
    }

    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        inOrder(root, ans);
        return ans;
    }
};
```

- 迭代遍历

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        stack<TreeNode*> s;
        TreeNode* curr = root;
        while(curr != nullptr || !s.empty()) {
            if(curr != nullptr) {
                s.push(curr);
                curr = curr->left;
            }
            else {
                curr = s.top();
                s.pop();
                ans.push_back(curr->val);
                curr = curr->right;
            }
        }
        return ans;
    }
};
```