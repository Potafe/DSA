## Problems:

1. [Height of a Binary Tree](#ans-1---do-any-traversal-return-1--maxleft-right)

2. [Check if the Binary tree is height-balanced or not](#ans-2---check-absleft---right--1)

3. [Diameter of Binary Tree](#ans-3---calcualte-height-update-maxi--maxleft--right-maxi)

4. [Maximum path sum](#ans-4-calculate-maxi--root-val--left--right)

5. [Check if two trees are identical or not](#ans-5-traverse-at-same-time)

6. [Zig Zag Traversal of Binary Tree](#ans-6)

7. [Vertical Order Traversal](#ans-7-vertical-order-traversal)

8. [Top View of Binary Tree](#ans-8)

9. [Bottom View of Binary Tree](#ans-9)
	
10. [Boundary Traversal of Binary Tree](#ans-10)

11. [Symmetric Binary Tree](#ans-11)

12. [Right & Left View](#ans-12)


## Solutions:

#### Ans 1. 
    -> do any traversal, return 1 + max(left, right)
```cpp
    int height(root) {

        if root = nullptr return 0;

        int left = height(root->left);
        int right = height(root->right);

        return 1 + max(left, right)
    }
```
________________________________
#### Ans 2. 
    -> check abs(left - right) == 1
```cpp
    int traverse(root) {
        if root = nullptr return 0;

        int left = height(root->left);
        if (left == -1) return -1;
        int right = height(root->right);
        if (right == -1) return -1;

        if (abs(left - right) > 1) return -1;
    
        return 1 + max( left, right)
    }

    bool isBalanced(root) {
        return traverse(root) != -1;
    }
```
________________________________
#### Ans 3. 
    -> calcualte height, update maxi = max(left + right, maxi)

```cpp
    int maxi = 0;

    int traverse(root) {
        if root == nullptr return 0;

        left = height(root->left);
        right = height(root->right);

        maxi = max(maxi, left+right);

        return 1 + max(left, right);
    }

    int diameterOfBinaryTree(root) {
        int x = height(root);
        return maxi;    
    }
```
________________________________
#### Ans 4. 
    calculate maxi = root->val + left + right
```cpp
    int maxi = INT_MIN;

    int traverse(root) {
        if root == nullptr return 0;
                
        //Postorder -- L, R, Ro

        int left = max(0, traverse(root->left));
        int right = max(0, traverse(root->right));

        maxi = max(maxi, root->val + left + right);
                
        return root->val + max(left, right);
    }
    int maxPathSum(root) {
        int x = traverse(root);
        return maxi;    
    }
```
________________________________
#### Ans 5.
     Traverse at same time 

```cpp
    bool isSameTree(p, q) {
        if (p == nullptr && q == nullptr) return 1;

        if (p == nullptr || q == nullptr) return 0;
                
        return (p->val == q->val) && isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
    }
```
________________________________
#### Ans 6. 
    Do level order, keep a flag which turns off after every for loop iteration ends, add element to level vector by logic :  

        (index of addition = flag ? i : size - i - 1)

```cpp
    vector<vector<int>> LOT(root) {
        vector<vector<int>> ans;

        if (root == nullptr) return ans; 
        queue<NODE> q;
        q.push(root);
        
        //FLAG IS HERE
        bool flag = 1;

        while(!q.empty()) {
            int size = q.size();
            vector<int> level(size);
            
            for (int i = 0; i < size; i++) {
                node = q.front();
                q.pop();
                
                //FINDING INDEX OF ADDITION LOGIC IS HERE
                int idx = flag ? i : (size - 1 - i);

                level[idx] = node->val;

                if(node->left != NULL) q.push(node->left);
                if(node->right != NULL) q.push(node->right);
            }

            //FLAG FLIPPED AFTER EVERY FOR LOOP
            flag = !flag;

            ans.push_back(level);
        }

        return ans;
    }
```
________________________________
#### Ans 7.  
    Has a little fucked up solution to it, requires me to memorize somethings, namely:
        
        map<int, map<int, multiset<int>>>
        queue<pair<NODE>, pair<int>>
```cpp
/// WILL WRITE TOMORROW TO CHECK MEMORY;
```
________________________________
#### Ans 8.
    Idea is simple store the level and the node in a map, since its top view, dont update the node value in map if another node of same val is found
```cpp
    vector<int> topView(root) {
        if (root == NULL) return {};
        
        queue<pair<NODE, int>> q;
        vector<int> ans;
        map<int, int> mp;
        
        q.push({root, 0});
        
        while(!q.empty()) {
            int size = q.size();
            
            for (int i = 0; i < size; i++) {
                auto p = q.front();
                q.pop();
                
                node = p.first;
                int level = p.second;
                
                if (node->left) q.push({node->left, level - 1});
                if (node->right) q.push({node->right, level + 1});
                
                if(mp.find(level) == mp.end()) {
                    mp.insert({level, node->data});
                }
            }
        }
        
        
        for (const auto& pair : mp) {
            ans.push_back(pair.second);
        }
        
        return ans;
    }
```
_______________________________
#### Ans 9.
    This time just update the value of the node upon reaching a level visited before.
```cpp
    vector <int> bottomView(root) {
        // code here
        if (root == NULL) return {};
        
        queue<pair<NODE, int>> q;
        vector<int> ans;
        map<int, int> mp;
        
        q.push({root, 0});
        
        while(!q.empty()) {
            int size = q.size();
            
            for (int i = 0; i < size; i++) {
                auto p = q.front();
                q.pop();
                
                Node* node = p.first;
                int level = p.second;
                
                if (node->left) q.push({node->left, level - 1});
                if (node->right) q.push({node->right, level + 1});
                
                mp[level] = node->data;
            }
        }
        
        
        for (const auto& pair : mp) {
            ans.push_back(pair.second);
        }
        
        return ans;
    }
```
________________________________

#### Ans 10.
    first do left traversal -> 
            Move to the left child if it exists,
            Otherwise move to the right child
    
    then do leaf traversals ->
        do inorder traversal of leafes and their roots
    
    then do right traversals -> 
            Move to the right child if it exists,
            Otherwise move to the left child
```cpp

    bool tell(*root) {
        return (root->left == NULL && root->right == NULL);    
    }
    
    void left(*root, vector<int> &ans) {
        Node* curr = root->left;
        
        while(curr) {
            
            if (!tell(curr)) ans.push_back(curr->data);
            
            if (curr->left) curr = curr->left;
            else curr = curr->right; 
        }
    }
        
    void leaf(*root, vector<int> &ans) {
        if (tell(root)) {
            // cout << " child: " << root->data;
            ans.push_back(root->data);
            return;
        }
        
        else {
            // cout << " root: " << root->data ; 
            if (root->left) leaf(root->left, ans);
            if (root->right) leaf(root->right, ans);
        }
        // right(root->right, ans);
        // cout << root->right->data <<  " ";
    }
    
    void right(*root, vector<int> &ans) {
        Node* curr = root->right;
        vector<int> temp;
         
        while (curr) {
            // If the current node is not a leaf,
            // add its value to a temporary vector
            if (!tell(curr)) {
                temp.push_back(curr->data);
            }
            // Move to the right child if it exists,
            // otherwise move to the left child
            if (curr->right) {
                curr = curr->right;
            } else {
                curr = curr->left;
            }
        }
        // Reverse and add the values from
        // the temporary vector to the result
        for (int i = temp.size() - 1; i >= 0; --i) {
            ans.push_back(temp[i]);
        }    
    }
    
    vector<int> boundaryTraversal(*root) {
        // code here
        if (root==NULL) return {};
        
        vector<int> ans;
        
        if(!tell(root)) ans.push_back(root->data);
        
        left(root, ans);
        leaf(root, ans);
        right(root, ans);
        return ans;
    }

```
________________________________
#### Ans 11. 
    Check if BT is mirror image, by doing traversal on the root's left and right child
```cpp
    bool sym(root1, root2) {
        if (root1 == NULL || root2 == NULL) return root1 == root2;

        return (root1->val == root2->val) && sym(root1->left, root2->right) && sym(root2->left, root1->right);
    }

    bool isSymmetric(root) {
        if (root == NULL) return true;

        return sym(root->left, root->right);    
    }
```
________________________________

#### Ans 12.
    Solution 1: Do BFS and add the back values from the BFS vector

    Solution 2: // WILL CHECK LATER -> RELATED TO RECURSION 
```cpp
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;

        if (!root) {
            return ans;
        }

        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            int size = q.size();
            vector<int> level;

            for (int i = 0; i < size; i++) {
                TreeNode* top = q.front();
                level.push_back(top->val);
                q.pop();

                if (top->left != NULL) {
                    q.push(top->left);
                }

                if (top->right != NULL) {
                    q.push(top->right);
                }
            }
            ans.push_back(level);
        }
        return ans;
    }

    vector<int> rightSideView(TreeNode* root) {
        vector<int> res;

        vector<vector<int>> levelTraversal = levelOrder(root);

        for (auto level : levelTraversal) {
            res.push_back(level.back());
        }

        return res;    
    }
```
________________________________
   



