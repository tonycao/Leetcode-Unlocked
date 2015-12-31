## Sparse Matrix Multiplication

>**Question**

Given two sparse matrices `A` and`B`, return the result of `AB`.

You may assume that A's column number is equal to B's row number.

**Example:**
```
A = [
      [ 1, 0, 0],
      [-1, 0, 3]
]

B = [
      [ 7, 0, 0 ],
      [ 0, 0, 0 ],
      [ 0, 0, 1 ]
]


     |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
                  | 0 0 1 |
```

>**Solution**

[Here](https://leetcode.com/discuss/71968/regular-28-ms-math-solution-6-8-lines)
and [here](https://leetcode.com/discuss/71912/easiest-java-solution)

```c++
class Solution {
public:
    vector<vector<int>> multiply(vector<vector<int>>& A, vector<vector<int>>& B) {
        int m = A.size(), n = B.size(), p = B[0].size();
        vector<vector<int>> C(m, vector<int>(p, 0));
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                if (A[i][j])
                    for (int k = 0; k < p; k++)
                        C[i][k] += A[i][j] * B[j][k];
        return C;
    }
};
```
