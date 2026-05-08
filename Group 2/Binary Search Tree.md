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

### ***Ceil in a BST***

- Given a Binary Search Tree and a key, return the ceiling of the given key in the Binary Search Tree. Ceiling of a value refers to the value of the smallest node in the Binary Search Tree that is greater than or equal to the given key. If the ceiling node does not exist, return nullptr.

- The strategy to find the ceil value is to keep track of the smallest node value encountered that is greater than or equal to the key. Traverse the tree recursively and move through it until it reaches the end or locates the key. During the traversal, at every node, if the key matches the node’s values, it directly assigns the node’s value as the ceiling and concludes the search.

- If the key is greater than the current node’s value, the algorithm navigates to the right subtree to potentially find a larger value and if the key is smaller the algorithm updates the ceil value with the current node’s values and explores the left subtree for potentially smaller values.

- ```cpp
    int findCeil(TreeNode* root, int key){
        int ceil = -1; 
        while(root){
            if(root->val == key){
                ceil = root->val;
                return ceil;
            }
            if(key > root->val) { root = root->right; }
            else {
                ceil = root->val;
                root = root->left;
            }
        }
        return ceil;
    }
    ```

### ***Insert a Given Node***

- Given the root node of a binary search tree (BST) and a value val to insert into the tree. Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST. Note that the compiler output shows true if the node is added correctly, else false.

- Inserting a value into a Binary Search Tree (BST) requires preserving the BST property, where left children are smaller and right children are larger than the parent node. This is done recursively or iteratively by traversing the tree and finding the correct position for the new value.

- ```cpp
    Node* insertNode(Node* root, int key) {
        if (root == nullptr)
            return new Node(key);

        if (key < root->val)
            root->left = insertNode(root->left, key);
        else
            root->right = insertNode(root->right, key);

        return root;
    }
    ```

### ***Delete a Given Node***

- Given the root node of a binary search tree (BST) and a value key. Return the root node of the BST after the deletion of the node with the given key value. Note: As there can be many correct answers, the compiler returns true if the answer is correct, otherwise false.

- Deleting a value from a Binary Search Tree (BST) involves preserving the BST property while handling three possible structural cases: deleting a leaf node, a node with one child, or a node with two children.

- ```cpp
    Node* findMin(Node* root) {
        while (root->left)
            root = root->left;
        return root;
    }

    Node* deleteNode(Node* root, int key) {
        // If tree is empty, return null
        if (root == nullptr)
            return nullptr;

        // If key is less, go to left subtree
        if (key < root->val)
            root->left = deleteNode(root->left, key);
        // If key is greater, go to right subtree
        else if (key > root->val)
            root->right = deleteNode(root->right, key);
        else {
            // Node with only one child or no child
            if (root->left == nullptr)
                return root->right;
            else if (root->right == nullptr)
                return root->left;

            // Node with two children: get inorder successor
            Node* temp = findMin(root->right);
            root->val = temp->val;
            root->right = deleteNode(root->right, temp->val);
        }

        // Return the updated root
        return root;
    }
    ```



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


### ***Check if Tree is BST or not***

- Problem Statement: You are given the root of a binary tree. The task is to determine if the given binary tree qualifies as a binary search tree. Conditions for a Binary Tree to qualify as Binary Search Tree (BST): The left child’s key is less than the key of its parent. The right child’s key is more than the key of its parent. The left and right subtrees also count as BST individually.

- To verify if a binary tree is a Binary Search Tree (BST), we must ensure every node follows the BST rule — values in the left subtree should be less than the current node, and values in the right subtree should be greater. But it's not enough to compare a node with just its immediate children — we must ensure that all nodes in its left and right subtrees fall within valid value boundaries based on the ancestors.

- Imagine passing down a range of valid values as we go deeper into the tree. For the left child, the maximum allowable value becomes the current node's value. For the right child, the minimum becomes the current node’s value. If at any point a node violates its range, it’s not a BST.

