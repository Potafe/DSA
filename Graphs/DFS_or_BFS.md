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
Using DFS:
    The algorithm steps are as follows:

    1.Create a corresponding visited matrix and initialize it to 0.

    2. Start with boundary elements, once ‘O’ is found, call the DFS function for that element and mark it as visited. In order to traverse for boundary elements, you can traverse through the first row, last row, first column, and last column. 

    3. DFS function call will run through all the unvisited neighboring ‘O’s in all 4 directions and mark them as visited so that they are not converted to ‘X’ in the future. The DFS function will not be called for the already visited elements to save time, as they have already been traversed. 

    4. When all the boundaries are traversed and corresponding sets of ‘O’s are marked as visited, they cannot be replaced with ‘X’. All the other remaining unvisited ‘O’s are replaced with ‘X’. This can be done in the same input matrix as the problem talks about replacing the values, otherwise tampering with data is not advised. 
```cpp
void dfs(int row, int col, vector<vector<int>> &visited,
    vector<vector<char>>& board, int delRow[], int delCol[]) {
    visited[row][col] = 1;

    int m = board.size();
    int n = board[0].size();

    for (int i = 0; i < 4; i++) {
        int nRow = row + delRow[i];
        int nCol = col + delCol[i];

        if (nRow < 0 || nCol < 0 || nRow >= m || nCol >=n ||
        visited[nRow][nCol] == 1 
        || board[nRow][nCol] == 'X') continue;

        dfs(nRow, nCol, visited, board, delRow, delCol);
    }

}

void solve(vector<vector<char>>& board) {
    int m = board.size();
    int n = board[0].size();
    
    vector<vector<int>> visited(m, vector<int>(n, 0));   
    
    int delRow[] = {-1, 1, 0, 0};
    int delCol[] = {0, 0, 1, -1}; 

    for (int col = 0; col < n; col++) {
        if (!visited[0][col] && board[0][col] == 'O') {
            dfs(0, col, visited, board, delRow, delCol); 
        }

        if (!visited[m-1][col] && board[m-1][col] == 'O') {
            dfs(m-1, col, visited, board, delRow, delCol); 
        }
    }

    for (int row = 0; row < m; row++) {
        if (!visited[row][0] && board[row][0] == 'O') {
            dfs(row, 0, visited, board, delRow, delCol); 
        }

        if (!visited[row][n-1] && board[row][n-1] == 'O') {
            dfs(row, n-1, visited, board, delRow, delCol); 
        }
    }

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (!visited[i][j] && board[i][j] == 'O') {
                board[i][j] = 'X';
            } 
        }
    }
}
```

Using BFS: same idea but with BFS
```cpp
void bfs(int row, int col, vector<vector<int>> &visited,
    vector<vector<char>>& board, int delRow[], int delCol[], int m, int n) {
    queue<pair<int, int>> q;
    q.push({row, col});
    visited[row][col] = 1;
    
    while (!q.empty()) {
        auto it = q.front();
        int r = it.first;
        int c = it.second;
        q.pop();
        
        for (int i = 0; i < 4; i++) {
            int nRow = r + delRow[i];
            int nCol = c + delCol[i];

            if (nRow < 0 || nCol < 0 || nRow >= m || nCol >=n ||
            visited[nRow][nCol] == 1 
            || board[nRow][nCol] == 'X') continue;

            q.push({nRow, nCol});
            visited[nRow][nCol] = 1;
        }
    }
}

void solve(vector<vector<char>>& board) {
    int m = board.size();
    int n = board[0].size();
    
    vector<vector<int>> visited(m, vector<int>(n, 0));   
    
    int delRow[] = {-1, 1, 0, 0};
    int delCol[] = {0, 0, 1, -1}; 

    for (int col = 0; col < n; col++) {
        if (!visited[0][col] && board[0][col] == 'O') {
            bfs(0, col, visited, board, delRow, delCol, m, n); 
        }

        if (!visited[m-1][col] && board[m-1][col] == 'O') {
            bfs(m-1, col, visited, board, delRow, delCol, m, n); 
        }
    }

    for (int row = 0; row < m; row++) {
        if (!visited[row][0] && board[row][0] == 'O') {
            bfs(row, 0, visited, board, delRow, delCol, m, n); 
        }

        if (!visited[row][n-1] && board[row][n-1] == 'O') {
            bfs(row, n-1, visited, board, delRow, delCol, m, n); 
        }
    }

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (!visited[i][j] && board[i][j] == 'O') {
                board[i][j] = 'X';
            } 
        }
    }
}
```
________________________________
#### Ans 8.
    The Algorithm involves the following steps:

    Firstly, we start by creating a queue data structure in order to store the word and the length of the sequence to reach that word as a pair. We store them in form of {word, steps}.

    Then, we push the startWord into the queue with length as ‘1’ indicating that this is the word from which the sequence needs to start from.

    We now create a hash set wherein, we put all the elements in wordList to keep a check on if we’ve visited that word before or not. In order to mark a word as visited here, we simply delete the word from the hash set. There is no point in visiting someone multiple times during the algorithm.

    Now, we pop the first element out of the queue and carry out the BFS traversal where, for each word popped out of the queue, we try to replace every character with ‘a’ - ‘z’, and we get a transformed word. We check if the transformed word is present in the wordList or not.

    If the word is present, we push it in the queue and increase the count of the sequence by 1 and if not, we simply move on to replacing the original character with the next character.

    Remember, we also need to delete the word from the wordList if it matches with the transformed word to ensure that we do not reach the same point again in the transformation which would only increase our sequence length.

    Now, we pop the next element out of the queue ds and if at any point in time, the transformed word becomes the same as the targetWord, we return the count of the steps taken to reach that word. Here, we’re only concerned about the first occurrence of the targetWord because after that it would only lead to an increase in the sequence length which is for sure not minimum.

    If a transformation sequence is not possible, return 0.

```cpp
int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
    int n = beginWord.size();

    queue<pair<string, int>> q;
    q.push({beginWord, 1});

    unordered_set<string> st(wordList.begin(), wordList.end());
    st.erase(beginWord);

    while (!q.empty()) {
        auto it = q.front();
        string word = it.first;
        int steps = it.second;

        q.pop();

        if (word == endWord) return steps;

        for (int i = 0; i < n; i++) {
            char original = word[i];

            for (char ch = 'a'; ch <= 'z'; ch++) {
                word[i] = ch;

                if (st.find(word) != st.end()) {
                    st.erase(word);
                    q.push({word, steps + 1});
                }
            }
            word[i] = original;
        }
    }

    return 0;
}
```
________________________________
#### Ans 9.
________________________________
#### Ans 10.
________________________________
#### Ans 11.
________________________________