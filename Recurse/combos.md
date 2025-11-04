## Problems:

1. [Palindrome Partitioning](#ans-1)
2. [Word Search](#ans-2)
3. [N Queen](#ans-3)
4. [Rat in a Maze](ans-4)
5. [Word Break](#ans-5)
6. [M Coloring Problem](#ans-6)
7. [Sudoko Solver](#ans-7)
8. [Expression Add Operators](#ans-8)

## Answers:

#### Ans 1.

We basically do this:

1. We loop from the current_index till s.size().
2. We check if the substring from current_index to the loop_index is a palindrome or not:
   -> If it is -> we push that to our copy array and move to the next index.

   -> If it is not -> we skip that index.

```cpp
bool check_party(string s, int start, int end) {
    while (start <= end) {
        if (s[start] != s[end]) return false;
        start++;
        end--;
    }

    return true;
}

void recurse(vector<vector<string>> &ans, vector<string> &copy, int index,
    string s) {
    if (index >= s.size()) {
        ans.push_back(copy);
        return;
    }

    for (int i = index; i < s.size(); i++) {
        if (check_party(s, index, i)) {
            copy.push_back(s.substr(index, i - index + 1));
            recurse(ans, copy, i + 1, s);
            copy.pop_back();
        }
    }
}

vector<vector<string>> partition(string s) {
    vector<vector<string>> ans;
    vector<string> copy;

    recurse(ans, copy, 0, s);

    return ans;
}
```

---

#### Ans 2.

The solution is based on backtracking recursion:

1. We loop through all letters in the board and then backtrack recursively from those letter to form the word.
2. The base is case is basically divided into 2 types:
   a. First if the index has reached the length of the word.
   b. Second if we have travelled out of the board or the current letter is not part of the word
3. We then recursively backtrack in all possible directions to try to form the word.

```cpp
bool backtrack_recurse(vector<vector<char>> &board, string word,
int index, int row, int col, int m, int n)
{
    if (index >= word.size()) return true;

    if (row < 0 || col < 0 ||
        row >= m || col >= n
        || board[row][col] != word[index]) return false;

    char temp = board[row][col];
    board[row][col] = ' '; // => marked cell as visited.

    bool found = backtrack_recurse(board, word, index + 1, row + 1, col, m, n) ||
            backtrack_recurse(board, word, index + 1, row - 1, col, m, n) ||
            backtrack_recurse(board, word, index + 1, row, col + 1, m, n) ||
            backtrack_recurse(board, word, index + 1, row, col - 1, m, n);

    board[row][col] = temp; // => restore original value
    return found;
}

bool exist(vector<vector<char>>& board, string word) {
    int m = board.size();
    int n = board[0].size();

    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            if (backtrack_recurse(board, word, 0, i, j, m, n)) return true;
        }
    }

    return false;
}
```

---

#### Ans 3.

We basically do this:

1. We mantain an array of strings (all strings are "." $$*$$ n)
   `	-> so array will look like this:
	[
		["....","....","....","...."]
	]`
2. No we recursively check for every col if we can place a queen in the respective rows or not
3. We do so by recursively checking the columns and looping through the rows to check if we can place a queen through a helper method (which checks all possible ways)
4. If we reach the last column then that means that the copy array has now been completed and we can push this array into our column

```cpp
bool can_place_queen(vector<string> &copy, int row, int col, int n) {
    int duprow = row;
    int dupcol = col;

    // check diagonal top-left -- bottom right
    while (row >= 0 && col >= 0) {
        if (copy[row][col] == 'Q')
        return false;
        row--;
        col--;
    }

    col = dupcol;
    row = duprow;

    // check upwards
    while (col >= 0) {
        if (copy[row][col] == 'Q')
        return false;
        col--;
    }

    row = duprow;
    col = dupcol;

    // check diagonal top-right -- bottom-left
    while (row < n && col >= 0) {
        if (copy[row][col] == 'Q')
        return false;
        row++;
        col--;
    }

    return true;
}

void recurse(vector<vector<string>> &ans, vector<string> &copy,
int col, int n)
{
    if (col == n) {
        ans.push_back(copy);
        return;
    }

    for (int row = 0; row < n; row++) {
        if (can_place_queen(copy, row, col, n)) {
            copy[row][col] = 'Q';
            recurse(ans, copy, col + 1, n);
            copy[row][col] = '.';
        }
    }
}

vector<vector<string>> solveNQueens(int n) {
    vector<vector<string>> ans;
    vector<string> copy(n, string(n, '.'));

    recurse(ans, copy, 0, n);

    return ans;
}
```

---

#### Ans 4.

The problem is simple:

1. We just have to recursively travel all the available paths.

-> Major point to note was the marking of cell as visited.

```cpp
void recurse(vector<vector<int>>& maze, vector<string> &ans,
    int n, int i, int j, string traversal_string)
{
    // we define the base cases first
    if (i == n - 1 && j == n - 1) {
        ans.push_back(traversal_string);
        return;
    }

    if (i < 0 || i >= n || j < 0 || j >= n) return;

    // now we have to define the recursion
    // to traverse all the paths
    // and store the path direction in traversal_string

    int temp = maze[i][j];
    maze[i][j] = 0; // Mark as visited

    // up
    if (i > 0 && maze[i - 1][j] == 1) {
        recurse(maze, ans, n, i - 1, j, traversal_string + 'U');
    }

    // down
    if (i < n - 1 && maze[i + 1][j] == 1) {
        recurse(maze, ans, n, i + 1, j, traversal_string + 'D');
    }

    // left
    if (j > 0 && maze[i][j - 1] == 1) {
        recurse(maze, ans, n, i, j - 1, traversal_string + 'L');
    }

    // right
    if (j < n - 1 && maze[i][j + 1] == 1) {
        recurse(maze, ans, n, i, j + 1, traversal_string + 'R');
    }

    maze[i][j] = temp;
}

vector<string> ratInMaze(vector<vector<int>>& maze) {
    // code here
    vector<string> ans;
    recurse(maze, ans, maze.size(), 0, 0, "");


    sort(ans.begin(), ans.end());

    return ans;
}
```

---

#### Ans 5.

---

#### Ans 6.

---

#### Ans 7.

---

#### Ans 8.
