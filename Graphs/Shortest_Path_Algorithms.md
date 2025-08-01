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
    Time: O((|V| + |E|) log V)
    Space: O(|V| + |E|)
    
    1. We would be using a set and a distance array of size V 
    (where ‘V’ are the number of nodes in the graph) 
    initialized with infinity (indicating that at present none of 
    the nodes are reachable from the source node) 
    and initialize the distance to source node as 0.
    
    2. We push the source node to the set along with its distance which is also 0.
    
    3. Now, we start erasing the elements from the set and look out for their adjacent nodes one by one. 
    If the current reachable distance is better than the previous distance indicated by the distance array, 
    we update the distance and insert it in the set.
    
    4. A node with a lower distance would be first erased from 
    the set as opposed to a node with a higher distance. 
    By following step 3, until our set becomes empty, we would get the
    minimum distance from the source node to all other nodes. 
    We can then return the distance array. 
    
    5. The only difference between using a Priority Queue 
    and a Set is that in a set we can check if there exists a pair 
    with the same node but a greater distance 
    than the current inserted node as there will be no point in keeping 
    that node into the set if we come across a much better value than that. 
    So, we simply delete the element with a greater distance value for the same node.

```cpp
vector<int> dijkstra(int V, vector<vector<int>> &edges, int src) {
    vector<vector<pair<int, int>>> adjLs(V);
    
    for (auto it: edges) {
        adjLs[it[0]].push_back({it[1], it[2]});
    }
    
    set<pair<int, int>> seti;
    seti.insert({0, src});
    
    vector<int> distance(V, 1e9);
    distance[src] = 0;
    
    while (seti.size()) {
        auto it = *(seti.begin());
        int node = it.second;
        int wt = it.first;
        
        seti.erase(it);
        
        for (auto it: adjLs[node]) {
            int adjNode = it.first;
            int edgW = it.second;
            
            
            if (distance[adjNode] > wt + edgW) {
                if (distance[adjNode] != 1e9) seti.erase({distance[adjNode], adjNode});
                distance[adjNode] = wt + edgW;
                seti.insert({distance[adjNode], adjNode});
            }
        }
    }
    
    return distance;
}
```

________________________________

#### Ans 4.

    1. We would be using a min-heap and a distance array of size V (where ‘V’ are the number of nodes in the graph) 
    initialized with infinity (indicating that at present none of the nodes are reachable from the source node) 
    and initialize the distance to source node as 0.
    
    2. We push the source node to the queue along with its distance which is also 0.
    
    3. For every node at the top of the queue, we pop the element out and look out for its adjacent nodes.
    If the current reachable distance is better than the previous distance indicated by the distance array, 
    we update the distance and push it into the queue.

```cpp
vector<int> dijkstra(int V, vector<vector<int>> &edges, int src) {
    vector<vector<pair<int, int>>> adj(V);
    
    for (auto it: edges) {
        adj[it[0]].push_back({it[1], it[2]});
    }
    
    // pair<int, int>:
    //    This specifies the type of elements that will be stored in the priority queue.
    // vector<pair<int, int>>:
    //    This specifies the underlying container used to implement the priority queue. 
    //    By default, std::priority_queue uses std::vector as its container.
    // greater<pair<int, int>>:
    //    This is the comparator (or comparison function) used to determine the priority of elements.
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    
    vector<int> distance(V, 1e9);
    
    
    distance[src] = 0;
    pq.push({0, src});
    
    while (!pq.empty()) {
        int node = pq.top().second;
        int dis = pq.top().first;
        pq.pop();

        for (auto it : adj[node]) {   
            int v = it.first;
            int w = it.second;
            
            if (dis + w < distance[v]) {
                distance[v] = dis + w;
                pq.push({dis + w, v});
            }
        }
    }
    
    return distance;
}
```
________________________________

#### Ans 5.
    We implement simple Djisktra:
        1. Base Case if grid[0][0] == 1 -> return -1
        2. Now we make a distance array and a queue:
            distance array = [][] // 2D Array
            queue = {distance, {row, col}} // Contains distance and row and col pair
        3. Now we traverse in 8 directions:
            dr[] = [-1, 0, 1, 0, -1, -1, 1, 1]
            dc[] = [0, 1, 0, -1, -1, 1, -1, 1]
        4. Traverse according to given rule and if destination reached:
            return distance (from queue)
        5. Else queue.push(distance + 1, {row, col})
```cpp
int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
    if (grid[0][0] == 1) return -1;
    
    int M = grid.size();
    int N = grid[0].size();

    vector<vector<int>> distance(M, vector<int>(N, 1e9));
    distance[0][0] = 1;

    queue<pair<int, pair<int, int>>> q;
    q.push({1, {0, 0}});

    int dr[] = {-1, 0, 1, 0, -1, -1, 1, 1};
    int dc[] = {0, 1, 0, -1, -1, 1, -1, 1};

    while (!q.empty()) {
        auto it = q.front();
        int dis = it.first;
        int row = it.second.first;
        int col = it.second.second;

        q.pop();

        if (row == M - 1 && col == N - 1) return dis;

        for (int i = 0; i < 8; i++) {
            int newr = row + dr[i];
            int newc = col + dc[i];

            if (newr >= 0 && newr < M && newc >= 0 && newc < N && 
            grid[newr][newc] == 0 && dis + 1 < distance[newr][newc]) {
                distance[newr][newc] = dis + 1;
                
                if (row == M - 1 && col == N - 1) return dis;

                q.push({dis + 1, {newr, newc}});
            }
        }
    }

    return -1;
}
```
________________________________

