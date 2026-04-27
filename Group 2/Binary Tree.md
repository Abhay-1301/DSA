# ***Binary Tree***

# Part 1: Traversals


Introduction to Trees:
Array, LL, Stack & Queues were fundamental linear DS. Binary Trees are fundamental heirarchical DS. 
File system in our phone or laptop works as Binary Tree.
Root Nodes (entry point top node), Children Nodes (Directly connected to parent node) , Leaf Nodes (that do not have children), Ancestors (nodes that lie in the path from that node to root node)
Full binary tree: every node has either zero or two children. Makes traversal, search, insertion more predictable and efficient. 
Complete binary tree: All level filled except possibly last level. And Last level filled from left to right. Useful for storing data in heap. Makes implementation of algorithms efficient.
Perfect Binary Tree: All leaf nodes at same level and no of leaf nodes for that level is maximised. 
Balanced Binary Tree: Height of two subtree of any node differ by at most one. Ensures tree is balanced and not highly skewed or degenerate. Height of tree should be log N base 2 where N is number of nodes.
Degenerate Tree: Imagine just left or just right insertion. Makes it like linked list. Inefficient for search operation. Each level has one node. Height of it reaches N (no of nodes in tree). This should be learnt t handle worst case scenario of tree.


Binary Tree Representation
Class Node{ int data, Node* left, Node* right}
Preorder, Inorder, Postorder Traversals
SEE THE SOLUTION AND UNDERSTAND BY WRITING BECAUSE IN ONE TRAVERSAL ITS COMPLICATED.
Recursive Pre order Traversal
Return if root = NULL; ans.push_back(root->data); preorder(root->left, ans), preorder(root->right, ans)
Recursive In order Traversal
Return if root = NULL; inorder(root->left, ans); ans.push_back(root->data); inorder(root->right, ans)
Recursive Post order Traversal
Return if root = NULL; postorder(root->left, ans), postorder(root->right, ans); ans.push_back(root->data)
Level Order Traversal
temp = q.front() ; q.pop() ; cout << data ; q.push_left if not null ; q.push_right if not null
Iterative Pre order Traversal
SEE THE CODE OF ITERATIVE APPROACH
Iterative In order Traversal
SEE THE CODE OF ITERATIVE APPROACH
Iterative Post order Traversal (Using 2 Stacks)
SEE THE CODE OF ITERATIVE APPROACH
Iterative Post order Traversal (Using 1 Stack)
This is just recursive solution. CHECK ONCE THOUGH IS IT CORRECT OR NOT FORMATTED PROPER LINK ON STRIVER.

## Part 2: Medium Problems


Maximum Depth in BT
Check for balanced BT
Diameter of BT
Maximum Path Sum
Check if two trees are identical or not
Zig Zag or Spiral Traversal
Boundary Traversal
Vertical Order Traversal
Top View of BT
Bottom View of BT
Right/Left View of BT
Symmetric BT

## Part 3: Hard Problems

### ***Print Root to Node Path in a Binary Tree***

- Given a Binary Tree and a reference to a root belonging to it. Return the path from the root node to the given leaf node. Note: No two nodes in the tree have the same data value and it is assured that the given node is present and a path always exists. 

- Maybe apply DFS anc keep track of path ???? Start from root node. Push to an array. Go left. Push to array. Go left. Push to array. Left is NULL. Go to right. push to array. No left or right then pop back after returning. At any point of time if matches the values. End recursion and print array. I still can not think of code. I am bad.

- ```cpp
    bool getPath(TreeNode* root, vector<int>& arr, int x) {
        if (!root) return false;
        arr.push_back(root->val);
        if (root->val == x) return true;

        if (getPath(root->left, arr, x) || getPath(root->right, arr, x)) return true;
        arr.pop_back();
        return false;
    }

    vector<int> solve(TreeNode* A, int B) {
        vector<int> arr;
        if (A == NULL) return arr;
        getPath(A, arr, B);
        return arr;
    }
    ```

### ***Lowest Common Ancestor for two given Nodes***

- Given a root of binary tree, find the lowest common ancestor (LCA) of two given nodes (p, q) in the tree. The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).

- We are going to start from root anyway. We find the path to Node A & Node B. We iterate through this both array and whatever is last matching node is my answer. Okkk. I am wrong. Simple code in left and right subtree.

- ```cpp
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == NULL || root == p || root == q) return root;
        
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        
        if (left == NULL) return right;
        else if (right == NULL) return left;
        else { return root; } // Both left and right are not null, we found our result
    }
    ```

### ***Maximum Width of a Binary Tree***

- Given a Binary Tree, return its maximum width. The maximum width of a Binary Tree is the maximum diameter among all its levels. The width or diameter of a level is the number of nodes between the leftmost and rightmost nodes.

- Width = width of left + width of right ??? Again wrong. Apply BFS.

