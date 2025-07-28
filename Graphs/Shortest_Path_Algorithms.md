## Problems:

1. [Shortest Path in UG with unit weights](#ans-1)
2. [Shortest Path in DAG](#ans-2)
3. [Dijkstra's Algorithm](#ans-3)
4. [Why Priority Queue is used in Dijkstra's Algorithm](#ans-4)
5. [Shortest Path in a Binary Maze](#ans-5)
6. [Path with Minimum Effort](#ans-6)
7. [Cheapest Flights Within K Stops](#ans-7)
8. [Number of Ways to Arrive at Destination](#ans-8)
9. [Minimum Steps to Reach End from Start by Performing Multiplication and Mod Operations with Array Elements](#ans-9)
10. [Bellman-Ford Algorithm](#ans-10)
11. [Floyd-Warshall Algorithm](#ans-11)
12. [Find the City with the Smallest Number of Neighbors in a Threshold Distance](#ans-12)
13. [Network Delay Time](#ans-13)


## Solutions:

#### Ans 1.
    1. We create a dist array of size N initialized with a 
    very large number which can never be the answer to indicate 
    that initially, all the nodes are untraversed.
    
    2. Then, perform the standard BFS traversal. 
    
    3. In every iteration, pick up the front() node, 
    and then traverse for its adjacent nodes. 
    
    4. For every adjacent node, we will relax the 
    distance to the adjacent node 
    if (dist[node] + 1 < dist[adjNode]). 
    
    5. Here dist[node] means the distance taken to reach the current node, 
    and ‘1’ is the edge weight between the node and the adjNode. 
    
    6. We will relax the edges if this distance is shorter than the previously taken distance. 
    
    7. Every time a distance is updated for the adjacent node, 
    we push that into the Queue with the increased distance. 

```cpp
vector<int> shortestPath(vector<vector<int>>& adj, int src) {
    vector<int> distance(adj.size(), 1e9);
    distance[src] = 0;
    
    queue<int> q;
    q.push(src);
    
    while (q.size()) {
        int node = q.front();
        q.pop();
        
        for(auto adjacentNode : adj[node]) {
            if (distance[node] + 1 < distance[adjacentNode]) {
                distance[adjacentNode] = distance[node] + 1;
                q.push(adjacentNode);
            }
        }
    }
    
    for (int i = 0; i < distance.size(); i++) {
        if (distance[i] == 1e9) distance[i] = -1;
    }
    
    return distance;
}
```
________________________________

#### Ans 2.

    The shortest path in a directed acyclic graph can be 
    calculated by the following steps:

    1. Perform topological sort on the graph using BFS/DFS and store it in a stack.  
    
    2. Now, iterate on the topo sort. We can keep the generated topo sort in the stack only, 
    and do an iteration on it, it reduces the extra space which would have been required to store 
    it. Make sure for the source node, we will assign dist[src] = 0. 
    
    3. For every node that comes out of the stack which contains our topo sort, we can 
    traverse for all its adjacent nodes, and relax them. 
    
    4. In order to relax them, we simply do a simple comparison of dist[node] + wt and dist[adjNode]. 
    Here dist[node] means the distance taken to reach the current node, and it is 
    the edge weight between the node and the adjNode. 
    
    5. If (dist[node] + wt < dist[adjNode]), then we will go ahead and update 
    the distance of the dist[adjNode] to the new found better path. 
    
    6. Once all the nodes have been iterated, 
    the dist[] array will store the shortest paths and we can then return it.

    Dry Run:
    adjLs = {
        0: [(1, 2), (2, 1)],
        1: [],
        2: [],
        3: []
    }

    visited = {0, 0, 0, 0}
    stk = {}

    stk = {3, 0, 1, 2}
    distance = {0, 1e9, 1e9, 1e9}

    while loop dry run:
        0. node = 3
            v = null
            skip for loop
        1. node = 0
            v = 1
            wt = 2
            distance[0] + 2 < distance[1]:
                distance[1] = 2
            v = 2
            wt = 1
            distance[0] + 1 < distance[2]:
                distance[2] = 1
        2. node = 1
            v = null
            skip for loop
        3. node = 2
            v = null
            skip for loop

    distance = {0, 2, 1, 1e9}

```cpp
void topoSort(int i, vector<int> &visited, 
stack<int> &stk, vector<pair<int, int>> adjLs[]) {
    visited[i] = 1;
    
    for (auto it: adjLs[i]) {
        int v = it.first;
        if (!visited[v]) {
            topoSort(v, visited, stk, adjLs);
        }
    }
    
    stk.push(i);
}

vector<int> shortestPath(int V, int E, vector<vector<int>>& edges) {
    vector<pair<int, int>> adjLs[V];
    
    for (int i = 0; i < E; i++) {
        int u = edges[i][0];
        int v = edges[i][1];
        int wt = edges[i][2];
        adjLs[u].push_back({v, wt});
    }
    
    vector<int> visited(V, 0);
    stack<int> stk;
    
    for (int i = 0; i < V; i++) {
        if (!visited[i]) {
            topoSort(i, visited, stk, adjLs);
        }
    }
    
    vector<int> distance(V, 1e9);
    
    distance[0] = 0;
    
    while (!stk.empty()) {
        int node = stk.top();
        stk.pop();

        for (auto it: adjLs[node]) {
            int v = it.first;
            int wt = it.second;

            if (distance[node] + wt < distance[v]) {
                distance[v] = wt + distance[node];
            }
        }
    }

    for (int i = 0; i < V; i++) {
        if (distance[i] == 1e9) distance[i] = -1;
    }
        
    return distance;
}
```
________________________________

#### Ans 3.
```cpp
```

________________________________

#### Ans 4.

________________________________

#### Ans 5.

________________________________

#### Ans 6.

________________________________

#### Ans 7.

________________________________

#### Ans 8.

________________________________

#### Ans 9.

________________________________

#### Ans 10.

________________________________

#### Ans 11.

________________________________

#### Ans 12.

________________________________

#### Ans 13.

________________________________
