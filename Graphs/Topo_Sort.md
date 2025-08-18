## Problems:

1. [Topo Sort](#ans-1)

2. [Kahn's Algorithm](#ans-2)

3. [Cycle Detection in Directed Graph](#ans-3)

4. [Course Schedule - I](#ans-4)

5. [Find eventual safe states](#ans-5)

6. [Alien dictionary](#ans-6)

7. [Course Schedule - II](#ans-7)


## Solutions: 

#### Ans 1.
    In topological sorting, node u will always appear before node v 
    if there is a directed edge from node u towards node v(u -> v).

    Topological sorting only exists in Directed Acyclic Graph (DAG). 
    If the nodes of a graph are connected through directed edges and 
    the graph does not contain a cycle, 
    it is called a directed acyclic graph(DAG). 

    Why topological sort only exists in DAG:

    Case 1 (If the edges are undirected): 
        If there is an undirected edge between 
        node u and v, it signifies that there is an edge from 
        node u to v(u -> v) as well as there is an edge from node v to u(v -> u). 
        But according to the definition of topological sorting, 
        it is practically impossible to write such ordering 
        where u appears before v and v appears before u simultaneously. 
        So, it is only possible for directed edges.
    
    Case 2(If the directed graph contains a cycle): 
        The following directed graph contains a cycle:
        If we try to get topological sorting of this cyclic graph, 
        for edge 1->2, node 1 must appear before 2, for edge 2->3, 
        node 2 must appear before 3, and for edge 3->1, 
        node 3 must appear before 1 in the linear ordering. 
        But such ordering is not possible as there exists a cyclic dependency 
        in the graph. Thereby, topological sorting is only possible for a directed acyclic graph.

    The algorithm steps are as follows:

    1. We must traverse all components of the graph.
    
    2. Make sure to carry a visited array(all elements are initialized to 0) and a STACK data structure, 
    where we are going to store the nodes after completing the DFS call.
    
    3. In the DFS call, first, the current node is marked as visited. 
    Then DFS call is made for all its adjacent nodes.
    
    4. After visiting all its adjacent nodes, DFS will backtrack to the previous node and meanwhile, 
    the current node is pushed into the stack.
    
    5. Finally, we will get the stack containing one of the topological sortings of the graph.
```cpp
void dfs(int i, vector<int> &visited, stack<int> &stk, vector<vector<int>>& adj) {
        visited[i] = 1;
        
        for (auto it: adj[i]) {
            if (!visited[it]) {
                dfs(it, visited, stk, adj);
            }
        }
        
        stk.push(i);
    };
    
vector<int> topoSort(int V, vector<vector<int>>& edges) {
    vector<int> visited(V, 0);
    stack<int> stk;
    
    vector<vector<int>> adj(V);

    for (auto& e : edges) {
        adj[e[0]].push_back(e[1]);
    }
    
    for (int i = 0; i < V; i++) {
        if (!visited[i]) {
            dfs(i, visited, stk, adj);
        }
    }
    
    vector<int> ans;
    while (!stk.empty()) {
        ans.push_back(stk.top());
        stk.pop();
    }
    
    return ans;
}
```
________________________________
#### Ans 2.
    The indegree of a node is the number of directed edges incoming towards it.

    The algorithm steps are as follows:

    1. First, we will calculate the indegree of each node and store it in the indegree array. 
    We can iterate through the given adj list, 
    and simply for every node u->v, we can increase the indegree of v by 1 in the indegree array. 
    
    2. Initially, there will be always at least a single node whose indegree is 0. 
    So, we will push the node(s) with indegree 0 into the queue.
    
    3. Then, we will pop a node from the queue including the node in our answer array, 
    and for all its adjacent nodes, we will decrease the indegree of that node by one. 
    For example, if node u that has been popped out from the queue has an 
    edge towards node v(u->v), we will decrease indegree[v] by 1.
    
    4. After that, if for any node the indegree becomes 0, 
    we will push that node again into the queue.
    We will repeat steps 3 and 4 until the queue is completely 
    empty. Finally, completing the BFS we will get the linear ordering of the nodes in the answer array.
```cpp
vector<int> topoSort(int V, vector<vector<int>>& edges) {
    vector<vector<int>> adjLs(V);
    
    vector<int> inDegree(V, 0);
    
    for (auto it: edges) {
        adjLs[it[0]].push_back(it[1]);
    }
    
    for (int i = 0; i < V; i++) {
        for (auto it: adjLs[i]) {
            inDegree[it]++;
        }
    }
    
    queue<int> q;
    
    for (int i = 0; i < V; i++) {
        if (inDegree[i] == 0) {
            q.push(i);
        }
    }
    
    vector<int> ans;
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        ans.push_back(node);

        for (auto it : adjLs[node]) {
            inDegree[it]--;
            if (inDegree[it] == 0) q.push(it);
        }
    }

    return ans;
}
```
________________________________
#### Ans 3.
    Why visited alone is not enough:

    -> In a directed graph, a node might be visited through multiple paths. 
    Just because a node was visited earlier doesn't mean it's part of the current path that could form a cycle.

    -> So, we need two things:
        1. visited: marks nodes that have been completely processed (safe to skip later).
        2. pathVisited: marks nodes currently in the DFS call stack (current traversal path).

    What does pathVisited do?

    -> Let’s say we're doing DFS from node u, 
    and during DFS we encounter a neighbor v:
    -> If v is not visited, continue DFS normally.
    -> If v is visited:
        -> If v is in the current DFS path (i.e., pathVisited[v] == 1), 
        then there's a back edge → cycle.
        -> If pathVisited[v] == 0, 
        it's part of another traversal and already processed, so no cycle here.

```cpp
bool dfs(int node, vector<int> &visited, 
    vector<int> &pathVisited, vector<vector<int>>& adjLs) {
    
    visited[node] = 1;
    pathVisited[node] = 1;

    for (auto it : adjLs[node]) {
        if (!visited[it]) {
            if (dfs(it, visited, pathVisited, adjLs)) return true;
        }
        else if (pathVisited[it]) {
            return true;
        }
    }

    pathVisited[node] = 0;
    return false;
}

bool isCyclic(int V, vector<vector<int>> &edges) {
    vector<vector<int>> adjLs(V);
    
    for (auto it: edges) {
        adjLs[it[0]].push_back(it[1]);
    }
    
    vector<int> visited(V, 0);
    vector<int> pathVisited(V, 0);

    for (int i = 0; i < V; i++) {
        if (!visited[i]) {
            if (dfs(i, visited, pathVisited, adjLs)) {
                return true; 
            }
        }
    }

    return false;
}
```   
________________________________
#### Ans 4.
    This question is easy: if we detect a cycle: just return false:
    - Using DFS:
```cpp
bool dfs(int i, vector<int> &visited, 
vector<int> &pathVisited, vector<vector<int>> &adjLs) {
    visited[i] = 1;
    pathVisited[i] = 1;

    for (auto it: adjLs[i]) {
        if (!visited[it]) {
            if (!dfs(it, visited, pathVisited, adjLs)) return false;
        }

        else if (pathVisited[it]) return false;
    }

    pathVisited[i] = 0;
    return true;
}

bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
    vector<vector<int>> adjLs(numCourses);

    for (auto it: prerequisites) {
        adjLs[it[0]].push_back(it[1]);
    }

    vector<int> visited(numCourses, 0);
    vector<int> pathVisited(numCourses, 0);

    for (int i = 0; i < numCourses; i++) {
        if (!visited[i]) {
            if (!dfs(i, visited, pathVisited, adjLs)) {
                return false;
            }
        }
    }

    return true; 
}

```
    - Using Kahn's Algo (BFS topo sort, cycle detection)
```cpp
bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
    vector<vector<int>> adjLs(numCourses);

    for (auto it: prerequisites) {
        adjLs[it[0]].push_back(it[1]);
    }

    vector<int> inDegree(numCourses, 0);
    
    for (int i = 0; i < numCourses; i++) {
        for (auto it: adjLs[i]) {
            inDegree[it]++;
        }
    }

    queue<int> q;

    for (int i = 0; i < numCourses; i++) {
        if (inDegree[i] == 0) {
            q.push(i);
        }
    }

    int processedCourses = 0;

    while (q.size()) {
        int node = q.front();
        q.pop();
        processedCourses++;

        for (auto it: adjLs[node]) {
            inDegree[it] -= 1;
            if (inDegree[it] == 0) {
                q.push(it);
            }
        }
    }

    return processedCourses == numCourses;
}
```

________________________________
#### Ans 5.
    All we have to do is find all the nodes which are part of the cycle.

    - Using DFS:
```cpp
bool dfs(int i, vector<int> &visited,
vector<int> &pathVisited, vector<vector<int>> &adjLs
, vector<int> &ans) {
    visited[i] = 1;
    pathVisited[i] = 1;

    for (auto it: adjLs[i]) {
        if (!visited[it]) {
            if (!dfs(it, visited, pathVisited, adjLs, ans)) return false;
        }

        else if (pathVisited[it]) return false;
    }

    pathVisited[i] = 0;
    ans.push_back(i);
    return true;
}

vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
    vector<int> visited(graph.size(), 0);
    vector<int> pathVisited(graph.size(), 0);
    vector<int> ans;

    for (int i = 0; i < graph.size(); i++) {
        if (!visited[i]) {
            dfs(i, visited, pathVisited, graph, ans);
        }
    }

    sort(ans.begin(), ans.end());
    return ans; 
}
```

    - Using BFS

        All we have to do in this case is:

        1. Create a reverse graph (Destination -> Source)
        2. Have an outdegree array 
        3. Use Kahn's algo for traversing the reverse graph and marking
            safe nodes (using safe array)
        4. At last push all safe nodes to the ans.

```cpp
vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<vector<int>> reverseGraph(n);
    vector<int> outDegree(n, 0);
    vector<int> ans;

    for (int i = 0; i < n; i++) {
        for (int neighbor : graph[i]) {
            reverseGraph[neighbor].push_back(i);
            outDegree[i]++;
        }
    }

    queue<int> q;

    for (int i = 0; i < n; i++) {
        if (outDegree[i] == 0) {
            q.push(i);
        }
    }

    vector<bool> safe(n, false);

    while (!q.empty()) {
        int node = q.front();
        q.pop();
        safe[node] = true;

        for (int neighbor : reverseGraph[node]) {
            outDegree[neighbor]--;
            if (outDegree[neighbor] == 0) {
                q.push(neighbor);
            }
        }
    }

    for (int i = 0; i < n; i++) {
        if (safe[i]) {
            ans.push_back(i);
        }
    }

    return ans;
}
```
________________________________
#### Ans 6.
    So all I have to do is something like this:

```cpp
for (int i = 0; i < dict.size(); i++) {
    if (dict[i] != dict[i+1]) {
        int len = min(dict[i].size(), dict[i+1].size());
        for (int ptr = 0; ptr < len; ptr++) {
            if (dict[i][ptr] != dict[i+1][ptr]) {
                adj[dict[i][ptr] - 'a'].push_back(dict[i+1][ptr] - 'a');
                break;
            }
        }
    }
}
```

    Once the adjLs is created, all I have to do is topo sort the list and return the ans.

```cpp
vector<int> inDegree(V, 0);

for (int i = 0; i < V; i++) {
    for (auto it: adjLs[i]) {
        inDegree[it]++;
    }
}

queue<int> q;

for (int i = 0; i < V; i++) {
    if (inDegree[i] == 0) {
        q.push(i);
    }
}

vector<int> ans;
while (!q.empty()) {
    int node = q.front();
    q.pop();
    
    ans.push_back(node);

    for (auto it : adjLs[node]) {
        inDegree[it]--;
        if (inDegree[it] == 0) q.push(it);
    }
}

return ans;
```
________________________________
#### Ans 7.
    What we need to do is:

    1. We know that a course is impossible if there is a cycle in the courses.
    2. For every traversal we store the path traveled
    3. If cycle is detected and we return false:
        a. return {}
    4. Else ans.push(node)
    5. Return ans
```cpp
bool dfs(int i, vector<int> &visited,
vector<int> &pathVisited, vector<vector<int>> &adjLs
, vector<int> &ans) {
    visited[i] = 1;
    pathVisited[i] = 1;

    for (auto it: adjLs[i]) {
        if (!visited[it]) {
            if (!dfs(it, visited, pathVisited, adjLs, ans)) return false;
        }

        else if (pathVisited[it]) return false;
    }

    pathVisited[i] = 0;
    ans.push_back(i);
    return true;
}

vector<int> findOrder(int numCourses, 
vector<vector<int>>& prerequisites) {
    vector<vector<int>> adjLs(numCourses);

    for (auto it: prerequisites) {
        adjLs[it[0]].push_back(it[1]);
    }

    vector<int> visited(numCourses, 0);
    vector<int> pathVisited(numCourses, 0);
    vector<int> ans;

    for (int i = 0; i < numCourses; i++) {
        if (!visited[i]) {
            if (!dfs(i, visited, pathVisited, adjLs, ans)) {
                return {};
            }
        }
    }

    return ans; 
}
```
________________________________