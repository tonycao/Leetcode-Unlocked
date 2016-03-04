## Largest BST Subtree

>**Questions**

Given a binary tree, find the largest subtree which is a Binary Search Tree (BST), where largest means subtree with largest number of nodes in it.

>Note:

A subtree must include all of its descendants.
Here's an example:
```
    10
    / \
   5  15
  / \   \
 1   8   7
```
The Largest BST Subtree in this case is the highlighted one.
The return value is the subtree's size, which is 3.


>Show Hint:

1. You can recursively use algorithm similar to 98. Validate Binary Search Tree at each node of the tree, which will result in O(nlogn) time complexity.

>Follow up:

Can you figure out ways to solve it with O(n) time complexity?


>**Solutions**

```c++

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

struct Data {
    int size;
    int lower;
    int upper;
    bool isBST;
    Data() : size(0), lower(INT_MAX), upper(INT_MIN), isBST(false){}
};

class Solution {
public:
    int largestBSTSubtree(TreeNode *root) {
        if (root == nullptr) {
            return 0;
        }

        largestBSTHelper(root);
        return max;
    }
private:
    Data largestBSTHelper(TreeNode *root) {
        Data curr;
        if (root == nullptr) {
            curr.isBST = true;
            return curr;
        }

        Data left = largestBSTHelper(root.left);
        Data right = largestBSTHelper(root.right);

        curr.lower = min(root->val, min(left->lower, right->lower));
        curr.upper = min(root->val, min(left->upper, right->upper));

        if (left.isBST && root->val > left->upper && right.isBST && root->val < right->lower) {
            curr.size = left->size + right->size + 1;
            cur.isBST = true;
            max = max(max, curr.size);
        } else {
            curr.size = -;
        }

        return curr;
    }
    
    int max = 0;

};


```
