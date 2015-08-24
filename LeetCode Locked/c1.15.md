## Paint House

>**Question 1**

There are a row of `n` houses, each house can be painted with one of the three colors: `red`, `blue` or `green`. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a `n x 3` cost matrix. For example, `costs[0][0]` is the cost of painting house `0` with color red; `costs[1][2]` is the cost of painting house `1` with color green, and so on... Find the minimum cost to paint all houses.

>**Note**

All costs are positive integers.


>**Solution**

DP.

O(1) space from [here](http://likemyblogger.blogspot.com/2015/08/leetcode-256-paint-house.html).

```c++
//C++: 12ms
class Solution {
public:
    int minCost(vector<vector<int>>& costs) {
        int n = costs.size();
        if(n==0) return 0;
        for(int i=1; i<n; ++i){
            for(int j=0; j<3; ++j){
                costs[i][j] += min(costs[i-1][(j+1)%3], costs[i-1][(j+2)%3]);
            }
        }
        return min(costs[n-1][0], min(costs[n-1][1], costs[n-1][2]));
    }
};
```
or [here](http://www.cnblogs.com/jcliBlogger/p/4729957.html).

```c++
class Solution {
public:
    int minCost(vector<vector<int>>& costs) {
        if (costs.empty()) return 0;
        int n = costs.size(), r = 0, g = 0, b = 0;
        for (int i = 0; i < n; i++) {
            int rr = r, bb = b, gg = g;
            r = costs[i][0] + min(bb, gg);
            b = costs[i][1] + min(rr, gg);
            g = costs[i][2] + min(rr, bb);
        }
        return min(r, min(b, g));
    }
};
```

another O(n) space from [here](http://blog.csdn.net/craiglin1992/article/details/44885775)

```java
public int minPaintCost(int[][] cost) {
    if (cost == null || cost.length == 0) return 0;
    int[][] dp = new int[cost.length][3];
    dp[0][0] = cost[0][0], dp[0][1] = cost[0][1], dp[0][2] = cost[0][2];
    for (int i = 1; i < cost.length; ++i) {
        dp[i][0] = cost[i][0] + Math.min(dp[i-1][1], dp[i-1][2]);
        dp[i][1] = cost[i][1] + Math.min(dp[i-1][0], dp[i-1][2]);
        dp[i][2] = cost[i][2] + Math.min(dp[i-1][0], dp[i-1][1]);
    }
    return Math.min(dp[dp.length-1][0], Math.min(dp[dp.length-1][1],[dp.length-1][2]));
}
```

>**Question 2**

There are a row of n houses, each house can be painted with one of the k colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a `n x k` cost matrix. For example, `costs[0][0]` is the cost of painting house 0 with color 0; `costs[1][2]` is the cost of painting house 1 with color 2, and so on... Find the minimum cost to paint all houses.

>**Note**

All costs are positive integers.

Follow up:
Could you solve it in O(nk) runtime?

>**Solution**

dp: maintain the minimum two costs min1(smallest) and min2 (second to smallest) after painting i-th house.

from [here](https://leetcode.com/discuss/52982/c-dp-time-o-nk-space-o-k)

```c++
class Solution {
public:
    int minCostII(vector<vector<int>>& costs) {
        if (costs.empty()) return 0;
        int n = costs.size(), k = costs[0].size(), min1, min2;
        vector<int> dp(k, 0);
        for (int i = 0; i < n; i++) {
            int premin1 = i ? min1 : 0, premin2 = i ? min2 : 0;
            min1 = min2 = INT_MAX;
            for (int j = 0; j < k; j++) {
                if (dp[j] != premin1 || premin1 == premin2)
                    dp[j] = premin1 + costs[i][j];
                // pre_min1 occurred when painting house i-1 with color j, 
                // so it cannot be added to dp[j]
                else dp[j] = premin2 + costs[i][j];
                if (min1 <= dp[j]) min2 = min(min2, dp[j]);
                else min2 = min1, min1 = dp[j];
            }
        }
        return min1;
    }
};
```
