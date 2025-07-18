## Problems:

1. [Number of Provinces](#ans-1)

2. [Rotten Oranges](#ans-2)

3. [Flood fill](#ans-3)

4. [Cycle Detection in unirected Graph (BFS)](#ans-4)

5. [Cycle Detection in unirected Graph (DFS)](#ans-5)

6. [0/1 Matrix (Bfs Problem)](#ans-6)

7. [Surrounded Regions (DFS)](#ans-7)

8. [Word ladder - 1](#ans-8)

9. [Word ladder - 2](#ans-9)

10. [Bipartite Graph](#ans-10)

11. [Cycle Detection in Directed Graph](#ans-11)

## Solutions: 

#### Ans 1.
________________________________
#### Ans 2.
    So we basically need to do a BFS whenever we encounter a '2':
		also keep track of the time_counter for that '2'
		
	First of all we will create a:
		Queue = To store coordinate of Rotten Oranges
		Total_Oranges = all oranges = 7
		Count = Oranges rotten by us
		Total_Time = Total time taken to rotten.
		
	Now while our queue is not empty:
		1. We will pick up each Rotten Orange and 
		check in all its 4  directions whether a 
		Fresh orange is present or not. 
				
		2. If it is present we will make it rotten and 
		push it in our queue data structure and 
		pop out the Rotten Orange which 
		we took up as its work is done now.
		
		3. Also we will keep track of the count 
		of rotten oranges we are getting.
		
		4. If we rotten some oranges, then 
		obviously our queue will not be empty. 
		In that case, we will increase our total time. 
		This goes on until our queue becomes empty. 
		
		5. After it becomes empty, We will check 
		whether the total number of oranges 
		initially is equal to the current count of oranges. 
		If yes, we will return the total time taken, 
		else will return -1 because some fresh 
		oranges are still left and can’t be made rotten.
```cpp
int orangesRotting(vector<vector<int>>& grid) {
    int m = grid.size();
    int n = grid[0].size();

    queue<pair<int, int>> rotten;

    int Total_Oranges = 0;
    int Count = 0;
    int Total_Time = 0;
    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] != 0) Total_Oranges++;
            if (grid[i][j] == 2) rotten.push({i, j});
        }
    }  

    // Our 4 directions of Travel
    int dx[4] = {0, 0, 1, -1};
    int dy[4] = {1, -1, 0, 0};

    while (!rotten.empty()) {
        int k = rotten.size();
        Count += k;
        
        while (k--) {
            int x = rotten.front().first;
            int y = rotten.front().second;
            rotten.pop();

            for(int i = 0; i < 4; ++i){
                int nx = x + dx[i];
                int ny = y + dy[i];

                if (nx < 0 || ny < 0 || nx >= m || ny >= n || grid[nx][ny] != 1) continue;
                
                grid[nx][ny] = 2;
                rotten.push({nx, ny});
            }
        }
        
        if (!rotten.empty()) Total_Time++;
    }

    return Total_Oranges == Count ? Total_Time : -1;
}
```
________________________________
#### Ans 3.
    We can solve this using BFS and DFS:

	Using BFS:
		We maintain a:
			start_color -> to check the starting color of [sr, sc]
			if start_color == color -> return image
			Queue
		Queue.push({sr, sc})
		While Queue is non-empty:
			We traverse the 4 directions: check if they are colored:
				if not -> color them
				push the color to Queue

	Using DFS:
		Initial DFS call will start with the starting pixel (sr, sc) 
		and make sure to store the start_color. 
		
		We traverse the 4 directions: check if they are colored:
				if not -> color them
				do this recursively

BFS:
```cpp
vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {        
    int m = image.size();
    int n = image[0].size();

    int startColor = image[sr][sc];
    if (startColor == color) return image; 

    queue<pair<int, int>> colored;
    colored.push({sr, sc});
    image[sr][sc] = color;


    int dx[4] = {0, 0, 1, -1};
    int dy[4] = {1, -1, 0, 0};

    while (!colored.empty()) {
        int k = colored.size();

        while (k--) {
            int x = colored.front().first;
            int y = colored.front().second;
            colored.pop();

            for (int i = 0; i < 4; i++) {
                int nx = x + dx[i];
                int ny = y + dy[i];

                if (nx < 0 || nx >= m || ny < 0 || ny >= n || image[nx][ny] != startColor) continue;
                
                image[nx][ny] = color;
                colored.push({nx, ny});
            }
        }
    } 

    return image;
}
```

DFS:
```cpp
    void dfs (int row, int col, vector<vector<int>>& ans, vector<vector<int>>& image, 
        int color, int start_color, int dRow[], int dCol[]) {
        
        ans[row][col] = color;

        int m = image.size();
        int n = image[0].size(); 

        for (int i = 0; i < 4; i++) {
            int nrow = row + dRow[i]; 
            int ncol = col + dCol[i]; 
            
            if(nrow >= 0 && nrow < m && ncol >= 0 && ncol < n && 
            image[nrow][ncol] == start_color && ans[nrow][ncol] != color) {
                dfs(nrow, ncol, ans, image, color, start_color, dRow, dCol); 
            }
        }
    }

    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        int start_color = image[sr][sc];

        int dRow[4] = {-1, 1, 0, 0};
        int dCol[4] = {0, 0, -1, 1};

        vector<vector<int>> ans = image;

        dfs(sr, sc, ans, image, color, start_color, dRow, dCol);

        return ans;
    }
```
________________________________
#### Ans 4.
    To check a cycle we do:
            Create Adjacency List for the given Edges
            A BFS traversal:
                Maintain a Queue, Visisted Array
                If visted and parent != adjacentNode return true
```cpp
void makeAdj(int V, vector<vector<int>>& edges, vector<vector<int>> &adjLs) {
    adjLs.resize(V);

    for (auto &edge : edges) {
        int u = edge[0];
        int v = edge[1];

        adjLs[u].push_back(v);
        adjLs[v].push_back(u);
    }
}

bool bfsCheckCycle(int src, vector<vector<int>> &adjLs, vector<int> &vis) {
    queue<pair<int, int>> q;
    vis[src] = 1;
    q.push({src, -1});

    while (!q.empty()) {
        int node = q.front().first;
        int parent = q.front().second;
        q.pop();

        for (int adjacent_node : adjLs[node]) {
            if (!vis[adjacent_node]) {
                vis[adjacent_node] = 1;
                q.push({adjacent_node, node});
            }
            else if (adjacent_node != parent) {
                return true;
            }
        }
    }
    return false;
}

bool isCycle(int V, vector<vector<int>>& edges) {
    vector<vector<int>> adjLs;
    makeAdj(V, edges, adjLs);

    vector<int> vis(V, 0);

    for (int i = 0; i < V; i++) {
        if (!vis[i]) {
            if (bfsCheckCycle(i, adjLs, vis)) {
                return true;
            }
        }
    }

    return false;
}
```
________________________________
#### Ans 5.
    All we have to is recursion:
        1. Keep track of visited array for the node
        2. If adjacent_node != parent and visited = true -> return true
```cpp
bool dfs(int node, int parent, int vis[], vector<int> adj[]) {
    vis[node] = 1; 

    for(auto adjacentNode: adj[node]) {
        
        if(!vis[adjacentNode]) {
            if(dfs(adjacentNode, node, vis, adj) == true) 
                return true; 
        }

        else if(adjacentNode != parent) return true; 
    }
    return false; 
}
```
________________________________
#### Ans 6.
    Solution is simple:

	First keep a visited array:
		-> this keeps track of all '0' visited points in the grid
	
	Now do a BFS Traversal:
		-> init a Queue -> which keeps -> row, col, steps
		-> if the cell is not visited:
			-> push to queue with step + 1
			-> visited[row][col] = true
		-> In the answer grid mark ans[row][col] = steps
```cpp
vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
    int m = mat.size();
    int n = mat[0].size();

    vector<vector<int>> ans(m, vector<int>(n, 0)); 
    
    //visited array and queue (which stores row, col, steps)
    vector<vector<int>> visited(m, vector<int>(n, 0)); 
    queue<pair<pair<int, int>, int>> q;

    //mark all 0s as visited and push to queue with steps = 0
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (mat[i][j] == 0) {
                q.push({{i, j}, 0});
                visited[i][j] = 1;
            }
        }
    }

    //4 directions of traverse
    int dRow[4] = {-1, 1, 0, 0};
    int dCol[4] = {0, 0, -1, 1};

    while (!q.empty()) {
        auto front = q.front();
        int x = front.first.first;
        int y = front.first.second;
        int steps = front.second;
        q.pop();

        ans[x][y] = steps;

        for (int i = 0; i < 4; i++) {
            int nx = x + dRow[i];
            int ny = y + dCol[i];

            if (nx < 0 || ny < 0 || nx >= m || ny >= n || visited[nx][ny] == 1) continue;

            q.push({{nx, ny}, steps + 1});
            visited[nx][ny] = 1;
        }
    }

    return ans;
}
```
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