- ```cpp
    int widthOfBinaryTree(TreeNode* root) {
        if (!root) return 0;
        int maxWidth = 0;
        queue<pair<TreeNode*, int>> q; // Each element is a pair of node and its index
        q.push({root, 0});

        while (!q.empty()) {
            int size = q.size();
            int minIndex = q.front().second;
            int first = 0; int last = 0;

            for (int i = 0; i < size; i++) {
                int currIndex = q.front().second - minIndex;
                TreeNode* node = q.front().first;
                q.pop();

                if (i == 0) first = currIndex;
                if (i == size - 1) last = currIndex;

                if (node->left) q.push({node->left, 2 * currIndex + 1});
                if (node->right) q.push({node->right, 2 * currIndex + 2});
            }
            maxWidth = max(maxWidth, last - first + 1);
        }
        return maxWidth;
    }
    ```

### ***Check for Children Sum Property in a Binary Tree***

- Given a Binary Tree, convert the value of its nodes to follow the Children Sum Property. The Children Sum Property in a binary tree states that for every node, the sum of its children's values (if they exist) should be equal to the node's value. If a child is missing, it is considered as having a value of 0. (The node values can be increased by any positive integer any number of times, but decrementing any node value is not allowed. A value for a NULL node can be assumed as 0. We cannot change the structure of the given binary tree.)

- Recursively find sum of left and right subtree and sum it to update root. Base case is if null return 0. So if it comes at leaf node. Return will be node value. This wont work because, if a node is a left or right child and its parent is of a greater value, since we cannot decrease the value of the parent, we increase the value of the children nodes. Also see the example it's not clear what is happening at leaf node.

- ```cpp
    void changeTree(TreeNode* root) {
        if (root == NULL) return;

        int child = 0;
        if (root->left) child += root->left->val;
        if (root->right) child += root->right->val;

        if (child >= root->val root->val = child;
        else {
            if (root->left) root->left->val = root->val;
            else if (root->right) root->right->val = root->val;
        }

        changeTree(root->left);
        changeTree(root->right);

        int tot = 0;
        if (root->left) tot += root->left->val;
        if (root->right) tot += root->right->val;
        if (root->left or root->right) root->val = tot;
    }
    ```

### ***Print all the Nodes at a distance of K in a Binary Tree***

- Given the root of a binary tree, the value of a target node target, and an integer k. Return an array of the values of all nodes that have a distance k from the target node. The answer can be returned in any order (N represents null).

- Here we can not do this using only tree and we must treat it like an undirected graph to handle upward connectivity. Since we can not change structure of Tree Node, we have to maintain parent array by traversing the graph first.

- Then simply do a BFS from target Node and get all nodes from it at distance K. Lets see the code.

- ```cpp
    vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
        if (!root) return {};
        unordered_map<TreeNode*, TreeNode*> parentMap;
        mapParentNodes(root, parentMap);
        return bfsFromTarget(target, parentMap, k);
    }

    void mapParentNodes(TreeNode* root, unordered_map<TreeNode*, TreeNode*>& parentMap) {
        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            TreeNode* node = q.front();
            q.pop();

            if (node->left) {
                parentMap[node->left] = node;
                q.push(node->left);
            }

            if (node->right) {
                parentMap[node->right] = node;
                q.push(node->right);
            }
        }
    }

    vector<int> bfsFromTarget(TreeNode* target, unordered_map<TreeNode*, TreeNode*>& parentMap, int k) {
        queue<TreeNode*> q;
        unordered_set<TreeNode*> visited; 

        q.push(target);
        visited.insert(target);

        int currentLevel = 0;

        while (!q.empty()) {
            int size = q.size();
            if (currentLevel++ == k) break;

            for (int i = 0; i < size; ++i) {
                TreeNode* node = q.front(); q.pop();

                if (node->left && visited.find(node->left) == visited.end()) {
                    visited.insert(node->left);
                    q.push(node->left);
                }

                if (node->right && visited.find(node->right) == visited.end()) {
                    visited.insert(node->right);
                    q.push(node->right);
                }

                if (parentMap.count(node) && visited.find(parentMap[node]) == visited.end()) {
                    visited.insert(parentMap[node]);
                    q.push(parentMap[node]);
                }
            }
        }

        vector<int> result;
        while (!q.empty()) {
            result.push_back(q.front()->val);
            q.pop();
        }

        return result;
    }
    ```

### ***Minimum time taken to BURN the Binary Tree from a Node***

- Given a target node data and a root of binary tree. If the target is set on fire, determine the shortest amount of time needed to burn the entire binary tree. It is known that in 1 second all nodes connected to a given node get burned. That is its left child, right child, and parent.

- Here again since the tree can be burnt upwards. We need to first convert it into graph. And then BFS will tell us seconds to burn.

