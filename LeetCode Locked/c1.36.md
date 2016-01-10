## Shortest Distance from All Buildings

>**Question**

You want to build a house on an empty land which reaches all buildings in the shortest amount of distance. You can only move up, down, left and right. You are given a 2D grid of values `0, 1 or 2`, where:

Each 0 marks an empty land which you can pass by freely.
Each 1 marks a building which you cannot pass through.
Each 2 marks an obstacle which you cannot pass through.
For example, given three buildings at `(0,0), (0,4), (2,2)`, and an obstacle at `(0,2)`:
```
1 - 0 - 2 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0
```
The point `(1,2)` is an ideal empty land to build a house, as the total travel distance of `3+3+1=7` is minimal. So return 7.

**Note:**

There will be at least one building. If it is not possible to build such house according to the above rules, return -1.

>**Solution**

[BFS](https://segmentfault.com/a/1190000004187914)

```c++
Class Solution{
public:
    int shortestDistance(vector<vector<int>> grid) {
        int rows = grid.size();
        if (rows == 0) {
            return -1;
        }
        int cols = grid[0].size();

        // sum of distances to all the buildings
        vector<vector<int>> dist(rows, vector<int>(cols, 0));

        // number of reachable buildings
        vector<vector<int>> nums(rows, vector<int>(cols, 0));
        int buildingNum = 0;

        // BFS from each building
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                if (grid[i][j] == 1) {
                    buildingNum++;
                    bfs(grid, i, j, dist, nums);
                }
            }
        }

        int minval = INT_MAX;
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                if (grid[i][j] == 0 && dist[i][j] != 0 && nums[i][j] == buildingNum) {
                    minval = min(minval, dist[i][j]);
                }
            }
        }
        if (minval < INT_MIN) {
            return minval;
        }
        return -1;
    }

private:
    void bfs(vector<vector<int>>& grid, int row, int col, vector<vector<int>>& dist, vector<vector<int>>& nums) {
        int rows = grid.size();
        int cols = grid[0].size();
        queue<pair<int, int>> q;
        q.push(make_pair(row, col));

        vector<vector<int>> dirs = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};

        // record visited node
        vector<vector<bool>> visited(rows, vector<bool>(cols, false));
        int level = 0;
        while(!q.empty()) {
            level++;
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                pair<int, int> coords = q.front();
                q.pop();
                for (int k = 0; k < dirs.size(); ++k) {
                    int x = coords.first + dirs[k][0];
                    int y = coords.second + dirs[k][1];
                    if (x >= 0 && x < rows && y >= 0 && y < cols && !visited[x][y] && grid[x][y] == 0) {
                        visited[x][y] = true;
                        dist[x][y] += level;
                        nums[x][y]++;
                        q.push(make_pair(x, y));
                    }
                }
            }
        }
    }
};
```
