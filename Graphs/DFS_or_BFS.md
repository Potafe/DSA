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
    The Algorithm for this problem involves the following steps:

    1. Firstly, we start by creating a hash set to store all the elements present in the wordList which would make the search and delete operations faster for us to implement.

    2. Next, we create a Queue data structure for storing the successive sequences/ path in the form of a vector which on transformation would lead us to the target word.

    3. Now, we add the startWord to the queue as a List and also push it into the usedOnLevel vector to denote that this word is currently being used for transformation in this particular level.

    4. Pop the first element out of the queue and carry out the BFS traversal, where for each word that popped out from the back of the sequence present at the top of the queue, we check for all of its characters by replacing them with ‘a’ - ‘z’ if they are present in the wordList or not. In case a word is present in the wordList, we simply first push it onto the usedOnLevel vector and do not delete it from the wordList immediately.

    5. Now, push that word into the vector containing the previous sequence and add it to the queue. So we will get a new path, but we need to explore other paths as well, so pop the word out of the list to explore other paths.

    6. After completion of traversal on a particular level, we can now delete all the words that were currently being used on that level from the usedOnLevel vector which ensures that these words won’t be used again in the future, as using them in the later stages will mean that it won’t be the shortest path anymore.

    7. If at any point in time we find out that the last word in the sequence present at the top of the queue is equal to the target word, we simply push the sequence into the resultant vector if the resultant vector ‘ans’ is empty.

    8. If the vector is not empty, we check if the current sequence length is equal to the first element added in the ans vector or not. This has to be checked because we need the shortest possible transformation sequences.

    9. In case, there is no transformation sequence possible, we return an empty 2D vector.
```cpp
vector<vector<string>> findLadders(string beginWord, string endWord,
    vector<string>& wordList) {
    int n = beginWord.size();
    vector<vector<string>> ans;

    unordered_set<string> st(wordList.begin(), wordList.end());

    queue<vector<string>> q;
    q.push({beginWord});
    
    vector<string> usedOnLevel;
    usedOnLevel.push_back(beginWord);
    int level = 0;

    while (!q.empty()) {
        vector<string> vec = q.front();
        q.pop();

        if (vec.size() > level) {
            level++;
            for (auto it : usedOnLevel) {
                st.erase(it);
            }
        }

        string word = vec.back();

        if (word == endWord) {
            if (ans.size() == 0) {
                ans.push_back(vec);
            }
            else if (ans[0].size() == vec.size()) {
                ans.push_back(vec);
            }
        }

        for (int i = 0; i < n; i++) {
            char original = word[i];

            for (char ch = 'a'; ch <= 'z'; ch++) {
                word[i] = ch;

                if (st.count(word) > 0) { 
                    vec.push_back(word);
                    q.push(vec);
                    usedOnLevel.push_back(word);
                    vec.pop_back();
                }
            }
            word[i] = original;
        }
    }

    return ans;    
}
```
The approach used in this solution splits the problem into two parts:

 - BFS Phase (Forward Traversal):

	1. Used to compute the shortest distance (depth) from beginWord to every reachable word in the wordList.

	2. Builds a depthMap (i.e., distance from beginWord) for each reachable word.

	3. Efficiently avoids storing full transformation paths during BFS — instead, it records only how deep each word appears in the shortest path.

-  DFS Phase (Backtracking):

	1. Once the depthMap is built, a reverse DFS is used to reconstruct all possible shortest paths from endWord to beginWord, using depth constraints to ensure only valid shortest sequences are followed.

	2. Paths are built in reverse (from endWord to beginWord) and then reversed before being added to the answer.

- 🧠 Algorithm Steps:

	1. Convert the wordList into a hash set (unordered_set) for fast lookups and deletions during BFS traversal.

	2. Initialize a queue for BFS and start from the beginWord. Push it into the queue and assign it a depth of 1 in the depthMap.

 - Perform BFS:

	1. For each word dequeued, explore all one-letter transformations.

	2. If a transformed word exists in wordSet (i.e., unvisited), enqueue it, remove it from wordSet, and assign it a depth of currentDepth + 1.

	3. This ensures each word is processed only once (when it's first reached at the shortest depth).

	4. Stop BFS once the endWord is reached, as we are only interested in the shortest paths.

	5. If endWord is found in the depthMap, start DFS:

	6. Begin with the endWord and recursively explore all previous-level words (i.e., words that appear one step before the current word in the shortest path).

	7. For each valid previous-level word, add it to the current sequence and recurse.

	8. Once the beginWord is reached, reverse the sequence and store it in the final answer.

	9. Return all shortest sequences found during DFS traversal.

- ✅ Why This Version Works Well (vs. Memory Limit Exceeded)

	1. Avoids storing full paths during BFS, unlike the previous solution that pushes entire sequences to the queue.

	2. Memory usage is controlled by keeping track only of depth levels and reconstructing valid paths after BFS.

	3. No need for a usedOnLevel list because words are immediately removed from wordSet once they’re added to the queue — this is valid because we only care about shortest paths.
```cpp
vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
    unordered_map<string, int> depthMap;
    vector<vector<string>> ans;
    
    unordered_set<string> wordSet(wordList.begin(), wordList.end());
    queue<string> q;
    q.push(beginWord);
    depthMap[beginWord] = 1;
    wordSet.erase(beginWord);
    
    while (!q.empty()) {
        string word = q.front();
        q.pop();
        int steps = depthMap[word];
        if (word == endWord) break;
        for (int i = 0; i < word.size(); ++i) {
            char original = word[i];
            for (char ch = 'a'; ch <= 'z'; ++ch) {
                word[i] = ch;
                if (wordSet.count(word)) {
                    q.push(word);
                    wordSet.erase(word);
                    depthMap[word] = steps + 1;
                }
            }
            word[i] = original;
        }
    }
    
    if (depthMap.count(endWord)) {
        vector<string> seq = {endWord};
        dfs(endWord, beginWord, seq, depthMap, ans);
    }
    
    return ans;
}

void dfs(string word, string beginWord, vector<string>& seq, unordered_map<string, int>& depthMap, vector<vector<string>>& ans) {
    if (word == beginWord) {
        reverse(seq.begin(), seq.end());
        ans.push_back(seq);
        reverse(seq.begin(), seq.end());
        return;
    }
    
    int steps = depthMap[word];
    for (int i = 0; i < word.size(); ++i) {
        char original = word[i];
        for (char ch = 'a'; ch <= 'z'; ++ch) {
            word[i] = ch;
            if (depthMap.count(word) && depthMap[word] + 1 == steps) {
                seq.push_back(word);
                dfs(word, beginWord, seq, depthMap, ans);
                seq.pop_back();
            }
        }
        word[i] = original;
    }
}
```
________________________________
#### Ans 10.
    Logic is simple:
        1. Just do a simple DFS traversal and keep track of the visited by coloring them
            alternately.
        2. If visited[i] == color -> return false
        3. Do this recursively
```cpp
bool dfs(int i, int color, vector<int> &visited,
vector<vector<int>>& graph) {
    visited[i] = color;

    for (auto it: graph[i]) {
        if (visited[it] == -1) {
            if (!(dfs(it, !color, visited, graph))) return false;
        }

        else if (visited[it] == color) return false;
    }

    return true;
}

bool isBipartite(vector<vector<int>>& graph) {
    int n = graph.size();

    vector<int> visited(n, -1);

    for (int i = 0; i < n; i++) {
        if (visited[i] == -1) {
            if (!(dfs(i, 1, visited, graph))) return false;
        }
    }   

    return true;     
}
```
________________________________