- ```cpp
    int minTime(TreeNode* root, int target) {
        unordered_map<int, vector<int>> graph;
        buildGraph(root, nullptr, graph);
        unordered_set<int> visited;

        queue<int> q;
        q.push(target);
        visited.insert(target);
        int time = 0;

        while (!q.empty()) {
            int size = q.size();
            bool burned = false;

            for (int i = 0; i < size; i++) {
                int node = q.front();
                q.pop();

                for (int neighbor : graph[node]) {
                    if (!visited.count(neighbor)) {
                        visited.insert(neighbor);
                        q.push(neighbor);
                        burned = true;
                    }
                }
            }
            if (burned) time++;
        }
        return time;
    }

    void buildGraph(TreeNode* node, TreeNode* parent, unordered_map<int, vector<int>>& graph) {
        if (!node) return;

        if (parent) {
            graph[node->val].push_back(parent->val);
            graph[parent->val].push_back(node->val);
        }

        buildGraph(node->left, node, graph);
        buildGraph(node->right, node, graph);
    }
    ```

### ***Count Number of Nodes in a Complete Binary Tree***

- Given a Complete Binary Tree, count and return the number of nodes in the given tree. A Complete Binary Tree is a binary tree in which all levels are completely filled, except possibly for the last level, and all nodes are as left as possible.

- Brute Force is in O(N). Just traverse using inorder and return count of nodes.

- Can we make use of fact that it's Complete Binary Tree to find optimally ?

- Go left left left untill there is NULL to know the level/height. But how to track the leaf nodes ?

- Ok so the solution is go left left left and go right right right. if left equal right height then answer is 2^h-1. If no then recursively return 1 + lefttreeNodes + righttree nodes. Lets see the code. It is O(logN * logN). 

- ```cpp
    int countNodes(TreeNode* root) {
        if (root == NULL) return 0;

        int lh = findHeightLeft(root);
        int rh = findHeightRight(root);

        if (lh == rh) { return (1 << lh) - 1; } // Use formula: 2^h - 1

        return 1 + countNodes(root->left) + countNodes(root->right); // Otherwise, recursively count left and right subtrees
    }

    int findHeightLeft(TreeNode* node) {
        int height = 0;
        while (node) {
            height++;
            node = node->left;
        }
        return height;
    }

    int findHeightRight(TreeNode* node) {
        int height = 0;
        while (node) {
            height++;
            node = node->right;
        }
        return height;
    }
    ```

### Requirement Needed to construct a unique BT

### ***Construct A Binary Tree from Inorder and Preorder Traversal***

- Given the Preorder and Inorder traversal of a Binary Tree, construct the Unique Binary Tree represented by them.

- ```cpp
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        map<int, int> inMap;
        for (int i = 0; i < inorder.size(); i++) { inMap[inorder[i]] = i; }
        return build(preorder, 0, preorder.size() - 1, inorder, 0, inorder.size() - 1, inMap);}

    TreeNode* build(vector<int>& preorder, int preStart, int preEnd, vector<int>& inorder, int inStart, int inEnd, map<int, int>& inMap) {
        
        if (preStart > preEnd || inStart > inEnd) return nullptr; // Base condition
        TreeNode* root = new TreeNode(preorder[preStart]); // The first element in preorder is root

        int inRoot = inMap[root->val]; // Find the root index in inorder
        int numsLeft = inRoot - inStart; // Number of elements in left subtree

        root->left = build(preorder, preStart + 1, preStart + numsLeft, inorder, inStart, inRoot - 1, inMap);
        root->right = build(preorder, preStart + numsLeft + 1, preEnd, inorder, inRoot + 1, inEnd, inMap);

        return root;
    }
    ```

### ***Construct Binary Tree from Inorder and PostOrder Traversal***

- Given the Postorder and Inorder traversal of a Binary Tree, construct the Unique Binary Tree represented by them.

- ```cpp
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (inorder.size() != postorder.size()) return nullptr;
        map<int, int> hm;
        for (int i = 0; i < inorder.size(); i++) { hm[inorder[i]] = i; }
        return build(inorder, 0, inorder.size() - 1, postorder, 0, postorder.size() - 1, hm);
    }

    TreeNode* build(vector<int>& inorder, int is, int ie, vector<int>& postorder, int ps, int pe, map<int, int>& hm) {
        if (ps > pe || is > ie) return nullptr;

        TreeNode* root = new TreeNode(postorder[pe]); // Last element in postorder is root
        int inRoot = hm[postorder[pe]]; // Find root index in inorder
        int numsLeft = inRoot - is;

        root->left = build(inorder, is, inRoot - 1, postorder, ps, ps + numsLeft - 1, hm);
        root->right = build(inorder, inRoot + 1, ie, postorder, ps + numsLeft, pe - 1, hm);

        return root;
    }
    ```

### Serialize And Deserialize a Binary Tree

- Given a Binary Tree, design an algorithm to serialise and deserialise it. There is no restriction on how the serialisation and deserialization takes place. But it needs to be ensured that the serialised binary tree can be deserialized to the original tree structure. Serialisation is the process of translating a data structure or object state into a format that can be stored or transmitted (for example, across a computer network) and reconstructed later. The opposite operation, that is, extracting a data structure from stored information, is deserialization.

### Morris Preorder Traversal of a Binary Tree

### Morris Inorder Traversal of a Binary Tree

### Flatten Binary Tree to Linked List


