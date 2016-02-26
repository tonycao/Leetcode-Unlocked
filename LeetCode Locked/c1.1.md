## Factor Combinations
>**Question**

Print all unique combination of factors (except 1) of a given number.  

For example:  
Input: `12`  
Output: `[[2, 2, 3], [2, 6], [3, 4]]`  
Input: `15`  
Output: `[[3, 5]]`  
Input: `28`
Output: `[[2, 2, 7], [2, 14], [4, 7]]`


>**Solution**

DFS


```c++
class Solution {
public:
    vector<vector<int>> getFactors(int n) {
        vector<vector<int>> ret;
        vector<int> cur;
        dfs(n, ret, cur, 1, 2);
        return ret;
    }

    void dfs(int n, vector<vector<int>> &ret, vector<int> cur, int product, int pos){
        if(product == n){
            if(!cur.empty()) ret.push_back(cur);
        } else if(product < n && pos < n){
            for(int i = pos; i < n; ++i){
                if(product * i > n) break;  // prune
                if(n % i != 0) continue;  // prune
                cur.push_back(i);
                dfs(n, ret, cur, product * i, i);
                cur.pop_back();
            }
        }
    }
};
```

Another solution with Lambda expression
```c++
vector<vector<int>> factors_comb(int n) {
    vector<vector<int>> ret = {};
    function<void(int, vector<int>&)> dfs = [&](int num, vector<int>& cur) {
        int last = cur.empty() ? 2 : cur.back();
        for(int f = last; f < num; ++f) {
            if(num % f != 0) continue;
            cur.push_back(f);
            dfs(num / f, cur);
            cur.pop_back();
        }
        if(!cur.empty() && num >= last) {
            cur.push_back(num);
            ret.push_back(cur);
            cur.pop_back();
        }
    };
    vector<int> cur = {};
    dfs(n, cur);
    return ret;
}
```
