## Problems

1. [Minimum Spanning Tree](#minimum-spanning-tree)
2. [Prim's Algorithm](#prims-algorithm)
3. [Disjoint Set (Union by Rank)](#disjoint-set-union-by-rank)
4. [Disjoint Set (Union by Size)](#disjoint-set-union-by-size)
5. [Kruskal's Algorithm](#kruskals-algorithm)
6. [Number of Operations to Make Network Connected](#number-of-operations-to-make-network-connected)
7. [Most Stones Removed with Same Rows or Columns](#most-stones-removed-with-same-rows-or-columns)
8. [Accounts Merge](#accounts-merge)
9. [Number of Islands II](#number-of-islands-ii)
10. [Making a Large Island](#making-a-large-island)
11. [Swim in Rising Water](#swim-in-rising-water)

________________________________

## Solutions

### Minimum Spanning Tree

A **spanning tree** is a tree with `N` nodes and `N-1` edges such that all nodes are reachable from each other.

Among all possible spanning trees, the **Minimum Spanning Tree (MST)** has the smallest total edge weight.

> There may exist multiple minimum spanning trees for a graph.

________________________________

### Prim's Algorithm

Prim’s algorithm helps to find the MST for a graph.

Steps:

1. Push the triplet (edge weight `0`, node `0`, parent `-1`) into a **min-heap**.
2. Pop the top element (minimum edge weight) from the heap.
3. If the node is already visited, skip it.
4. If not, mark it visited and add the edge weight to the sum.
5. Insert all unvisited neighbors of the current node into the priority queue with triplets `(cost, neighborNode, currentNode)`.
6. Repeat steps 2–5 until the heap is empty.
7. The sum now contains the weight of the MST.

```cpp
int sum = 0;
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
pq.push({0, 0});
vector<int> visited(V, 0);

while (!pq.empty()) {
    auto it = pq.top();
    pq.pop();

    int node = it.second;
    int cost = it.first;

    if (visited[node] == 1) continue;

    visited[node] = 1;
    sum += cost;

    for (auto nebor : adjLs[node]) {
        int neborNode = nebor.first;
        int neborCost = nebor.second;

        if (!visited[neborNode]) {
            pq.push({neborCost, neborNode});
        }
    }
}

return sum;
```

________________________________

### Disjoint Set (Union by Rank)

Disjoint Set supports two main operations:

* `findPar(x)`: Find the **ultimate parent** of node `x`.
* `union(u, v)`: Merge components of `u` and `v`.

**Union by Rank**:

* Rank represents the tree depth (number of nodes including the leaf).
* When merging two sets, attach the tree with smaller rank under the tree with a higher rank.
* If ranks are equal, pick one as the root and increment its rank.

**Why Ultimate Parent?**

To check if two nodes are in the same component, compare their ultimate parents—not immediate parents.

**Path Compression**:

When finding the ultimate parent, make all nodes in the path directly point to that parent to flatten the tree. 
This reduces the time complexity almost to constant time.

> Even after path compression, rank is not updated—only parent pointers are.

**Time Complexity**:
Amortized nearly `O(1)` due to the inverse Ackermann function (extremely small, practically constant).

```cpp
class DisjointSet {
    vector<int> rank, parent, size;

public:
    DisjointSet(int n) {
        rank.resize(n + 1, 0);
        parent.resize(n + 1);
        
        for (int i = 0; i <= n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }

    int findUPar(int node) {
        if (node == parent[node]) return node;
        
        return parent[node] = findUPar(parent[node]);
    }

    void unionByRank(int u, int v) {
        int ulp_u = findUPar(u);
        int ulp_v = findUPar(v);
        
        if (ulp_u == ulp_v) return;
        
        if (rank[ulp_u] < rank[ulp_v]) {
            parent[ulp_u] = ulp_v;
        }
        
        else if (rank[ulp_v] < rank[ulp_u]) {
            parent[ulp_v] = ulp_u;
        }
        
        else {
            parent[ulp_v] = ulp_u;
            rank[ulp_u]++;
        }
    }
}
```
________________________________

### Disjoint Set (Union by Size)

**Union by Size** is similar to union by rank but instead of rank, the size of each component (number of nodes) is used.

* Attach the smaller component to the larger one.
* Update the size of the new parent after union.

Benefits:

* Ensures minimal tree height and efficient `findPar` operations.

```cpp
void unionBySize(int u, int v) {
    int ulp_u = findUPar(u);
    int ulp_v = findUPar(v);
    
    if (ulp_u == ulp_v) return;
    
    if (size[ulp_u] < size[ulp_v]) {
        parent[ulp_u] = ulp_v;
        size[ulp_v] += size[ulp_u];
    }
    
    else {
        parent[ulp_v] = ulp_u;
        size[ulp_u] += size[ulp_v];
    }
}
```
________________________________

### Kruskal's Algorithm
________________________________
### Number of Operations to Make Network Connected
________________________________
### Most Stones Removed with Same Rows or Columns
________________________________
### Accounts Merge
________________________________
### Number of Islands II
________________________________
### Making a Large Island
________________________________
### Swim in Rising Water
________________________________