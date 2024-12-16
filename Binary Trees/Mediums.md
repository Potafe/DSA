## Problems:

1. [Height of a Binary Tree](#ans-1---do-any-traversal-return-1--maxleft-right)

2. [Check if the Binary tree is height-balanced or not](#ans-2---check-absleft---right--1)

3. [Diameter of Binary Tree](#ans-3---calcualte-height-update-maxi--maxleft--right-maxi)

4. [Maximum path sum](#ans-4-calculate-maxi--root-val--left--right)

5. [Check if two trees are identical or not](#ans-5-traverse-at-same-time)

6. [Zig Zag Traversal of Binary Tree](#ans-6)

7. [Vertical Order Traversal](#ans-7-vertical-order-traversal)

8. Top View of Binary Tree

9. Bottom View of Binary Tree
	
10. Boundary Traversal of Binary Tree

11. Symmetric Binary Tree


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

    



