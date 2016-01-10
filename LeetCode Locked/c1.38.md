## Number of Connected Components in an Undirected Graph

>**Question**

Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to find the number of connected components in an undirected graph.

Example 1:
```
     0          3
     |          |
     1 --- 2    4
```
Given `n = 5` and `edges = [[0, 1], [1, 2], [3, 4]]`, return `2`.

Example 2:
```
     0           4
     |           |
     1 --- 2 --- 3
```
Given `n = 5` and `edges = [[0, 1], [1, 2], [2, 3], [3, 4]]`, return `1`.

**Note:**

You can assume that no duplicate edges will appear in edges. Since all edges are undirected, `[0, 1]` is the same as `[1, 0]` and thus will not appear together in edges.


>**Solution**

Union Find

```java
public class Solution {
    public int countComponents(int n, int[][] edges) {
        unionFind uf = new unionFind(n);
        for (int[] edge : edges) {
            if (!uf.isConnected(edge[0], edge[1])) {
                uf.union(edge[0], edge[1]);
            }
        }
        return uf.findCount();
    }

    public class unionFind{
            int[] ids;
            int count;

            public unionFind(int num) {
                this.ids = new int[num];
                for (int i=0; i<num; i++) {
                    ids[i] = i;
                }
                this.count = num;
            }

            public int find(int i) {
                return ids[i];
            }

            public void union(int i1, int i2) {
                int id1 = find(i1);
                int id2 = find(i2);
                if (id1 != id2) {
                    for (int i=0; i<ids.length; i++) {
                        if (ids[i] == id2) {
                            ids[i] = id1;
                        }
                    }
                    count--;
                }
            }

            public boolean isConnected(int i1, int i2) {
                return find(i1)==find(i2);
            }

            public int findCount() {
                return count;
            }
        }
}
```

[another solution](http://segmentfault.com/a/1190000004224298)

time: O(m*h), space: O(n), m表示edge的数量

```java
public class Solution {
    public int countComponents(int n, int[][] edges) {
        int[] id = new int[n];

        // 初始化
        for (int i = 0; i < n; i++) {
            id[i] = i;
        }

        // union
        for (int[] edge : edges) {              
            int i = root(id, edge[0]);
            int j = root(id, edge[1]);
            id[i] = j;
        }

        // 统计根节点个数
        int count = 0;
        for (int i = 0; i < n; i++) {
            if (id[i] == i)
                count++;
        }
        return count;
    }

    // 找根节点
    public int root(int[] id, int i) {
        while (i != id[i]) {
            id[i] = id[id[i]];
            i = id[i];
        }
        return i;
    }
}
```