- ```cpp
    bool bSearch(int elem, int arr[], int n) {
        int start = 0;
        int end = n - 1;

        // Perform binary search
        while (start <= end) {
            int mid = (start + end) / 2;

            // If element is found, return true
            if (arr[mid] == elem)
                return true;

            // If the element is greater than mid, search in the right half
            else if (arr[mid] < elem)
                start = mid + 1;

            // If the element is smaller than mid, search in the left half
            else
                end = mid - 1;
        }
        return false; // If the element is not found
    }

    // Function to check if arr1[] is a subset of arr2[]
    bool isSubset(int arr1[], int m, int arr2[], int n) {

        // Sort arr2[] for efficient binary search
        sort(arr2, arr2 + n);

        // If arr1[] has more elements than arr2[], it cannot be a subset
        if (m > n) return false;

        // For each element in arr1[], check if it exists in arr2[]
        for (int i = 0; i < m; i++) {
            bool present = bSearch(arr1[i], arr2, n); // Check if arr1[i] is present in arr2[]

            // If any element from arr1[] is not present in arr2[], return false
            if (present == false) return false;
        }

        // If all elements of arr1[] are found in arr2[], return true
        return true;
    }
    ```

### ***LCA in BST***

- Given the root node of a binary search tree (BST) and two node values p and q, return the lowest common ancestors(LCA) of the two nodes in BST.

- In a Binary Search Tree (BST), all nodes in the left subtree of a node are smaller than it, and all nodes in the right subtree are larger. Therefore, if we are looking for the lowest common ancestor of two nodes, we can take advantage of the BST structure.

- If both values are smaller than the current node, it means both are in the left subtree, so we move left. If both values are greater, we move right. The moment one value is on the left and the other on the right (or one of them equals the current node), we’ve found the split point i.e. the current node is the lowest common ancestor.

- ```cpp
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        while (root != nullptr) {
            // If both nodes are smaller, go left
            if (p->val < root->val && q->val < root->val)
                root = root->left;
            // If both nodes are greater, go right
            else if (p->val > root->val && q->val > root->val)
                root = root->right;
            // Else, this is the split point and thus the LCA
            else
                return root;
        }
        return nullptr;
    }
    ```

### ***Construct a BST from a preorder traversal***

- Given a preorder traversal of a binary search tree. Return the root of the Binary Search Tree constructed from the given preorder array.

- Brute: Time Complexity: O(N2) as We traverse the entire array of N elements and for each element insertion, we make comparisons and insert it into the tree starting from the root resulting in a time complexity of O(N x N) = O(N2). Space Complexity: O(N), as we create a binary search tree proportional to the number of nodes in the preorder array.

- Binary Search Tree would be to create the BST iteratively according to the preorder sequence. The first element is considered the root of the BST, rest of the elements are then inserted based on their relationship with the existing nodes. If an element is smaller than the current node, it is placed as the left child, if greater then placed as the right child. This process continues until the entire array is traversed.

- ```cpp
    TreeNode* bstFromPreorder(vector<int>& A) {
        // Return null if input array is empty
        if (A.empty()) return NULL;

        // First value is always the root
        TreeNode* root = new TreeNode(A[0]);

        // Iterate over remaining elements
        for (int i = 1; i < A.size(); ++i) {
            TreeNode* curr = root;

            // Traverse tree to find correct position for A[i]
            while (true) {
                // Go left if smaller
                if (A[i] < curr->val) {
                    if (!curr->left) {
                        curr->left = new TreeNode(A[i]);
                        break;
                    } else {
                        curr = curr->left;
                    }
                } else {
                    // Go right if greater or equal
                    if (!curr->right) {
                        curr->right = new TreeNode(A[i]);
                        break;
                    } else {
                        curr = curr->right;
                    }
                }
            }
        }
        return root;
    }
    ```

- Optimal: Time Complexity: O(N), Each node is processed once while constructing the BST. Space Complexity: O(H), Stack space for recursion, where H is the height of the tree.

- Intuition: In a Binary Search Tree (BST), each node obeys a range rule: all nodes in its left subtree must be smaller, and those in the right must be larger. While building a BST from preorder traversal, we use bounds to enforce this rule. The root starts with an infinite range, and as we traverse, we adjust upper bounds for left children and maintain global constraints for right children. This ensures that each node fits into its correct position while preserving BST properties.