#### Ans 6.
    So we basically have this:
            1. Grid -> x and y coordinates and a value
            2. Goal is to reach M - 1, N - 1
            3. Maximum absolute difference should be minimum

    What we can do is use Dijkstra's algorithm to 
    find the minimum maximum absolute difference path:
            1. We use a priority queue to always expand the path with the smallest maximum absolute difference.
            2. Keep track of the maximum absolute difference for each cell.
            3. Update the maximum absolute difference when we find a better path.

```cpp
// So we create a distance array
vector<vector<int>> dist(M, vector<int>(N, INT_MIN))
dist[0][0] = 0

// Now we create a priority queue
priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, greater<pair<int, pair<int, int>>>> q;
q.push({0, {0, 0}})

int dx = {-1, 1, 0, 0}
int dy = {0, 0, -1, 1}

// Traverse the grid using Dijkstra's algorithm
while (q.size()) {
    auto it = q.top()
    int dist = it.first
    int x = it.second.first
    int y = it.second.second

    q.pop()

    if (x == M - 1 && y == N - 1) return dist

    for (int i = 0; i < 4; i++) {
        int nx = x + dx[i];
        int ny = y + dy[i];

        if (nx >= 0 && nx < M && ny >= 0 && ny < N) {
                // Calculate the new distance which should be MAX absolute difference
                int newDist = max(abs(grid[nx][ny] - grid[x][y]), dist);
                if (newDist > dist[nx][ny]) {
                        dist[nx][ny] = newDist;
                        q.push({newDist, {nx, ny}});
                }
        }
    }
}
```
________________________________

#### Ans 7.
    What we do is basically Djisktra with a constraint of stops:
            1. Keep a distance array which contains both dist and stops
            2. Keep a priority queue to store the least costly path first
            3. Djikstra that shit

```cpp
int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
    if (src == dst) return 0;

    vector<vector<pair<int, int>>> adjLs(n);

    for (auto it: flights) {
        adjLs[it[0]].push_back({it[1], it[2]});
    }

    // pair<int, pair<int, int>> -> <cost, <point, stops>>
    priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, greater<pair<int, pair<int, int>>>> q;
    q.push({0, {src, 0}});

    // distance array which stores both distance and stops
    vector<vector<int>> dist(n, vector<int>(k + 2, INT_MAX)); 
    dist[src][0] = 0;

    while (!q.empty()) {
        auto top = q.top();
        q.pop();

        int cost = top.first;
        int point = top.second.first;
        int stops = top.second.second;

        if (point == dst) return cost;

        if (stops > k) continue;

        for (auto& neighbor : adjLs[point]) {
            int nextPoint = neighbor.first;
            int nextCost = neighbor.second;

            if (cost + nextCost < dist[nextPoint][stops + 1]) {
                dist[nextPoint][stops + 1] = cost + nextCost;
                q.push({dist[nextPoint][stops + 1], {nextPoint, stops + 1}});
            }
        }
    }

    return -1;
}
```
________________________________

#### Ans 8.
    0. First we make a adjLs:
        adjLs = [
            0: [[4, 5], [1, 2]]
            ...
        ]

    1. Now we simply do a Djikstra 
    2. Store the distance in the distance array
    3. Store and update the ways array by checking if shortest distance is already the ways[adjNode]

```cpp
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> q;
q.push({0, 0});

vector<int> distance(n, 1e9);
distance[0] = 0;

vector<int> ways(n, 0);
ways[0] = 1;

while (q.size()) {
    auto it = q.top();
    int node = it.second;
    int dist = it.first;
    q.pop();

    if (dist > distance[node]) continue;

    for (auto adj : adjLs[node]) {
        int adjNode = adj.first;
        int adjWt = adj.second;

        if (dist + adjWt < distance[adjNode]) {
            distance[adjNode] = dist + adjWt;
            ways[adjNode] = ways[node];
            q.push({distance[adjNode], adjNode});
        }    

        else if (dist + adjWt == distance[adjNode]) {
            ways[adjNode] = (ways[adjNode] + ways[node]);
        }
    }
}

return ways[n - 1];
```
________________________________

#### Ans 9.
    Idea is basically this:
        0. Our graph will start from src and will have the next nodes as:
            a. neighbour_node = node * given element in array
        1. We don't have a "n" -> so we create a distance array of 1e6 size
        2. Now we simply do a Djikstra till we reach node == destination

