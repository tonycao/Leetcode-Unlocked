## Count Univalue Subtrees

>**Question**

Given a complete binary tree, count the number of nodes.
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

Universal value binary tree means all value in that tree is the same.

```c++
//C++: 4ms
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int countUnivalSubtrees(TreeNode* root) {
        int cnt = 0;
        helper(root, cnt);
        return cnt;
    }
    bool helper(TreeNode* root, int &cnt){
        if(!root) return true;
        bool left = helper(root->left, cnt);
        bool right = helper(root->right, cnt);
        if(left && right &&
            root->left && root->left->val == root->val &&
            root->right && root->right->val == root->val) {
            cnt++;
            return true;
        }

        return false;
    }
};
```
