## Some Basics of BTs:

#### Types:    
    1. FULL --> 0 OR 2 CHILDREN
    2. COMPLETE --> ALL LEVELS FILLED 
    3. PERFECT --> ALL LEAFS AT SAME LEVEL
    4. BALANCED --> ABS(LEFT - RIGHT) == 1 or O(logN) traversal
    5. DEGENERATE --> SKEW
________________________________

#### Traversals:
    1. PRE  | => Ro, L, R
    2. POST | => L, R, Ro
    3. IN   | => L, Ro, R
    4. BFS/LEVEL ORDER => Queue 


##### Pseudocodes:

* PRE PSEUDOCODE:
    * RECURSIVE WAY:
    ```
        void preorder(root) {
            if root == null return 0;
            print root->val;
            preorder(root->left);
            preorder(root->right);
        }
    ```
    * ITERATIVE WAY:
    ```
    //Use a stack
    ```
________________________________
* POST PSEUDOCODE:
    * RECURSIVE WAY:
    ```
        void postorder(root) {
            if root == null return 0;
            postorder(root->left);
            postorder(root->right);
            print root->val;
        }
    ```
    * ITERATIVE WAY:
    ```
    //Use 2 stacks
    ```

    ```
    //Use 1 stacks
    ```
________________________________

* INORDER PSEUDOCODE:
    * RECURSIVE WAY:
    ```
        void inorder(root) {
            if root == null return 0;
            inorder(root->left);
            print root->val;
            inorder(root->right);
        }
    ```
    * ITERATIVE WAY:
    ```
    //Use a stack
    ```
________________________________
* LEVEL ORDER PSEUDOCODE:
    * BFS:
    ```
    //USE A QUEUE

    void bfs(root) {
        
        vector<int>vec;
        queue<root> q;
        q.add(root)'

        while(!q.empty()) {
            front_node = q.front();
            q.pop();

            if (front_node->left != null) q.add(front_node->left)
            if (front_node->right != null) q.add(front_node->right) 

            vec.push(front_node -> val);
        }
    }
    ```
________________________________