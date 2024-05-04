# Day15 二叉树 part2

## 二叉树的层序遍历

## [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal)

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if(!root){
            return ans;
        }
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()) {
            vector<int> lv;
            int size = q.size();
            for(int i=0; i<size; ++i) {
                TreeNode *curr = q.front();
                q.pop();
                lv.push_back(curr->val);
                if(curr->left)
                    q.push(curr->left);
                if(curr->right)
                    q.push(curr->right);
            }
            ans.push_back(lv);
        }
        return ans;
    }
};
```

## [107. Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii)

```cpp
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> ans;
        if(!root){
            return ans;
        }
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()) {
            vector<int> lv;
            int size = q.size();
            for(int i=0; i<size; ++i) {
                TreeNode *curr = q.front();
                q.pop();
                lv.push_back(curr->val);
                if(curr->left)
                    q.push(curr->left);
                if(curr->right)
                    q.push(curr->right);
            }
            ans.push_back(lv);
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

## [199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view)

```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        queue<TreeNode*> q;
        vector<int> ans;
        if(!root)
            return ans;
        q.push(root);
        while(!q.empty()) {
            int size = q.size();
            for(int i=0; i<size; ++i) {
                TreeNode* curr = q.front();
                q.pop();
                if(i == size-1)
                    ans.emplace_back(curr->val);
                if(curr->left)
                    q.push(curr->left);
                if(curr->right)
                    q.push(curr->right);
            }
        }
        return ans;
    }
};
```

## [637. Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree)

```cpp
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        queue<TreeNode*> q;
        vector<double> ans;

        q.push(root);
        while(!q.empty()) {
            int size = q.size();
            double sum = 0;
            for(int i=0; i<size; ++i) {
                TreeNode* curr = q.front();
                q.pop();
                sum += curr->val;
                if(curr->left)
                    q.push(curr->left);
                if(curr->right)
                    q.push(curr->right);
            }
            ans.emplace_back(sum/size);
        }
        return ans;
    }
};
```

## [429. N-ary Tree Level Order Traversal](https://leetcode.com/problems/n-ary-tree-level-order-traversal)

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        queue<Node*> q;
        vector<vector<int>> ans;
        if(!root)
            return ans;
        q.push(root);
        while(!q.empty()) {
            int size = q.size();
            vector<int> lv;
            for(int i=0; i<size; ++i) {
                Node* curr = q.front();
                q.pop();
                lv.emplace_back(curr->val);
                for(auto child : curr->children) {
                    if(child)
                        q.push(child);
                }
            }
            ans.emplace_back(lv);
        }
        return ans;
    }
};
```

## [515. Find Largest Value in Each Tree Row](https://leetcode.com/problems/find-largest-value-in-each-tree-row)

```cpp
class Solution {
public:
    vector<int> largestValues(TreeNode* root) {
        queue<TreeNode*> q;
        vector<int> ans;
        if(!root)
            return ans;
        q.push(root);
        while(!q.empty()) {
            int size = q.size();
            int max_val = INT_MIN;
            for(int i=0; i<size; ++i) {
                TreeNode* curr = q.front();
                q.pop();
                max_val = max(max_val, curr->val);
                if(curr->left)
                    q.push(curr->left);
                if(curr->right)
                    q.push(curr->right);
            }
            ans.emplace_back(max_val);
        }
        return ans;
    }
};
```

## [116. Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node)

```cpp
class Solution {
public:
    Node* connect(Node* root) {
        queue<Node*> q;
        if (!root)
            return root;
        q.push(root);
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                Node* curr = q.front();
                q.pop();
                if (!q.empty() && i < size - 1) {
                    Node* nextNode = q.front();
                    curr->next = nextNode;
                } else {
                    curr->next = nullptr;
                }
                if (curr->left)
                    q.push(curr->left);
                if (curr->right)
                    q.push(curr->right);
            }
        }
        return root;
    }
};
```

## [117. Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii)

```cpp
class Solution {
public:
    Node* connect(Node* root) {
        queue<Node*> q;
        if (!root)
            return root;
        q.push(root);
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                Node* curr = q.front();
                q.pop();
                if (!q.empty() && i < size - 1) {
                    Node* nextNode = q.front();
                    curr->next = nextNode;
                } else {
                    curr->next = nullptr;
                }
                if (curr->left)
                    q.push(curr->left);
                if (curr->right)
                    q.push(curr->right);
            }
        }
        return root;
    }
};
```

## [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree)

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        queue<TreeNode*> q;
        int ans = 0;
        if(!root)
            return 0;
        q.push(root);
        while(!q.empty()) {
            ++ans;
            int size = q.size();
            for(int i=0; i<size; ++i) {
                TreeNode* curr = q.front();
                q.pop();
                if(curr->left)
                    q.push(curr->left);
                if(curr->right)
                    q.push(curr->right);
            }
        }
        return ans;
    }
};
```

## [111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree)

```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(!root)
            return 0;
        queue<TreeNode*> q;
        q.push(root);
        int depth = 1;
        while(!q.empty()) {
            int size = q.size();
            for(int i=0; i<size; ++i) {
                TreeNode* curr = q.front();
                q.pop();
                if(!curr->left && !curr->right){
                    return depth;
                }
                if(curr->left)
                    q.push(curr->left);
                if(curr->right)
                    q.push(curr->right);
            }
            ++depth;
        }
        return depth;
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