- ```cpp
    TreeNode* bstFromPreorder(vector<int>& A) {
        int i = 0; // Index to track current element
        return build(A, i, INT_MAX); // Start building with upper bound as INT_MAX
    }

    // Recursive helper with current index and value bound
    TreeNode* build(vector<int>& A, int& i, int bound) {
        // If out of elements or current value exceeds bound, stop recursion
        if (i == A.size() || A[i] > bound) return nullptr;

        // Create node with current value and increment index
        TreeNode* root = new TreeNode(A[i++]);

        // Recursively build left subtree with root->val as new upper bound
        root->left = build(A, i, root->val);

        // Recursively build right subtree with original bound
        root->right = build(A, i, bound);

        return root;
    }
    ```

### ***Inorder Successor/Predecessor in BST***

- Given a Binary Search Tree and a ‘key’ value which represents the data data of a node in this tree. Return the inorder predecessor and successor of the given node in the BST.

- The predecessor of a node in BST is that node that will be visited just before the given node in the inorder traversal of the tree. Return nullptr if the given node is the one that is visited first in the inorder traversal.

- The successor of a node in BST is that node that will be visited immediately after the given node in the inorder traversal of the tree. Return nullptr if the given node is visited last in the inorder traversal.

- Brute: Time Complexity: O(N + logN) where N is the number of nodes of the binary search tree. O(N) to traverse all nodes and store them in an inorder array and O(log N) for the binary search. Space Complexity: O(N) as an array of size N is used to store the inorder traversal of the binary search tree.

- ```cpp
    // find inorder successor by building inorder list and binary searching
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        // container for inorder traversal
        vector<int> inorder;
        // fill inorder list
        inorderTraversal(root, inorder);
        // locate index of p->val (or insertion index)
        int idx = binarySearch(inorder, p->val);
        // handle edge cases: last element or invalid
        if (idx == (int)inorder.size() - 1 || idx == -1) {
            return nullptr;
        }
        // return a new node with successor value (mirroring given logic)
        return new TreeNode(inorder[idx + 1]);
    }

    // inorder traversal helper
    void inorderTraversal(TreeNode* root, vector<int>& inorder) {
        // base case
        if (root == nullptr) return;
        // traverse left
        inorderTraversal(root->left, inorder);
        // visit node
        inorder.push_back(root->val);
        // traverse right
        inorderTraversal(root->right, inorder);
    }

    // binary search helper
    int binarySearch(vector<int>& arr, int target) {
        // search bounds
        int left = 0, right = (int)arr.size() - 1;
        // standard binary search
        while (left <= right) {
            // midpoint
            int mid = left + (right - left) / 2;
            // found case
            if (arr[mid] == target) return mid;
            // move right
            else if (arr[mid] < target) left = mid + 1;
            // move left
            else right = mid - 1;
        }
        // not found: return insertion index or -1 if at end
        return left == (int)arr.size() ? -1 : left;
    }
    ```

- Better: Time Complexity: O(N) where N is the number of nodes in the binary search tree. This complexity arises from the fact that we have to traverse all nodes in an inorder fashion to get to the inorder successor. Space Complexity: O(1) as no additional data structure or memory allocation is done during the traversal and algorithm. Only a value comparison at each node.

- ```cpp
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        // initialize successor pointer
        TreeNode* successor = nullptr;
        // traverse until null
        while (root != nullptr) {
            // move left while updating successor when root > p
            if (root->val > p->val) {
                successor = root;
                root = root->left;
            }
            // otherwise move right
            else {
                root = root->right;
            }
        }
        // return final successor (or null)
        return successor;
    }
    ```

- Optimal: Time Complexity: O(H) where H is the height of the binary search tree as we are traversing along the height of the tree. Space Complexity: O(1) as no additional data structure or memory allocation is done during the traversal and algorithm.

