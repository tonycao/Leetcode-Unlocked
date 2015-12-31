## Range Sum Query 2D - Mutable

>**Question**

Given a 2D matrix, find the sum of the elements inside the rectangle defined by (row1, col1), (row2, col2).

**Example:**
```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12
```
**Note:**

1. You may assume that the matrix does not change.
2. There are many calls to sumRegion function.
3. You may assume that row1 ≤ row2 and col1 ≤ col2.


>**Solution**

[binary indexed tree](https://www.topcoder.com/community/data-science/data-science-tutorials/binary-indexed-trees/)

```c++
class NumMatrix {
    vector<vector<int>> tree;
    vector<vector<int>> ma;
public:
    NumMatrix(vector<vector<int>> &matrix) {
        int n = matrix.size();
        int m = n == 0 ? 0 : matrix[0].size();
        if (n == 0 || m == 0) {
          return ;
        }

        tree = vector<vector<int>>(n + 1, vector<int>(m + 1, 0));
        ma = vector<vector<int>>(n, vector<int>(m, 0));
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                update(i, j, matrix[i][j]);
            }
        }
    }

    void update(int row, int col, int val) {
        int delta = val - ma[row][col];
        ma[row][col] = val;
        for (int i = row + 1; i <= ma.size(); i += i & (-i)) {
            for (int j = col + 1; j <= ma[0].size(); j += j & (-j)) {
                tree[i][j] += delta;
            }
            //for (int j = col + 1; j > 0; j -= j & (-j)) {
            //    sol += tree[i][j];
            //}
        }
        return sol;
    }

    int sumRegion(int row1, int col1, int row2, int col2) {

    }
};

// Your NumMatrix object will be instantiated and called as such:
// NumMatrix numMatrix(matrix);
// numMatrix.sumRegion(0, 1, 2, 3);
// numMatrix.update(1, 1, 10);
// numMatrix.sumRegion(1, 2, 3, 4);
```

another implementation
```c++
class NumMatrix {
    vector<vector<int>> rowSums;
    vector<vector<int>> ma;
public:
    NumMatrix(vector<vector<int>> &matrix) {
        int n = matrix.size();
        int m = (n == 0) ? 0 : matrix[0].size();
        if (n == 0 || m == 0) {
            return;
        }
        ma = matrix;
        rowSums = vector<vector<int>>(n, vector<int>(m + 1, 0));
        for (int i = 0; i < n; i++) {
            for (int j = 1; j <= m; j++) {
                rowSums[i][j] = rowSums[i][j - 1] + matrix[i][j - 1];
            }
        }
    }

    void update(int row, int col, int val) {
        for (int i = col + 1; i <= ma[0].size(); i++) {
            rowSums[row][i] += (-ma[row][col] + val);
        }
        ma[row][col] = val;
    }

    int sumRegion(int row1, int col1, int row2, int col2) {
        int sol = 0;
        for (int i = row1; i <= row2; i++) {
            sol += (rowSums[i][col2 + 1] - rowSums[i][col1]);
        }
        return sol;
    }
};
```
