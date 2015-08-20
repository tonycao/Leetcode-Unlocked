## Verify preorder sequence of Binary Search Tree

>**Question**

You have an array of preorder traversal of Binary Search Tree ( BST). Your program should verify whether it is a correct sequence or not.

Input: `Array of Integer [ Pre-order BST ] `
Output: `String “Yes” or “No”`

>**Solution 1**

divide and conquer

```c++
bool verifyPreorder(vector<int>& preorder) {
    int n = preorder.size();
    return helper(preorder, 0, n-1, INT_MIN, INT_MAX);
}
bool helper(vector<int>& preorder, int s, int e, int lb, int ub){
    if(s>=e) return true;
    int r = preorder[s];
    int i = s;
    while(i<=e && preorder[i]<=r){
        if(preorder[i]<lb || preorder[i]>ub) return false;
        i++;
    }
    return helper(preorder, s+1, i-1, lb, r-1) &&
           helper(preorder, i, e, r+1, ub);
}

```

>**Solution 2**

simulate the traversal, keeping a stack of nodes (just their values) of which we're still in the left subtree. If the next number is smaller than the last stack value, then we're still in the left subtree of all stack nodes, so just push the new one onto the stack. But before that, pop all smaller ancestor values, as we must now be in their right subtrees (or even further, in the right subtree of an ancestor). Also, use the popped values as a lower bound, since being in their right subtree means we must never come across a smaller number anymore.

```c++
bool verifyPreorder(vector<int>& preorder) {
    int low = INT_MIN; // lower bound
    stack<int> path;
    for (int p : preorder) {
        if (p < low) {  
            return false;
        }
        while (!path.empty() && p > path.top()) {
            low = path.top();
            path.pop();
        }
        path.push(p);
    }
    return true;
}

```

>other solution  [here](http://www.snippetexample.com/2015/03/verify-preorder-sequence-in-binary-search-tree/)
