## Problems:

1. [Root to Node Path in Binary Tree](#ans-1)

2. [LCA in Binary Tree](#ans-2)

3. Maximum width of a Binary Tree

4. [Check for Children Sum Property](#ans-4)

5. Print all the Nodes at a distance of K in a Binary Tree

6. Construct Binary Tree from inorder and preorder

7. Construct the Binary Tree from Postorder and Inorder 
Traversal

8. Flatten Binary Tree to LinkedList

## Solutions: 

#### Ans 1.

    Very Basic and fuck off solution:
        1. traverse the tree and add nodes to the tree.
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
________________________________
#### Ans 2.
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
