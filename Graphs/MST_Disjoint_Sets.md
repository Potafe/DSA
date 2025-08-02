## Problems:

1. [Minimum Spanning Tree](#ans-1)
2. [Prim's Algorithm](#ans-2)
3. [Disjoint Set \[Union by Rank\]](#ans-3)
4. [Disjoint Set \[Union by Size\]](#ans-4)
5. [Kruskal's Algorithm](#ans-5)
6. [Number of operations to make network connected](#ans-6)
7. [Most stones removed with same rows or columns](#ans-7)
8. [Accounts merge](#ans-8)
9. [Number of islands II](#ans-9)
10. [Making a Large Island](#ans-10)
11. [Swim in rising water](#ans-11)


## Solutions:

#### Ans 1. Minimum Spanning Tree
    A spanning tree is a tree in which we have N nodes
    and N-1 edges and all nodes are reachable from each other.

    Among all possible spanning trees of a graph, the minimum spanning 
    tree is the one for which the sum of all the edge weights is the minimum.

    There may exist multiple minimum spanning trees for 
    a graph like a graph may have multiple spanning trees.
________________________________

#### Ans 2. Prim's Algorithm
    Prim’s algorithm: help us to find the minimum spanning tree for a given graph.

    1. We will first push edge weight 0, node value 0, 
    and parent -1 as a triplet into the priority queue to start the algorithm.

    2. Then the top-most element (element with minimum edge weight 
    as it is the min-heap we are using) of the priority queue is popped out.

    3. After that, we will check whether the popped-out node is visited or not.

    4. If the node is visited: We will continue to the next element of the priority queue.

    5. If the node is not visited: We will mark the node visited in the 
    visited array and add the edge weight to the sum variable. If we wish 
    to store the mst, we should insert the parent node and the 
    current node into the mst array as a pair in this step.

    6. Now, we will iterate on all the unvisited adjacent nodes of 
    the current node and will store each of their information in 
    the specified triplet format i.e. (edge weight, node value, and parent node) in the priority queue.

    7. We will repeat steps 2, 3, and 4 using a loop until the priority queue becomes empty.

    8. Finally, the sum variable should store the sum of all the edge weights of the minimum spanning tree.

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

    for (auto nebor: adjLs[node]) {
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

#### Ans 3. Disjoint Set \[Union by Rank\]
________________________________

#### Ans 4. Disjoint Set \[Union by Size\]
________________________________

#### Ans 5. Kruskal's Algorithm
________________________________

#### Ans 6. Number of operations to make network connected
________________________________

#### Ans 7. Most stones removed with same rows or columns
________________________________

#### Ans 8. Accounts merge
________________________________

#### Ans 9. Number of islands II
________________________________

#### Ans 10. Making a Large Island
________________________________

#### Ans 11. Swim in rising water
________________________________
