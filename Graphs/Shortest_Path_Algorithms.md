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

________________________________

#### Ans 3.

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
