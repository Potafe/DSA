## Problems:

1. [Root to Node Path in Binary Tree](#ans-1)

2. [LCA in Binary Tree](#ans-2)

3. [Maximum width of a Binary Tree](#ans-3)

4. [Check for Children Sum Property](#ans-4)

5. [Print all the Nodes at a distance of K from given node in a Binary Tree](#ans-5)

6. [Count total Nodes in a COMPLETE Binary Tree](#ans-6)

7. [Minimum time taken to BURN the Binary Tree from a Node](#ans-7)

8. [Construct Binary Tree from inorder and preorder](#ans-8)

9. [Construct the Binary Tree from Postorder and Inorder Traversal](#ans-9)

10. [Serialize and deserialize Binary Tree](#ans-10)

11. [Flatten Binary Tree to LinkedList](#ans-11)

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
    The Lowest Common Ancestor of two nodes p and q in a binary tree is 
    the lowest node in the tree that has both p and q as descendants.
    
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


#### Ans 7.
    Since there is a start node -> most probably we will have to do both backward and forward
    traversals, so just make a graph first.

    Now what we do is do a BFS traversal while keeping in mind the time.

    Each time we process the queue -> we increment the time.

```cpp
void genGraph(TreeNode* root, unordered_map<int, vector<int>> &adjLs) {
    if (!root) return;

    if (root->left) {
        adjLs[root->data].push_back(root->left->data);
        adjLs[root->left->data].push_back(root->data);
        genGraph(root->left, adjLs);
    }

    if (root->right) {
        adjLs[root->data].push_back(root->right->data);
        adjLs[root->right->data].push_back(root->data);
        genGraph(root->right, adjLs);
    }
}

int timeToBurnTree(TreeNode* root, int start){
    unordered_map<int, vector<int>> adjLs;
    genGraph(root, adjLs);

    unordered_map<int, bool> visited;
    queue<int> q;
    q.push(start);
    visited[start] = true;
    
    int time = -1;
    while (!q.empty()) {
        int size = q.size();
        time++;
        for (int i = 0; i < size; ++i) {
            int node = q.front(); 
            q.pop();
            
            for (int neighbor : adjLs[node]) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    q.push(neighbor);
                }
            }
        }
    }
    
    return time;
}	
```
________________________________

#### Ans 8.
    We have the preorder array to get the roots and the inorder 
    array to get the connections (ie left and right)

    What we do is basically this:

    1. Make the root node from the preorder array
    2. Get the root node's location in inorder array
    3. Make the left and right inorder and preorder array 
        a. left inorder will be from begin to root node location
        b. left preorder will be from preorder begin to left inorder size

        c. right inorder will be from root node location to end of inorder array
        d. right preorder will be from left inorder end to end of preorder array

            Example:
            
            preorder = [3,9,20,15,7], 
            inorder = [9,3,15,20,7]

            root = 20
            left_inorder = [9, 3, 15]
            left_preorder = [9, 20, 15]

            right_inorder = [7]
            right_preorder = []

            Now we easily can get the left and right connections 


```cpp
TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    if (preorder.empty() || inorder.empty()) return nullptr;

    TreeNode* root = new TreeNode(preorder[0]);

    // now we find the location of this root->val in inorder array
    // to construct the left and right preorder and inorder arrays
    int rootIndex = 0;
    for (int i = 0; i < inorder.size(); i++) {
        if (inorder[i] == root->val) {
            rootIndex = i;
            break;
        }
    }

    vector<int> leftInorder(inorder.begin(), inorder.begin() + rootIndex);
    vector<int> leftPreorder(preorder.begin() + 1, preorder.begin() + 1 + leftInorder.size());
    // this gives us the root->left's preorder and inorder
    
    vector<int> rightPreorder(preorder.begin() + 1 + leftInorder.size(), preorder.end());
    vector<int> rightInorder(inorder.begin() + rootIndex + 1, inorder.end());
    // this gives us the root->right's preorder and inorder

    root->left = buildTree(leftPreorder, leftInorder);
    root->right = buildTree(rightPreorder, rightInorder);


    return root;
}	
```
________________________________

#### Ans 9.
    We have the postorder array to get the roots and the inorder 
    array to get the connections (ie left and right)

    What we do is basically this:

    1. Make the root node from the postorder array
    2. Get the root node's location in inorder array
    3. Make the left and right inorder and postorder array 
        a. left inorder will be from begin to root node location
        b. left postorder will be from postorder begin to rootIndex

        c. right inorder will be from root node location to end of inorder array
        d. right postorder will be from rootIndex to second last element of postorder array

            Example:
            
            postorder = [3,9,20,15,7], 
            inorder = [9,3,15,20,7]

            root = 20
            left_inorder = [9, 3, 15]
            left_postorder = [9, 20, 15]

            right_inorder = [7]
            right_postorder = []

            Now we easily can get the left and right connections 


```cpp
TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
    if (postorder.empty() || inorder.empty()) return nullptr;

    TreeNode* root = new TreeNode(postorder[postorder.size() - 1]);

    // now we find the location of this root->val in inorder array
    // to construct the left and right postorder and inorder arrays
    int rootIndex = 0;
    for (int i = 0; i < inorder.size(); i++) {
        if (inorder[i] == root->val) {
            rootIndex = i;
            break;
        }
    }

    vector<int> leftInorder(inorder.begin(), inorder.begin() + rootIndex);
    vector<int> leftPostorder(postorder.begin(), postorder.begin() + rootIndex);
    // this gives us the root->left's postorder and inorder
    
    vector<int> rightInorder(inorder.begin() + rootIndex + 1, inorder.end());
    vector<int> rightPostorder(postorder.begin() + rootIndex, postorder.end() - 1);
    // this gives us the root->right's postorder and inorder

    root->left = buildTree(leftInorder, leftPostorder);
    root->right = buildTree(rightInorder, rightPostorder);


    return root;        
}
```
________________________________

#### Ans 10.
    The question is basically BT construction from a level order array

    What we basically do is:

    1. Push to queue the root which is array[0]
    2. Now we traverse the array from index i = 1
    3. pop the current node in queue
    4. node->left = array[index], q.push(node->left), i++
    5. node->right = array[index], q.push(node->right), i++

```cpp
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if (!root) return "null";
        
        string ans;
        
        queue<TreeNode*> q;
        q.push(root);

        while (q.size()) {
            int size = q.size();
            
            for (int i = 0; i < size; i++) {
                TreeNode* node = q.front();
                q.pop();

                if (node) {
                    ans.append(to_string(node->val) + ",");
                    q.push(node->left);
                    q.push(node->right);
                }

                else ans.append("null,");     
            }
        }

        return ans;
    }

    vector<string> split(const string& data) {
        vector<string> result;
        string temp;
        for (char ch : data) {
            if (ch == ',') {
                if (!temp.empty()) result.push_back(temp);
                temp.clear();
            } else if (ch != ' ' && ch != '"') {
                temp += ch;
            }
        }
        if (!temp.empty()) result.push_back(temp);
        return result;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        vector<string> nodes = split(data);
        if (nodes.empty() || nodes[0] == "null") return nullptr;

        TreeNode* root = new TreeNode(stoi(nodes[0]));
        queue<TreeNode*> q;
        q.push(root);

        int i = 1;
        while (i < nodes.size()) {
            TreeNode* current = q.front();
            q.pop();

            if (i < nodes.size() && nodes[i] != "null") {
                current->left = new TreeNode(stoi(nodes[i]));
                q.push(current->left);
            }
            i++;

            if (i < nodes.size() && nodes[i] != "null") {
                current->right = new TreeNode(stoi(nodes[i]));
                q.push(current->right);
            }
            i++;
        }

        return root;
    }
};
```
________________________________

#### Ans 11.
    COMPLETE NON INTUTIVE (FOR ME AT FIRST):

    1. We recursively traverse the left and right tree
    2. We set the left part of root as NULL
    3. We set the right part of root as the root's left
    4. We set the right part of the above left (root's left) as the original right of the root

```cpp
void flatten(TreeNode* root) {
    if (!root) return;

    flatten(root->left);
    flatten(root->right);

    TreeNode* left = root->left;
    TreeNode* right = root->right;

    root->left = NULL;
    root->right = left;

    TreeNode* curr = root;
    while (curr->right) {
        curr = curr->right;
    }
    curr->right = right;    
}
```
________________________________

