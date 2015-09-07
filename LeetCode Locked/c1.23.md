## Paint Fence

>**Question**

There is a fence with n posts, each post can be painted with one of the k colors.

You have to paint all the posts such that no more than two adjacent fence posts have the same color.

Return the total number of ways you can paint the fence.

>**Note**

n and k are non-negative integers.

>**Solution**

DP, see [here](https://leetcode.com/discuss/56146/dynamic-programming-c-o-n-time-o-1-space-0ms) and [here](http://www.fgdsb.com/2015/01/04/fence-painter/)

```c++
class Solution {
public:
    int numWays(int n, int k) {
        if (n < 2 || !k) return n * k;
        int s = k, d1 = k, d2 = k * (k - 1);
        for (int i = 2; i < n; i++) {
            s = d2;
            d2 = (k - 1) * (d1 + d2);
            d1 = s;
        }
        return s + d2;
    }
};
```
