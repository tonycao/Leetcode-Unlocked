## Binary Tree Vertical Order Traversal

>**Question**

Given a binary tree, return the vertical order traversal of its nodes' values. (ie, from top to bottom, column by column).
If two nodes are in the same row and column, the order should be from left to right.

**Examples:**

Given binary tree `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
return its vertical order traversal as:
```
[
  [9],
  [3,15],
  [20],
  [7]
]
```
Given binary tree `[3,9,20,4,5,2,7]`,
```
    _3_
   /   \
  9    20
 / \   / \
4   5 2   7
```
return its vertical order traversal as:
```
[
  [4],
  [9],
  [3,5,2],
  [20],
  [7]
]
```

>**Solution**

BFS

```c++
Class Solution {
public:
    vector<vector<int>> verticalOrder(TreeNode* root) {
        vector<vector<int>> ret;
        if (root == nullptr) {
            return ret;
        }

        ret.resize(1);
        int l = 0, r = 0;

        queue<pair<TreeNode*, int>> que;//node, col
        que.push(pair<TreeNode*, int>(root, 0));
        while(!que.empty()){
            TreeNode *node = que.front().first;
            int col = que.front().second;
            que.pop();
            if(l<=col && col<=r){
                int i = col-l;
                ret[i].push_back(node->val);
            }else if(col<l){
                l--;
                ret.insert(ret.begin(), vector<int>());
                ret.front().push_back(node->val);
            }else{
                r++;
                ret.insert(ret.end(), vector<int>());
                ret.back().push_back(node->val);
            }
            if(node->left) que.push(pair<TreeNode*, int>(node->left, col-1));
            if(node->right) que.push(pair<TreeNode*, int>(node->right, col+1));
        }
        return ret;
    }
};
```
another solution:
```c++
class Solution {
public:
	vector<vector<int>> verticalOrder(TreeNode* root) {
	    vector<vector<int>> result;
	    if (!root)
	        return result;
		map<int, pair<int, int>> pos;
		map<int,vector<int>> mp;
		//do bfs to traverse first
		queue<TreeNode*> que;
		que.push(root);
		mp[0].push_back(root->val);
		pos[root->val] = make_pair(0, 0);
		while (!que.empty()) {
			int size = que.size();
			for (int i = 0; i < size; i++) {
				TreeNode* cur = que.front();
				que.pop();
				int x = pos[cur->val].first;
				int y = pos[cur->val].second;
				if (cur->left) {
					que.push(cur->left);
					pos[cur->left->val] = make_pair(x + 1, y - 1);
					mp[y-1].push_back(cur->left->val);
				}
				if (cur->right) {
					que.push(cur->right);
					pos[cur->right->val] = make_pair(x + 1, y + 1);
					mp[y+1].push_back(cur->right->val);
				}
			}
		}

		//generate result
		for (auto &pair : mp) {
			result.push_back(pair.second);
		}
		return result;
	}
};

```
