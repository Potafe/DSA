## Problems:

1. [Root to Node Path in Binary Tree](#ans-1)

2. [LCA in Binary Tree](#ans-2)

3. [Maximum width of a Binary Tree](#ans-3)

4. [Check for Children Sum Property](#ans-4)

5. [Print all the Nodes at a distance of K from given node in a Binary Tree](#ans-5)

6. [Count total Nodes in a COMPLETE Binary Tree](#ans-6)

7. Minimum time taken to BURN the Binary Tree from a Node

8. Construct Binary Tree from inorder and preorder

9. Construct the Binary Tree from Postorder and Inorder 
Traversal

10. Serialize and deserialize Binary Tree

11. Flatten Binary Tree to LinkedList

## Solutions: 

#### Ans 1.

    Very Basic and fuck off solution:
        1. depth traverse the tree and add nodes to the tree.
        2. if reach leaf node -> add it to ans vec<vec>
        3. always pop back the added elements after traversal

```cpp  
    bool tell(Node* root) {
        return (root->left == NULL && root->right == NULL);
    }
    
    void traverse(Node* root, vector<vector<int>>& ans, vector<int>& depth) {
        if (root == NULL) return;
        
        if (tell(root)) {
            depth.push_back(root->data);
            ans.push_back(depth);
            depth.pop_back();
            return;
        }
        
        depth.push_back(root->data);
        traverse(root->left, ans, depth);
        traverse(root->right, ans, depth);
        depth.pop_back();
    }
    
    vector<vector<int>> Paths(Node* root) {
        vector<int> depth;
        vector<vector<int>> ans;
        traverse(root, ans, depth);
        return ans;
    }
```

The above solution is for printing all the paths to leaf node

The below solution is for printing Root to Given Node Path in Binary Tree

```cpp
bool getPath(TreeNode* A, vector<int> ans, int B) {
    if (A == NULL) return false
    
    ans.push(A -> val)

    if (A->val == B) return true

    if (getPath(A->left, ans, B) or getPath(A->right, ans, B)) return true // travel both left and right

    ans.pop()

    return false
}

vector<int> Root_to_Given_Node_Path(TreeNode* A, int B) {
        vector<int> ans;

        if (A == NULL) return ans

        getPath(A, ans, B);

        return ans;
    }
```
________________________________
#### Ans 2.
The Lowest Common Ancestor of two nodes p and q in a binary tree is the lowest node in the tree that has both p and q as descendants.
A node can be a descendant of itself.

Remember we have to return the lowest common ANCESTOR of the 2 nodes (it cant be the one in the middle).

    Clever solution, maine nahi socha tha yeh:

        1. Just return the node you are travelling if the node is either of the destination node, else return null;
        
```cpp
    Node* lowestCommonAncestor(Node* root, Node* p, Node* q) {
        if(root == NULL || root == p || root == q){
            return root;
        }

        Node* left = lowestCommonAncestor(root->left, p, q);
        
        Node* right = lowestCommonAncestor(root->right, p, q);

        if(left == NULL){
            return right;
        }

        else if(right == NULL){
            return left;
        }
        
        return root;
    }
```
________________________________
#### Ans 3.
    We do a level order traversal and assign each node the correct level according to rule:
        left = (2 * current_level + 1)
        right = (2 * current_level + 2)

```cpp
int widthOfBinaryTree(TreeNode* root) {
    if (root == NULL) return 0;

    int ans = 0;

    queue<pair<TreeNode*, int>> q;
    q.push({root, 0});

    while (!q.empty()) {
        int size = q.size();
        auto front = q.front();
        int front_level = front.second;

        int first, last;

        for (int i = 0; i < size; i++) {
            TreeNode* front_node = q.front().first;
            long long current_level = q.front().second - front_level;
            q.pop();

            if (i == 0) first = current_level;
            if (i == size - 1) last = current_level;    

            if (front_node->left) q.push({front_node->left, 2 * current_level + 1});
            if (front_node->right) q.push({front_node->right, 2 * current_level + 2});
        }

        ans = max(last - first + 1, ans);
    }       

    return ans;
}
```
________________________________
#### Ans 4.
    Recurse over left and right:
        1. Check and calculate the left and right.
        2. Return 1 if sum of childs = root->data.
```cpp
     int isSumProperty(Node *root) {
     
        if (root == NULL || (root->left == NULL && root->right == NULL)) {
            return 1;
        }
        
        int child = 0;
        if (root->right) child += root->right->data;
        if (root->left) child += root->left->data;
        
        if (child == root->data) {
            if (isSumProperty(root->left) == 1 && isSumProperty(root->right) == 1) {
                return 1;
            }
        }
        return 0;
    }
```
________________________________
#### Ans 5.
    Since we have to find all the nodes at a distance k -> the question resembles a graph
    where we can travel in all directions.

    So we construct a graph from the tree using DFS.

    Then we do a BFS traversal of the graph keeping the steps in mind, when the steps hit "k"
    we push that node to ans.

```cpp
void buildGraph(TreeNode* root, unordered_map<int, vector<int>>& adj) {
    if (!root) return;

    if (root->left) {
        adj[root->val].push_back(root->left->val);
        adj[root->left->val].push_back(root->val);
        buildGraph(root->left, adj);
    }

    if (root->right) {
        adj[root->val].push_back(root->right->val);
        adj[root->right->val].push_back(root->val);
        buildGraph(root->right, adj);
    }
}

vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
    unordered_map<int, vector<int>> adj;
    buildGraph(root, adj);

    int steps = 0;
    vector<int> ans;

    queue<pair<int, int>> q;
    q.push({target->val, 0});
    unordered_set<int> visited;
    visited.insert(target->val);


    while (q.size()) {
        auto [node, step] = q.front();
        q.pop();

        if (step == k) {
            ans.push_back(node);
            continue;
        }

        for (auto negbors: adj[node]) {
            if (!visited.count(negbors)) {
                visited.insert(negbors);
                q.push({negbors, step + 1});
            }
        }
    }

    return ans;
}
```
________________________________
#### Ans 6.
    SOLUTION 1:
        One way is to just do a BFS and then return the total size of the whole array (THIS CODE IS NOT WRITTEN)
    SOLUTION 2:
        Check if the last level is completely filled:
            If so, use the formula for total nodes in a perfect binary tree ie. 2^h - 1
            If the last level is not completely filled, recursively count nodes in left and right subtrees
```cpp
    int leftHeight(TreeNode* root) {
        int height = 0;

        while (root) {
            height++;
            root = root->left;
        }

        return height;
    }

    int rightHeight(TreeNode* root) {
        int height = 0;

        while (root) {
            height++;
            root = root->right;
        }

        return height;
    }

    int countNodes(TreeNode* root) {
        if (root == NULL) return 0;

        int left = leftHeight(root);
        int right = rightHeight(root);

        if (right == left) {
            return (1 << left) - 1; 
        }

        return 1 + countNodes(root->left) + countNodes(root->right);
    }
```
________________________________