```cpp
int mod = 100000;

queue<pair<int, int>> q;
q.push({0, start});

vector<int> distance(1e7, 1e9);
distance[start] = 0;

while (q.size()) {
    int node = q.front().first;
    int steps = q.front().second;
    q.pop();

    for (int num: arr) {
        int adjNode = (node * num) % mod;

        if (adjNode == end) return steps + 1;

        if (steps + 1 < distance[adjNode]) {
            distance[adjNode] = steps + 1;
            q.push({distance[adjNode], adjNode});
        }
    }
}

return -1;

```
________________________________

#### Ans 10.
    Where Dijkstra's algorithm fails:
     - If the graph contains negative edges.
     - If the graph has a negative cycle
    
    Bellman-Ford's algorithm:
     - Is only applicable for directed graphs
     - To work for undirected graphs -> we convert them to directed graph

```cpp
vector<int> dist(V, 1e8);
dist[src] = 0;

for (int i = 0; i < V - 1; i++) {
    for (auto it: edges) {
        int u = it[0];
        int v = it[1];
        int wt = it[2];
        
        if (dist[u] != 1e8 && dist[u] + wt < dist[v]) {
            dist[v] = dist[u] + wt;
        }
    }
    
}

for (auto it: edges) {
    int u = it[0];
    int v = it[1];
    int wt = it[2];
    
    if (dist[u] != 1e8 && dist[u] + wt < dist[v]) {
        return {-1};
    }
}

return dist;
```
________________________________

#### Ans 11.
    In this algorithm, we are going to use the adjacency matrix method.

    Adjacency Matrix: The adjacency matrix should store the edge weights 
    for the given edges and the rest of the cells must be initialized with infinity().

    1. After having set the adjacency matrix accordingly, we will run a loop 
    from 0 to V-1(V = no. of vertices). In the kth iteration, this loop will 
    help us to check the path via node k for every possible pair of nodes. 
    Basically, this loop will change the value of k in the formula.
    
    2. Inside the loop, there will be two nested loops for generating every  
    possible pair of nodes(nothing but to visit each cell of a 2D matrix using the nested loop). 
    Among these two loops, the first loop will change the value of i 
    and the second one will change the value of j in the formula.
    
```
matrix[i][j] = min(matrix[i][j], matrix[i][k] + matrix[k][j])
```

    3. Inside these nested loops, we will apply the above formula to calculate 
    the shortest distance between the pair of nodes.
    
    4. Finally, the adjacency matrix will store all the shortest paths. 
    For example, matrix[i][j] will store the shortest path from node i to node j.

```cpp
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        if (matrix[i][j] == -1) {
            matrix[i][j] = 1e9;
        }
        if (i == j) matrix[i][j] = 0;
    }
}

for (int k = 0; k < n; k++) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            matrix[i][j] = min(matrix[i][j], matrix[i][k] + matrix[k][j]);
        }
    }
}

for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        if (matrix[i][j] == 1e9) {
            matrix[i][j] = -1;
        }
    }
}
```
________________________________

#### Ans 12.

    1. Just do a Floyd Warshal to store the distance of each node's distance in a matrix
    2. Then all we have to do is keep a count variable -> 
        if count < cntCity 
            -> cntCity = count 
            -> cityNo = current city index

```cpp
vector<vector<int>> matrix(n, vector<int>(n, 1e9));
        
int cntCity = n;
int cityNo = -1;

for (int i = 0; i < n; i++) {
    matrix[i][i] = 0;
}

for (auto it: edges) {
    matrix[it[0]][it[1]] = it[2];
    matrix[it[1]][it[0]] = it[2];
} 

for (int k = 0; k < n; k++) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            matrix[i][j] = min(matrix[i][j], matrix[i][k] + matrix[k][j]);
        }
    }
}

for (int i = 0; i < n; i++) {
    int count = 0;
    for (int j = 0; j < n; j++) {
        if (matrix[i][j] <= distanceThreshold) {
            count++;
        }
    }
    if (count <= cntCity) {
        cntCity = count;
        cityNo = i;
    }
}

return cityNo;
```
________________________________

#### Ans 13.
    Standard Djikstra:
        1. Make a distance array
        2. Have priority_queue
        3. Return -1 if distance has a 1e9
        4. Else return max value in distance (as that would be the min time to traverse all nodes)
```cpp
int networkDelayTime(vector<vector<int>>& times, int n, int k) {
    vector<vector<pair<int, int>>> adjLs(n + 1);

    for (auto it: times) {
        adjLs[it[0]].push_back({it[1], it[2]});
    }

    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> q;
    q.push({0, k});

    vector<int> dist(n + 1, 1e9);
    dist[k] = 0;

    while (q.size()) {
        auto it = q.top();
        int node = it.second;
        int distance = it.first;
        q.pop();

        for (auto adj: adjLs[node]) {
            int adjNode = adj.first;
            int adjwt = adj.second;

            if (adjwt + distance < dist[adjNode]) {
                dist[adjNode] = adjwt + distance;
                q.push({dist[adjNode], adjNode});
            }
        }
    }

    int mini = -1e9;

    for (int i = 1; i < dist.size(); i++) {
        if (dist[i] == 1e9) return -1;
        if (i == k) continue;
        mini = max(mini, dist[i]);
    }
    
    return mini;
}
```
________________________________
