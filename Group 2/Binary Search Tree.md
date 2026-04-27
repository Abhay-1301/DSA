# ***Binary Search Tree***

### ***Introduction to BST***

- Binary Tree. Optimize the operation of searching an element.
- `Left Child < Node < Right Child` for every node.
- Every subtree is itself a Binary Search Tree.
- Duplicate node values are not allowed generally.
- Why need BST instead of a simple BT ?
    - Generally in a BST, the maximum height in almost all cases is kept in order of log(N) base 2, where N = No. of nodes which is in contrast to the plain BT whose maximum height can reach the order of N when the tree is skewed. This particularly makes the time of searching for a given node in a BST a lot less than searching in a simple BT.

### ***Search in a Binary Search Tree***

- Given a BST and a key value return the node in the BST having data equal to ‘key’ otherwise return nullptr.
- ```cpp
    struct TreeNode {
        int val;
        TreeNode* left;
        TreeNode* right;

        TreeNode(int data) {
            val = data;
            left = right = nullptr;}};

    TreeNode* searchBST(TreeNode* root, int target) {
        while (root != nullptr && root->val != target) {
            if (target < root->val) root = root->left;
            else { root = root->right; }
        }
        return root;}

    int main(){
        TreeNode* root = new TreeNode(4);
        root->left = new TreeNode(2);
        root->right = new TreeNode(7);
        root->left->left = new TreeNode(1);
        root->left->right = new TreeNode(3);

        TreeNode* result = searchBST(root, 2);
    }
    ```

### ***Floor & Ceil in a BST

### ***Floor in a BST***

- Given a Binary Search Tree and a key, return the floor of the given key in the BST. (Floor of a value refers to the value of the largest node in the BST that is smaller than or equal to the given key. If the floor node does not exist, return nullptr.)
- ```cpp
    int floorInBST(TreeNode* root, int key){
        int floor = -1;
        while(root){
            if(root->val == key){floor = root->val;return floor;}
            if(key > root->val){floor = root->val;root = root->right;}
            else{root = root->left;}
        }
        return floor;
    }
    ```

### ***Insert a Given Node

### ***Delete a Given Node

### ***Kth Smallest and Largest Element***

- Given the root node of a BST and an integer k. Return the kth smallest and largest value (1-indexed) of all values of the nodes in the tree. Return the 1st integer as kth smallest and 2nd integer as kth largest in the returned array.

- Brute Force TC = O(N), SC = O(N). DFS. Inorder. Sorted. Return values based on K.

- ```cpp
    void inorderTraversal(TreeNode* node, vector<int>& values) {
        if (node) {
            inorderTraversal(node->left, values);
            values.push_back(node->data);
            inorderTraversal(node->right, values);}}
    
    vector<int> kLargesSmall(TreeNode* root, int k) {
        vector<int> values;
        inorderTraversal(root, values);
        int kth_smallest = values[k - 1];
        int kth_largest = values[values.size() - k];
        return {kth_smallest, kth_largest};}
    ```

- Optimal TC = O(N), SC = O(H). In the worst-case scenario, the inorder and reverse inorder traversals visit each node exactly once. Inorder. Use a counter to monitor the progress of the traversal, especially focusing on the Kth position. This eliminates the need for an additional data structure.

- ```cpp
    vector<int> kLargesSmall(TreeNode* root, int k) {
        return {kthSmallest(root, k), kthLargest(root, k)};
    }

    int kthSmallest(TreeNode* root, int k) {
        this->k = k;
        this->result = -1;
        inorder(root);
        return result;
    }

    int kthLargest(TreeNode* root, int k) {
        this->k = k;
        this->result = -1;
        reverse_inorder(root);
        return result;
    }

    int k;
    int result;

    void inorder(TreeNode* node) {
        if (node != nullptr) {
            inorder(node->left);
            if (--k == 0) {
                result = node->data;
                return;
            }
            inorder(node->right);
        }
    }

    void reverse_inorder(TreeNode* node) {
        if (node != nullptr) {
            reverse_inorder(node->right);
            if (--k == 0) {
                result = node->data;
                return;
            }
            reverse_inorder(node->left);
        }
    }
    ```


### ***Check if Tree is BST or not

### ***LCA in BST

### ***Construct a BST from a preorder traversal

### ***Inorder Successor/Predecessor in BST

### ***Merge 2 BST's

### ***Two Sum In BST | Check if there exists a pair with Sum K

### ***Correct BST with two nodes swapped

### ***Largest BST in Binary Tree