- ```cpp
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        // initialize successor
        TreeNode* successor = nullptr;
        // traverse until root becomes null
        while (root != nullptr) {
            // when p is greater or equal, move right
            if (p->val >= root->val) {
                root = root->right;
            }
            // otherwise update successor and move left
            else {
                successor = root;
                root = root->left;
            }
        }
        // return successor (may be null)
        return successor;
    }
    ```

### ***Merge 2 BST's***

- Given two BSTs, return elements of merged BSTs in sorted form.

- Brute: Time Complexity: O((n+m)*log(n+m)), we traverse both the BSTs and sort all the elements. Space Complexity: O(m+n), additional space required for storing elements of the two BSTs.

- A naive approach to solve this problem is to simply traverse both of the trees using any technique and store all the elements in a list. Once we have all the elements, we can simply sort our list to get the elements of merged BST in sorted form.

- ```cpp
    void traverse(Node* root, vector<int>& elements) {
        // If root is NULL, stop recursion
        if (!root) return;
        // Traverse left subtree
        traverse(root->left, elements);
        // Store current node's data
        elements.push_back(root->data);
        // Traverse right subtree
        traverse(root->right, elements);
    }

    // Function to merge elements of two BSTs
    vector<int> mergeBSTs(Node* root1, Node* root2) {
        // Vector to store all elements
        vector<int> elements;
        // Traverse first BST and collect elements
        traverse(root1, elements);
        // Traverse second BST and collect elements
        traverse(root2, elements);
        // Sort the collected elements
        sort(elements.begin(), elements.end());
        // Return the sorted elements
        return elements;
    }
    ```

- Optimal: Time Complexity: O(n+m), we traverse both the BSTs and merge two sorted lists. Space Complexity: O(m+n), additional space required for storing elements of the two BSTs.

- We know that inorder traversal of any BST returns the elements in sorted order. Thus, we will use this property to optimize our solution. We can simply traverse both the BSTs one by one and store their elements in separate lists. Since, both these lists are individually sorted, we can just merge these sorted lists to get our elements of merged BST in sorted form.

- ```cpp
    void inorderTraversal(Node* root, vector<int>& arr) {
        // Base case
        if (!root) return;
        // Traverse left subtree
        inorderTraversal(root->left, arr);
        // Store current node data
        arr.push_back(root->data);
        // Traverse right subtree
        inorderTraversal(root->right, arr);
    }

    // Function to merge two sorted arrays
    vector<int> mergeArrays(vector<int>& arr1, vector<int>& arr2) {
        // Initialize result array
        vector<int> merged;
        // Initialize pointers
        int i = 0, j = 0;
        // Merge until one array ends
        while (i < arr1.size() && j < arr2.size()) {
            if (arr1[i] < arr2[j]) merged.push_back(arr1[i++]);
            else merged.push_back(arr2[j++]);
        }
        // Add remaining elements
        while (i < arr1.size()) merged.push_back(arr1[i++]);
        while (j < arr2.size()) merged.push_back(arr2[j++]);
        return merged;
    }

    // Function to merge two BSTs in sorted order
    vector<int> mergeBSTs(Node* root1, Node* root2) {
        // Arrays to store inorder traversals
        vector<int> arr1, arr2;
        // Perform inorder traversal on first BST
        inorderTraversal(root1, arr1);
        // Perform inorder traversal on second BST
        inorderTraversal(root2, arr2);
        // Merge and return the result
        return mergeArrays(arr1, arr2);
    }
    ```


### ***Two Sum In BST | Check if there exists a pair with Sum K***

- Brute: Time Complexity: O(N+N) where N is the number of nodes in the Binary Search Tree. To create the array that will store the inorder sequence, we have to traverse the entire BST, hence O(N) and to apply the two pointer approach and get the pair equal to sum again requires O(N) hence O(N+N) ~ O(2N) ~ O(N). Space Complexity : O(N)where N is the number of nodes in the BST, as we have to store all the nodes in an additional data structure array. The two pointer approach does not use any additional space hence the space complexity is O(N).

