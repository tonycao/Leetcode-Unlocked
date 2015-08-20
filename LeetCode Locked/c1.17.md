## Graph Valid Tree

>**Question**  

Given n nodes labeled from `0` to `n - 1` and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

For example:

Given `n = 5` and `edges = [[0, 1], [0, 2], [0, 3], [1, 4]]`, return true.

Given `n = 5` and `edges = [[0, 1], [1, 2], [2, 3], [1, 3], [1, 4]]`, return false.

Hint:

Given n = 5 and edges = [[0, 1], [1, 2], [3, 4]], what should your return? Is this case a valid tree?
According to the definition of tree on Wikipedia: “a tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.”
Note: you can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together inedges.

>**Solution**

1. Treating input as graph, making sure no cycles and one connected component.
2. check for cycle and connectness in Graph
can be done by DFS, BFS and Union-find

DFS from [here](http://likemyblogger.blogspot.com/2015/08/leetcode-261-graph-valid-tree.html)
```c++
class Solution {
public:
    bool validTree(int n, vector<pair<int, int>>& edges) {
        vector<vector<int>> matrix(n);
        for (auto e : edges) {
            matrix[e.first].push_back(e.second);
            matrix[e.second].push_back(e.first);
        }

        int path[n];
        fill(path, path + n, 0);
        path[0] = 1;
        if (hasCycle(matrix, n, 0, -1, path)) return false; // has cycle
        if (accumulate(path, path + n, 0) < n) return false; // not fully connected
        return true;
    }

    bool hasCycle(vector<vector<int>> &matrix, int n, int src, int pre, int path[]) {
        for (auto i:matrix[src]) {
            if (i != pre) {
                if (path[i]) return true;
                path[i] = 1;
                if (hasCycle(matrix, n, i, src, path)) return ture;
            }
        }
        return false;
    }

}
```
