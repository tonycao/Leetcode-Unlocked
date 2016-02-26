## Inorder Successor

>**Question**

Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

>Note

If the given node has no in-order successor in the tree, return null.

>**Solution**

from [here](http://www.cnblogs.com/jcliBlogger/p/4829200.html)
There are just two cases:

The easier one: p has right subtree, then its successor is just the leftmost child of its right subtree;
The harder one: p has no right subtree, then a traversal is needed to find its successor.

**Traversal**: we start from the root, each time we see a node with val larger than `p -> val`, we know this node may be p's successor. So we record it in suc. Then we try to move to the next level of the tree: if `p -> val > root -> val`, which means p is in the right subtree, then its successor is also in the right subtree, so we update root = root -> right; if `p -> val < root -> val`, we update `root = root -> left` similarly; once we find `p -> val == root -> val`, we know we've reached at p and the current sucis just its successor.

```c++
class Solution {
public:
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        if (p -> right) return leftMost(p -> right);
        TreeNode* suc = NULL;
        while (root) {
            if (p -> val < root -> val) {
                suc = root;
                root = root -> left;
            }
            else if (p -> val > root -> val)
                root = root -> right;
            else break;
        }
        return suc;
    }
private:
    TreeNode* leftMost(TreeNode* node) {
        while (node -> left) node = node -> left;
        return node;
    }
};
```
Another solution:

```c++
class Solution {
public:
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        TreeNode* suc = NULL;
        while (root) {
            root = (root->val <= p->val) ? root->right : (suc = root)->left;
        }
        return suc;
    }

};
```