- By getting the inorder traversal of a Binary Search Tree, we get a sorted sequence. On this sorted sequence we can apply the Two Sum problem to return the pair with sum equal to K. This can be done by initialising two pointers at the sequences starts and end, navigating based on their sum compared to the target. This approach leverages the sorted nature of the inorder traversal of a Binary Search Tree.

- ```cpp
    bool findTarget(TreeNode* root, int k) {
        // Vector stores inorder traversal of BST
        vector<int> inorder;

        // Call helper to fill vector
        inorderTraversal(root, inorder);

        // Initialize two pointers for searching in sorted inorder array
        int left = 0;
        int right = inorder.size() - 1;

        // Loop until pointers meet
        while (left < right) {
            // Calculate sum of current pair
            int sum = inorder[left] + inorder[right];

            // If sum equals k, we found a pair
            if (sum == k) {
                return true;
            }
            // If sum smaller than k, move left pointer forward
            else if (sum < k) {
                left++;
            }
            // If sum larger than k, move right pointer backward
            else {
                right--;
            }
        }

        // If no such pair found, return false
        return false;
    }

    // Helper function to perform inorder traversal
    void inorderTraversal(TreeNode* root, vector<int>& inorder) {
        // If root is null, stop recursion
        if (!root) return;

        // Traverse left subtree
        inorderTraversal(root->left, inorder);
        // Add current node value to vector
        inorder.push_back(root->val);
        // Traverse right subtree
        inorderTraversal(root->right, inorder);
    }
    ```

- Optimal: Time Complexity: O(N) where N is the number of nodes in the BST as we have to traverse all the nodes using the i and j pointers to find the pair with sum ‘k’. Space Complexity : O(H) where H is the height of the Binary Search Tree as the BSTIterator class uses a stack to store the nodes. At maximum the size of such a stack will be equal to the height of the Binary Tree.

- The previous approach uses O(N) space complexity which can be eliminated by leveraging the properties of a Binary Search Tree instead. As a prerequisite for this problem, make sure you are thorough with the concepts of Binary Search Tree Iterator. This BSTIterator class allows one to access the next and previous elements (in order predecessor and successor) in a BST.

- Using the BSTIterator class implementation, initialise pointers 'i' and 'j' to the first and last elements of the BST's inorder traversal, respectively. These pointers are navigated through the BST using the next() and before() functions of the BSTIterator. The 'i' pointer progresses towards larger values with next(), while 'j' moves towards smaller values with before(). This approach leverages on the BST properties to efficiently navigate through the elements and identify the pair satisfying the given sum without using any additional data structure to store the inorder traversal.


- ```cpp
    class BSTIterator {
    private:
        // A stack is used to keep track of nodes while traversing
        stack<TreeNode*> myStack;
        // This flag tells whether we are moving forward (inorder) or backward (reverse inorder)
        bool reverse;

    public:
        // Constructor initializes the iterator with the root node and traversal direction
        BSTIterator(TreeNode* root, bool isReverse) : reverse(isReverse) {
            // Push all nodes on one side (left or right) into the stack
            pushAll(root);
        }

        // This function checks if there are more nodes left to visit
        bool hasNext() {
            // If the stack is not empty, there are still nodes left
            return !myStack.empty();
        }

        // This function returns the next node’s value in the chosen order
        int next() {
            // Get the node on top of the stack
            TreeNode* tmpNode = myStack.top();
            // Remove this node from the stack
            myStack.pop();

            // If we are not in reverse mode, we need to go right after visiting a node
            if (!reverse) {
                pushAll(tmpNode->right);
            }
            // If we are in reverse mode, we need to go left after visiting a node
            else {
                pushAll(tmpNode->left);
            }

            // Return the value of the node that was just visited
            return tmpNode->val;
        }

    private:
        // This helper function pushes all nodes from the current node down to the left/right edge
        void pushAll(TreeNode* node) {
            // Keep going until we reach a null node
            while (node != nullptr) {
                // Push the node onto the stack
                myStack.push(node);
                // If reverse is true, move to the right child
                if (reverse) {
                    node = node->right;
                }
                // Otherwise, move to the left child
                else {
                    node = node->left;
                }
            }
        }
    };

    // This class contains the solution logic to check if a target sum exists in BST
    class Solution {
    public:
        // This function checks if two nodes in BST sum to k
        bool findTarget(TreeNode* root, int k) {
            // If tree is empty, return false immediately
            if (!root) return false;

            // Create two iterators: one for smallest-to-largest order, another for largest-to-smallest
            BSTIterator l(root, false);
            BSTIterator r(root, true);

            // Get first values from both ends
            int i = l.next();
            int j = r.next();

            // Loop until the two pointers meet
            while (i < j) {
                // If the two numbers add up to k, we found a pair
                if (i + j == k) return true;
                // If sum is too small, move left iterator forward
                else if (i + j < k) i = l.next();
                // If sum is too large, move right iterator backward
                else j = r.next();
            }

            // If no pair found, return false
            return false;
        }
    };
    ```


