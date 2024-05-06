# Day20 二叉树 part4

## [2487. Remove Nodes From Linked List](https://leetcode.com/problems/remove-nodes-from-linked-list/?envType=daily-question&envId=2024-05-06)

```cpp
class Solution {
public:
    ListNode* reverse(ListNode* head) {
        ListNode* curr = head;
        ListNode* prev = nullptr;
        ListNode* next = head->next;

        while(next) {
            curr->next = prev;
            prev = curr;
            curr = next;
            next = next->next;
        }

        curr->next = prev;
        return curr;
    }

    ListNode* removeNodes(ListNode* head) {
        ListNode* newHead = reverse(head);
        ListNode* curr = newHead;
        while(curr && curr->next) {
            if(curr->val > curr->next->val) {
                ListNode* toDelete = curr->next;
                curr->next = curr->next->next;
                delete toDelete;
                toDelete = nullptr;
            }
            else {
                curr = curr->next;
            }
        }
        return reverse(newHead);
    }
};
```

## [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree)

```
class Solution {
public:
    int height(TreeNode* root) {
        if(!root)
            return 0;
        int lheight = height(root->left);
        int rheight = height(root->right);
        if(lheight == -1 || rheight == -1)
            return -1;
        else if(abs(lheight-rheight) > 1)
            return -1;
        else
            return max(lheight, rheight) + 1;
    }

    bool isBalanced(TreeNode* root) {
        return height(root) == -1 ? false : true;
    }
};
```

## [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths)

```cpp
class Solution {
public:
    vector<string> ans;

    void explore_path(TreeNode* root, string& path) {
        path += to_string(root->val);
        if(!root->left && !root->right) {
            ans.push_back(path);
            return;
        }
        else{
            if(root->left) {
                path += "->";
                explore_path(root->left, path);

                path = path.substr(0, path.length()-2-to_string(root->left->val).length());
            }
            if(root->right) {
                path += "->";
                explore_path(root->right, path);
                path = path.substr(0, path.length()-2-to_string(root->right->val).length());
            }
        }
    }

    vector<string> binaryTreePaths(TreeNode* root) {
        string path = "";
        explore_path(root, path);
        return ans;
    }
};
```

## [404. Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves)

```cpp
class Solution {
public:
    int sum = 0;
    void sum_left(TreeNode* root, bool isLeft) {
        if(!root->left && !root->right && isLeft) {
            sum += root->val;
        }
        else {
            if(root->left)
                sum_left(root->left, true);
            if(root->right)
                sum_left(root->right, false);
        }
    }

    int sumOfLeftLeaves(TreeNode* root) {
        sum_left(root, false);
        return sum;
    }
};
```