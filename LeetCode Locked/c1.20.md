## 	Closest Binary Search Tree Value I, II

>**Question I**  

one closest value

>**Solution**

can be solved by recursion [here])(http://www.cnblogs.com/jcliBlogger/p/4763200.html)
```c++
class Solution {
public:
    int closestValue(TreeNode* root, double target) {
        if (!root) return INT_MAX;
        if (!root->left && !root->right) return root->val;

        int left = closestValue(target->left, target);
        int right = closestValue(target->right, target);

        double td = abs(root->val - target), ld = abs(left - target), rd = abs(right - target);
        if (td < ld) return (td < rd) ? root->val : right;
        else return (ld < rd) ? left : right;
    }
}
```

or

```c++
// recursive
int closestValue(TreeNode* root, double target) {
    int a = root->val;
    auto kid = target < a ? root->left : root->right;
    if (!kid) return a;
    int b = closestValue(kid, target);
    return abs(a - target) < abs(b - target) ? a : b;
}
```

```c++
// iterative
int closestValue(TreeNode* root, double target) {
    int closest = root->val;
    while (root) {
        if (abs(closest - target) >= abs(root->val - target))
            closest = root->val;
        root = target < root->val ? root->left : root->right;
    }
    return closest;
}
```


more solution [here](https://leetcode.com/discuss/54438/4-7-lines-recursive-iterative-ruby-c-java-python)

>**Question II**

k closest values
>**Solution**

[here](http://www.cnblogs.com/jcliBlogger/p/4771342.html)

```c++
Solution {
public:
    vector<int> closestKValues(TreeNode* root, double target, int k) {
        priority_queue<pair<double, int>> pq;
        closestK(root, pq, target, k);
        vector<int> closest;
        while(!pq.empty()) {
            closest.push_back(pq.top().second);
            pq.pop();
        }
        return closest;
    }
private:
    void closestK(TreeNode* node, priority_queue<pair<double, int>>pq, double target, int k){
        if(!node) return ;
        pq.push(make_pair(abs(target - node->val), node->val));
        if (pq.size() > k) pq.pop();
        closestK(node->left, pq, target, k);
        closestK(node->right, pq, target, k);
    }
}
```