### ***Correct BST with two nodes swapped***

- Given the root of a Binary Search Tree (BST), where the values of exactly two nodes of the BST have been swapped. Recover the tree without changing its structure.

- Brute: Time Complexity: O(2*N + N*logN), We traverse the entire tree twice. In addition, we also sort the array to get correct inorder traversal
Space Complexity: O(N), for storing the traversal in a data structure and extra recursion space for getting the inorder traversal.

- We know that, for a valid BST, the inorder traversal of a BST returns a sorted array. Thus we can perform an inorder traversal of the BST to gather the node values in an array and sort this array to get the corrected inorder traversal of the BST, this traversal has the correct node positions of the swapped nodes. Traverse the BST in inorder again while comparing each node’s values with the corresponding index from the sorted inorder array. At positions where the mismatch occurs, update the tree with the correct values from the sorted array, effectively fixing the swaps without altering the tree’s structure.

- ```cpp
    #include <bits/stdcc++.h>
    using namespace std;

    // Definition for a binary tree node.
    struct TreeNode {
        int val;
        TreeNode *left;
        TreeNode *right;
        TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    };

    class Solution {
        private:
            // Function to store the inorder traversal
            void inorder(TreeNode* root, vector<int>& values) {
                if (root == nullptr) return;
                inorder(root->left, values);
                values.push_back(root->val);
                inorder(root->right, values);
        }
        public:
            void recoverTree(TreeNode* root) {
                vector<int> sortedVals;
                // Perform an inorder traversal
                // to obtain values in sorted order
                inorder(root, sortedVals);
            
                // Sort the obtained values to
                // get the corrected inorder traversal
                sort(sortedVals.begin(), sortedVals.end());
                
                // Initialize pointers for tree
                // traversal and sortedVals array
                TreeNode* current = root;
                int index = 0;
            
                // Morris Traversal to traverse the
                // tree without recursion or stack
                while (current != nullptr) {
                    // If there is no left subtree
                    if (current->left == nullptr) {
                        // Compare the current node's
                        // value with the sortedVals array
                        if (current->val != sortedVals[index]) {
                            // Update the current node's
                            // value if there's a mismatch
                            current->val = sortedVals[index];
                        }
                        ++index;
                        // Move to the right subtree
                        current = current->right;
                    } else {
                        // Find the predecessor of the current node
                        TreeNode* predecessor = current->left;
                        while (predecessor->right != nullptr && predecessor->right != current) {
                            predecessor = predecessor->right;
                        }
            
                        // If the right pointer of
                        // predecessor is not set
                        if (predecessor->right == nullptr) {
                            // Set the threaded pointer
                            // to the current node
                            predecessor->right = current;
                            // Move to the left subtree
                            current = current->left;
                        } else {
                            // Revert threaded
                            // pointer back to nullptr
                            predecessor->right = nullptr;
                            
                            // Compare the current node's
                            // value with the sortedVals array
                            
                            if (current->val != sortedVals[index]) {
                                // Update the current node's
                                // value if there's a mismatch
                                current->val = sortedVals[index];
                            }
                            ++index;
                            // Move to the right subtree
                            current = current->right;
                        }
                    }
                }
            }

    };

    // Utility function to
    // insert nodes into the BST
    TreeNode* insert(TreeNode* root, int val) {
        if (root == NULL) {
            return new TreeNode(val);
        }

        if (val < root->val) {
            root->left = insert(root->left, val);
        } else {
            root->right = insert(root->right, val);
        }

        return root;
    }

    // Utility function to perform
    // inorder traversal of the BST
    void inorderTraversal(TreeNode* root) {
        if (root == NULL) {
            return;
        }

        inorderTraversal(root->left);
        cout << root->val << " ";
        inorderTraversal(root->right);
    }

    // Function to swap two tree
    // nodes and their subtrees
    void swapNodes(TreeNode* a, TreeNode* b) {
        // Swap values of the nodes
        int temp = a->val;
        a->val = b->val;
        b->val = temp;

        // Swap left subtrees of the nodes
        TreeNode* tempLeft = a->left;
        a->left = b->left;
        b->left = tempLeft;

        // Swap right subtrees of the nodes
        TreeNode* tempRight = a->right;
        a->right = b->right;
        b->right = tempRight;
    }

    int main() {
        Solution solution;

        // Create the BST
        TreeNode* root = nullptr;
        root = insert(root, 3);
        insert(root, 1);
        insert(root, 4);
        insert(root, 2);

        cout << "Original BST: ";
        inorderTraversal(root);
        cout << endl;


        // Intentionally swapping two nodes (4 and 2) 
        TreeNode* node4 = root->right;
        TreeNode* node2 = root->left->right;
        swapNodes(node4, node2);
        
        cout << "BST with swapped nodes: ";
        inorderTraversal(root);
        cout << endl;
        
        // Recover the BST
        solution.recoverTree(root);
        
        cout << "Recovered BST: ";
        inorderTraversal(root);
        cout << endl;
        
        return 0;
    }
    ```

