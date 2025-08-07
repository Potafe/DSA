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

    We will be implementing Kruskal’s algorithm using the Disjoint Set data structure

The algorithm steps are as follows:

1. First, we need to extract the edge information from the given 
adjacency list in the format of (wt, u, v) and we will store the tuples in an array.

2. Then the array must be sorted in the ascending order of the 
weights so that while iterating we can get the edges with the minimum weights first.

3. After that, we will iterate over the edge information, and for each tuple, we will apply the following operation:

4. First, we will take the two nodes u and v from the tuple and check 
if the ultimate parents of both nodes are the same or not using the 
findUPar() function provided by the Disjoint Set data structure.

5. If the ultimate parents are the same, we need not do anything to that 
edge as there already exists a path between the nodes and we will continue to the next tuple.

6. If the ultimate parents are different, we will add the weight of the 
edge to our final answer (i.e. mstWt variable used in the following code) and 
apply the union operation(i.e. either unionBySize(u, v) or unionByRank(u, v)) 
with the nodes u and v. The union operation is also provided by the Disjoint Set.

7. Finally, we will get our answer (in the mstWt variable as used in the following code).

```cpp
int kruskalsMST(int V, vector<vector<int>> &edges) {
    for (auto& it : edges) {
        swap(it[0], it[2]);
    }
    
    sort(edges.begin(), edges.end());
    
    DisjointSet ds(V);
    
    int mostWeight = 0;
    
    for (auto it: edges) {
        int wt = it[0];
        int u = it[1];
        int v = it[2];
        
        int ulp_u = ds.findUPar(u);
        int ulp_v = ds.findUPar(v);
        
        if (ulp_u != ulp_v) {
            mostWeight += wt;
            ds.UnionByRank(u, v);
        }
        
    }
    
    return mostWeight;
}
```
________________________________

### Number of Operations to Make Network Connected

The idea is simple: 

1. We first check if the connections are less than n - 1: if they are return -1 (as it's not possible to connect all comps)
2. Now what we basically do is -> do the Union of all the components given in the conncections
3. Now we loop for all the nodes and check the parents using findUPar -> if findUPar(node) == node -> ans++

```cpp
int makeConnected(int n, vector<vector<int>>& connections) {
    if (connections.size() < n - 1) return -1;

    DisjointSet ds(n);

    for (auto& connection : connections) {
        int u = connection[0];
        int v = connection[1];
        ds.UnionBySize(u, v);
    }

    int components = 0;
    for (int i = 0; i < n; i++) {
        if (ds.findUPar(i) == i) {
            components++;
        }
    }

    return components - 1;    
}
```
________________________________

### Most Stones Removed with Same Rows or Columns

So the max stones we can remove are: total_stones - total_components/groups/ultimateParents

1. First we make a DS of max no of rows + max no of cols + 1
2. Now we fill this DS by following logic:
-  We union the row with col -> where col is replaced by col + maxRows + 1
-  We also mantain a map to keep track of all columns and row visited
3. Now we traverse all the rows and columns using the above map and check if findPar(node) == node -> if yes -> groups++
4. Return total - groups

```cpp
int removeStones(vector<vector<int>>& stones) {
    int maxRow = 0;
    int maxCol = 0;
    
    for (auto it : stones) {
        maxRow = max(maxRow, it[0]);
        maxCol = max(maxCol, it[1]);
    }

    DisjointSet ds(maxRow + maxCol + 1);
    unordered_map<int, int> stoneNodes;

    for (auto it : stones) {
        int nodeRow = it[0];
        int nodeCol = it[1] + maxRow + 1;
        ds.UnionBySize(nodeRow, nodeCol);
        stoneNodes[nodeRow] = 1;
        stoneNodes[nodeCol] = 1;
    }

    int cnt = 0;
    for (auto it : stoneNodes) {
        if (ds.findUPar(it.first) == it.first) {
            cnt++;
        }
    }
    return stones.size() - cnt;
}
```
________________________________

### Accounts Merge

The algorithm steps are the following:

1. First, we will create a map data structure. Then we will store each email with the respective 
index of the account (the email belongs to) in that map data structure.

2. While doing so, if we encounter an email again (i.e. If any index is previously assigned for the email), 
we will perform union of the current index and the previously assigned index.

3. After completing step 2, now it’s time to merge the accounts. For merging, we will iterate over all the emails individually and find the ultimate parent of the assigned index of every email. Then we will add the email of the current account to the index that is the ultimate parent. Thus the accounts will be merged.

4. Finally, we will sort the emails for every account separately and store the final results in the answer array accordingly.

```cpp
vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
    int n = accounts.size();
    
    DisjointSet ds(n);
    unordered_map<string, int> mp;

    sort(accounts.begin(), accounts.end());

    for (int i = 0; i < n; i++) {
        for (int j = 1; j < accounts[i].size(); j++) {
            if (mp.find(accounts[i][j]) == mp.end()) {
                mp[accounts[i][j]] = i;
            }

            else {
                ds.UnionBySize(mp[accounts[i][j]], i);
            }
        }
    }

    vector<vector<string>> mergedMails(n);

    for (auto it: mp) {
        string mail = it.first;
        int node = it.second;
        
        int parent = ds.findUPar(node);
        mergedMails[parent].push_back(mail);
    }     

    vector<vector<string>> ans;

    for (int i = 0; i < n; i++) {
        if (mergedMails[i].size() == 0) continue;

        sort(mergedMails[i].begin(), mergedMails[i].end());

        vector<string> temp;
        
        //pushing the name
        temp.push_back(accounts[i][0]);
        
        //pushing all the merged mails
        for (auto it : mergedMails[i]) {
            temp.push_back(it);
        }
        
        ans.push_back(temp);
    }

    sort(ans.begin(), ans.end());
    return ans;
}
```
________________________________

### Number of Islands II

________________________________

### Making a Large Island
________________________________

### Swim in Rising Water
________________________________