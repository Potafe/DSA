## Problems:

1. [Preorder Traversal of Binary Tree](#ans-1)

2. [Inorder Traversal of Binary Tree](#ans-2)

3. [Post-order Traversal of Binary Tree](#ans-3)

4. [Level order Traversal / Level order traversal in spiral form](#ans-4)

5. [Iterative Preorder Traversal of Binary Tree](#ans-5)

6. [Iterative Inorder Traversal of Binary Tree](#ans-6)

7. [Post-order Traversal of Binary Tree using 2 stack](#ans-7)

8. [Post-order Traversal of Binary Tree using 1 stack](#ans-8)

9. [Preorder, Inorder, and Postorder Traversal in one Traversal](#ans-9)

## Solutions:

#### Ans 1.

Preorder is -> root, left, right

```cpp
void pre(TreeNode* root, vector<int> &ans) {
    if (root == NULL) return;
    ans.push_back(root->val);
    pre(root->left, ans);
    pre(root->right, ans);
}
```
________________________________

#### Ans 2.

Inorder is -> left, root, right

```cpp
void pre(TreeNode* root, vector<int> &ans) {
    if (root == NULL) return;
    pre(root->left, ans);
    ans.push_back(root->val);
    pre(root->right, ans);
}
```
________________________________

#### Ans 3.

Post-order is -> left, right, root

```cpp
void pre(TreeNode* root, vector<int> &ans) {
    if (root == NULL) return;
    pre(root->left, ans);
    pre(root->right, ans);
    ans.push_back(root->val);
}
```
________________________________

#### Ans 4.

Level order Traversal is done using a queue

```cpp
vector<int> ans;
queue<TreeNode*> q;
q.push(root);

while(!q.empty()) {
    int size = q.size();
    vector<int>level;
    
    for (int i = 0; i < size; i++) {
        TreeNode* front_node = q.front();
        q.pop();

        if (front_node->left != NULL) q.push(front_node->left);
        if (front_node->right != NULL) q.push(front_node->right);

        level.push_back(front_node->val);
    }
    
    ans.push_back(level); 
}  
``` 
________________________________

#### Ans 5.

Instead of the recursive stack space we use an actual stack

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    if (root == nullptr) return {};
    vector<int> ans;

    stack<TreeNode*> stk;
    stk.push(root);

    while (!stk.empty()) {
        TreeNode* node = stk.top();
        stk.pop();

        ans.push_back(node->val);
        
        if (node->right) stk.push(node->right);
        if (node->left) stk.push(node->left);        
    }    

    return ans;
}
```

________________________________

#### Ans 6.

In this method we first fill the stack with the leftmost values and then traverse right

```cpp
vector<int> inorderTraversal(TreeNode* root) {
    if (root == nullptr) return {};  
    
    vector<int> ans;

    stack<TreeNode*> stk;
    TreeNode* current = root;

    while (current != nullptr || !stk.empty()) {
        while (current != nullptr) {
            stk.push(current);
            current = current->left;
        }

        current = stk.top();
        stk.pop();

        ans.push_back(current->val);

        current = current->right;
    }

    return ans;
}
```
________________________________

#### Ans 7.

1. We basically push everything to stack 1 in order (root, left, right)
2. Now we push everything to stack 2 in order (root, right, left)

```cpp
vector<int> postorderTraversal(TreeNode* root) {
    if (root == nullptr) return {};
    vector<int> ans;

    stack<TreeNode*> st1;
    stack<TreeNode*> st2;

    st1.push(root);

    while (st1.size()) {
        root = st1.top();
        st1.pop();
        st2.push(root);

        if (root->left != NULL) st1.push(root->left);
        if (root->right != NULL) st1.push(root->right);
    }

    while (st2.size()) {
        ans.push_back(st2.top()->val);
        st2.pop();
    }

    return ans;
}
```
________________________________

#### Ans 8.

1. We traverse the left tree first and push it into the stack
2. After left view is completed push it into ans array
3. Now we travel the right tree

```cpp
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> ans;
    if (root == nullptr) return ans;

    stack<TreeNode*> st;
    TreeNode* curr = root;
    TreeNode* lastVisited = nullptr;

    while (curr != nullptr || !st.empty()) {
        while (curr != nullptr) {
            st.push(curr);
            curr = curr->left;
        }

        TreeNode* topNode = st.top();
        if (topNode->right == nullptr || topNode->right == lastVisited) {
            ans.push_back(topNode->val);
            lastVisited = topNode;
            st.pop();
        } else {
            curr = topNode->right;
        }
    }

    return ans;
}
```
________________________________

#### Ans 9.

________________________________