- Optimal: Time Complexity: O(N), where N is the number of nodes in the Binary Search Tree as the algorithm involves performing an inorder traversal to identify the swapped nodes. Space Complexity: O(N), extra recursion space for getting the inorder traversal.

- The inorder traversal of a Binary Search Tree which results in a sorted sequence. However due to the swapped elements, the sorted order is disrupted by these misplaced nodes. While traversing the tree, we keep a track of the previous and next node to each visited node. As we identify nodes that violate the sorted order we store them.

- By tracking these violations and handling the cases where the swapped nodes could be adjacent or non-adjacent, the algorithm can effectively pinpoint the two nodes that are out of place.

    - Four pointers are utilized: `first`, `prev`, `middle` and `last`. `first ` and `middle` identify the misplaced nodes in the tree. `prev` keeps track of the previous node during the inorder traversal. `last` marks the second node of the misplaced pair in case the nodes are adjacent.

    - Traversing using the inorder method and compare each node’s value with the previous node’s value `prev` to identify any violations that the current node value is less than the previous node value. If a violation is found, mark the nodes accordingly

    - The first violation indicates the first and middle nodes. The first node is the first element encountered that is not greater than its previous node. The middle node is temporarily stored in case the swapped nodes are adjacent and there's no second violation.

    - If a second violation (second element not greater than its previous) is found, it signifies that the swapped nodes are not adjacent, and the last node is identified. If there's no second violation, implying that the swapped nodes are adjacent, the middle node identified earlier is retained.

    - Once the misplaced nodes are identified, it performs necessary swaps based on whether the nodes are adjacent or not. If both first and last are identified, it swaps the values of the first and last nodes. If only first and middle are identified, it swaps their values.

- ```cpp
    TreeNode *first = NULL, *middle = NULL, *last = NULL, *prev = NULL;

    // Function to perform inorder traversal and find swapped nodes
    void inorder(TreeNode* root) {
        // Return if current node is NULL
        if (!root) return;

        // Traverse the left subtree
        inorder(root->left);

        // Check for violation of BST property
        if (prev && root->val < prev->val) {
            // First violation
            if (!first) {
                first = prev;
                middle = root;
            } 
            // Second violation
            else {
                last = root;
            }
        }

        // Update previous node
        prev = root;

        // Traverse the right subtree
        inorder(root->right);
    }

    // Function to recover the binary search tree
    void recoverTree(TreeNode* root) {
        // Perform inorder traversal to detect the two swapped nodes
        inorder(root);

        // If nodes are not adjacent
        if (first && last) swap(first->val, last->val);
        // If nodes are adjacent
        else if (first && middle) swap(first->val, middle->val);
    }

    // Helper function to print inorder traversal
    void printInorder(TreeNode* root) {
        if (!root) return;
        printInorder(root->left);
        cout << root->val << " ";
        printInorder(root->right);
    }
    ```

