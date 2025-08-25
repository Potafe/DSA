## Problems:

1. [Ceil in a Binary Search Tree](#ans-1)
2. [Floor in a Binary Search Tree](#ans-2)
3. [Insert a given Node in Binary Search Tree](#ans-3)
4. [Delete a Node in Binary Search Tree](#ans-4)
5. [Find K-th smallest/largest element in BST](#ans-5)
6. [Check if a tree is a BST or BT](#ans-6)
7. [LCA in Binary Search Tree](#ans-7)
8. [Construct a BST from a preorder traversal](#ans-8)
9. [Inorder Successor/Predecessor in BST](#ans-9)
10. [Merge 2 BST's](#ans-10)
11. [Two Sum In BST | Check if there exists a pair with Sum K](#ans-11)
12. [Recover BST | Correct BST with two nodes swapped](#ans-12)
13. [Largest BST in Binary Tree](#ans-13)

## Solutions:

#### Ans 1.
    What we basically do is:
        1. Travel the given BST.
        2. If we find root->val == key 
            -> we assign that the floor and ceil.
        3. Else if root->val > key 
            -> that is the ceil.
            -> we now travel left. (Go left to find a smaller possible ceil.)
        4. Else if root->val < key 
            -> that is the floor.
            -> we now travel right. (Go right to find a bigger possible floor.)

```cpp
void find(vector<int> &ans, TreeNode* root, int key) {
    int floor = -1;
    int ceil = -1;

    while (root) {
        if (root->val == key) {
            ceil = root->val;
            floor = root->val;
            break;
        }

        if (root->val > key) {
            ceil = root->val;
            root = root->left;
        } else {
            floor = root->val;
            root = root->right;
        }
    }   

    ans.push_back(ceil);
    ans.push_back(floor);
}

vector<int> floorCeilOfBST(TreeNode* root,int key){
    vector<int> ans;
    find(ans, root, key);
    return ans;
}
```
_______________________________________________________________

#### Ans 2.
    SOLUTION ABOVE.
_______________________________________________________________

#### Ans 3.
    What we basically do is:
        1. Assign root->left if val < root->val.
            -> Traverse left recursively.
        2. Assign root->right if val > root->val.
            -> Traverse right recurisvely.
        3. Return root.

```cpp
TreeNode* insertIntoBST(TreeNode* root, int val) {
    if (root == NULL) {
        TreeNode* root1 = new TreeNode(val);
        return root1;   
    }

    if (val > root->val) root->right = insertIntoBST(root->right, val);
    if (val < root->val) root->left = insertIntoBST(root->left, val);

    return root;
}
```
_______________________________________________________________

#### Ans 4.
    What we basically do is this:
    
    1. If root->val > key -> delete the left node.
    2. If root->val < key -> delete the right node.
    3. If root->val == key
        -> if it's a leaf node (!root->left && !root->right):
            -> return NULL.
        -> if it has a child:
            -> return the right node as the new parent of the relation
            -> we then delete the right node
    4. Return root

    DRY RUN:

    Example 1:

    [9,5,10,2,6,null,12,null,null,null,7]
	root>val = 9, key = 5
	
	-> root>val > key -> root->left = deleteNode(root->left, 5)
	-> root>val = 5 -> root->val == key:
		-> has a right child = 6 -> return 6
	-> root->left = 6 (for this call root->left = deleteNode(root->left, 5))

    Example 2:
    root = [5,3,6,2,4,null,7], key = 3

    -> root>val > key -> root->left = deleteNode(root->left, 3)
	-> root>val = 3 -> root->val == key:
		-> has a right child and left child:
            -> temp = 4
            -> root->val = 4
            -> root->right = deleteNode(4, 4)
                -> return NULL
            -> root->right = NULL
    -> return root

```cpp
TreeNode* deleteNode(TreeNode* root, int key) {
    if (root) {
        if (root->val > key) {
            root->left = deleteNode(root->left, key);
        }

        else if (root->val < key) {
            root->right = deleteNode(root->right, key);
        }

        else {
            if (!root->left && !root->right) return NULL;

            if (!root->left || !root->right) {
                return root->right ? root->right : root->left;
            } 

            TreeNode* temp = root->right;

            while (temp->left) {
                temp = temp->left;
            }

            root->val = temp->val;
            root->right = deleteNode(root->right, temp->val);
        }
    }    

    return root;
}
```
_______________________________________________________________

#### Ans 5.
_______________________________________________________________

#### Ans 6.
_______________________________________________________________

#### Ans 7.
_______________________________________________________________

#### Ans 8.
_______________________________________________________________

#### Ans 9.
_______________________________________________________________

#### Ans 10.
_______________________________________________________________

#### Ans 11.
_______________________________________________________________

#### Ans 12.
_______________________________________________________________

#### Ans 13.
_______________________________________________________________

