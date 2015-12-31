## Number of Islands II

>**Question**

A 2d grid map of m rows and n columns is initially filled with water. We may perform an addLand operation which turns the water at position `(row, col)` into a land. Given a list of positions to operate, count the number of islands after each addLand operation. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example**
Given `m = 3, n = 3`, `positions = [[0,0], [0,1], [1,2], [2,1]]`.
Initially, the 2d grid grid is filled with water. (Assume 0 represents water and 1 represents land).
```
0 0 0
0 0 0
0 0 0
```
Operation #1: `addLand(0, 0)` turns the water at `grid[0][0]` into a land.

```
1 0 0
0 0 0   Number of islands = 1
0 0 0
```
Operation #2: `addLand(0, 1)` turns the water at `grid[0][1]` into a land.

```
1 1 0
0 0 0   Number of islands = 1
0 0 0
```
Operation #3: `addLand(1, 2)` turns the water at `grid[1][2]` into a land.

```
1 1 0
0 0 1   Number of islands = 2
0 0 0
```
Operation #4: `addLand(2, 1)` turns the water at `grid[2][1]` into a land.
```
1 1 0
0 0 1   Number of islands = 3
0 1 0
```
We return the result as an array: `[1, 1, 2, 3]`


**Note**
`0` is represented as the sea, `1` is represented as the island. If two `1` is adjacent, we consider them in the same island. We only consider `up/down/left/right` adjacent.

>**Solution**

[Union-find](https://www.cs.princeton.edu/~rs/AlgsDS07/01UnionFind.pdf)

```c++
class UnionFind2D {
public:
    UnionFind2D(int m, int n) {
        for (int i = 0; i < m * n; i++) ids.push_back(-1);
        for (int i = 0; i < m * n; i++) szs.push_back(1);
        M = m, N = n, cnt = 0;
    }
    int index(int x, int y) {
        return x * N + y;
    }
    int size(void) {
        return cnt;
    }
    int id(int x, int y) {
        if (x >= 0 && x < M && y >= 0 && y < N)
            return ids[index(x, y)];
        return -1;
    }
    int add(int x, int y) {
        int idx = index(x, y);
        ids[idx] = idx;
        szs[idx] = 1;
        cnt++;
        return idx;
    }
    int root(int i) {
        for (; i != ids[i]; i = ids[i])
            ids[i] = ids[ids[i]];
        return i;
    }
    bool find(int p, int q) {
        return root(p) == root(q);
    }
    void unite(int p, int q) {
        int i = root(p), j = root(q);
        if (szs[i] > szs[j]) swap(i, j);
        ids[i] = j;
        szs[j] += szs[i];
        cnt--;
    }
private:
    vector<int> ids, szs;
    int M, N, cnt;
};

class Solution {
public:
    vector<int> numIslands2(int m, int n, vector<pair<int, int>>& positions) {
        UnionFind2D islands(m, n);
        vector<pair<int, int>> dirs = { { 0, -1 }, { 0, 1 }, { -1, 0 }, { 1, 0 } };
        vector<int> ans;
        for (auto& pos : positions) {
            int x = pos.first, y = pos.second;
            int p = islands.add(x, y);
            for (auto& d : dirs) {
                int q = islands.id(x + d.first, y + d.second);
                if (q >= 0 && !islands.find(p, q))
                    islands.unite(p, q);
            }
            ans.push_back(islands.size());
        }
        return ans;
    }
};
```

[another implementation](http://www.cnblogs.com/easonliu/p/4684136.html)
```c++
/**
 * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */
class Solution {
public:
    /*
     * @param n an integer
     * @param m an integer
     * @param operators an array of point
     * @return an integer array
     */

    int findFather(unordered_map<int, int> &father, int x) {
        if (father.find(x) == father.end()) {
            father[x] = x;
            return x;
        }
        int res = x, tmp;
        while (res != father[res]) {
            res = father[res];
        }
        while (x != father[x]) {
            tmp = father[x];
            father[x] = res;
            x = tmp;
        }
        return x;
    }
    vector<int> numIslands2(int n, int m, vector<Point>& operators) {
        // Write your code here
        unordered_map<int, int> father;
        vector<int> res;
        int cnt = 0;
        const int dx[] = {0, 1, 0, -1};
        const int dy[] = {1, 0, -1, 0};
        for (auto &op : operators) {
            int p = op.x * m + op.y;
            int fp = findFather(father, p);
            if (fp == p) ++cnt;
            for (int i = 0; i < 4; ++i) {
                int xx = op.x + dx[i], yy = op.y + dy[i];
                if (xx < 0 || xx >= n || yy < 0 || yy >= m) continue;
                int q = (op.x + dx[i]) * m + op.y + dy[i];
                if (father.find(q) != father.end()) {
                    int fq = findFather(father, q);
                    if (fp != fq) {
                        --cnt;
                        father[fq] = fp;
                    }
                }
            }
            res.push_back(cnt);
        }
        return res;
    }
};
```