### ***Largest BST in Binary Tree***

- Given a root of Binary Tree, where the nodes have integer values. Return the size of the largest subtree of the binary tree which is also a BST. A binary search tree (BST) is a binary tree data structure which has the following properties. The left subtree of a node contains only nodes with data less than the node’s data. The right subtree of a node contains only nodes with data greater than the node’s data. Both the left and right subtrees must also be binary search trees.

- Brute: Time Complexity: O(N), where N is the number of nodes in the binary tree. Each node is visited once. Space Complexity: O(H), where H is the height of the binary tree. This is due to the recursive stack space used during the traversal.

- ```cpp
    tuple<int, bool, int, int> isBSTAndSize(TreeNode* node, int minValue, int maxValue) {
        // Base case: if node is nullptr, it is a valid BST of size 0.
        if (!node) {
            return make_tuple(0, true, INT_MAX, INT_MIN);
        }

        // Recursively check the left and right subtrees.
        auto [leftSize, isLeftBST, leftMin, leftMax] = isBSTAndSize(node->left, minValue, node->data);
        auto [rightSize, isRightBST, rightMin, rightMax] = isBSTAndSize(node->right, node->data, maxValue);

        // Check if the current node is a valid BST node.
        if (isLeftBST && isRightBST && leftMax < node->data && node->data < rightMin) {
            // Current subtree is a valid BST, calculate its size.
            int size = leftSize + rightSize + 1;
            // Return size, isBST, min value, and max value for the current subtree.
            return make_tuple(size, true, min(node->data, leftMin), max(node->data, rightMax));
        } else {
            // Current subtree is not a valid BST, return the size of the largest valid BST found so far.
            return make_tuple(max(leftSize, rightSize), false, INT_MIN, INT_MAX);
        }
    }

    int largestBST(TreeNode* root) {
        // Initialize the function to call
        return get<0>(isBSTAndSize(root, INT_MIN, INT_MAX));
    }
    ```

- Optimal: Time Complexity: O(N), where N is the number of nodes in the binary tree. Each node is visited once. Space Complexity: O(N), where N is number of nodes in the Binary Tree as for each node we store additional information using a struct class whose new object is initialised.

- ```cpp
    struct NodeValue {
        int minNode, maxNode, maxSize;
        NodeValue(int minNode, int maxNode, int maxSize) : minNode(minNode), maxNode(maxNode), maxSize(maxSize) {}
    };

    // Helper function to recursively find the largest BST subtree.
    NodeValue largestBSTSubtreeHelper(TreeNode* node) {
        // Base case: if the node is null, return a default NodeValue.
        if (!node) {
            return NodeValue(INT_MAX, INT_MIN, 0);
        }

        // Recursively get values from the left and right subtrees.
        NodeValue left = largestBSTSubtreeHelper(node->left);
        NodeValue right = largestBSTSubtreeHelper(node->right);

        // Check if the current node is a valid BST node.
        if (left.maxNode < node->data && node->data < right.minNode) {
            // Current subtree is a valid BST.
            return NodeValue(
                min(node->data, left.minNode),
                max(node->data, right.maxNode),
                left.maxSize + right.maxSize + 1
            );
        }

        // Current subtree is not a valid BST.
        return NodeValue(INT_MIN, INT_MAX, max(left.maxSize, right.maxSize));
    }

    int largestBST(TreeNode* root) {
        // Initialize the recursive process and return the size of the largest BST subtree.
        return largestBSTSubtreeHelper(root).maxSize;
    }
    ```

