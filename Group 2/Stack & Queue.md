# Stack & Queue

- Implement Stack using Arrays
- Implement Queue using Arrays
- Implement Stack using Queue
- Implement Queue using Stack
- Implement Stack using LinkedList
- Implement Queue using LinkedList
- Balanced Paranthesis
- Implement Min Stack
___
- Infix to Postfix
- Infix to Prefix
- Prefix to Infix
- Prefix to Postfix
- Postfix to Prefix
- Postfix to Infix
___
- Next Greater Element
- Next Greater Element-2
- Next Smaller Element
- Number of NGE to right
___
- Trapping Rainwater
- Sum of subarray minimums
- Asteroid Collision
- Sum of subarray ranges
- Remove K digits
- Largest Rectangle in a histogram
- Maximum Rectangle
___
- Sliding Window Maximum
- Stock Span Problem
- Celebrity Problem
- LRU Cache
- LFU Cache
___
## ***Part 1 - Learning***

- These problems I think I can do. I will most probably be stuck in coding part because I have not coded in a while without AI. So lets see the last question because I do not know what it is. If I get time to code we will code all other questions later. At the end code all question for confidence in coding.

- So the last question is just simple that we have to Implement stack but I have to write a function getMin() which retrieves the minimum element in the stack. It has brute force as well as optimal approach. I can think of brute force solution by using O(N) extra space by maintaining another stack which in filled with the old stack.top and we track min. But no here the brute force implementaion is to store min element along with value as pair in stack while inserting. The optimal approach has some formula 2*min-x and 2 *value-min so read this. This is new.

### ***Implement Stack using Arrays***

- Implement a Last-In-First-Out (LIFO) stack using an array. The implemented stack should support the following operations: push, pop, peek, and isEmpty. Implement the ArrayStack class: void push(int x): Pushes element x onto the stack. int pop(): Removes and returns the top element of the stack. int top(): Returns the top element of the stack without removing it. boolean isEmpty(): Returns true if the stack is empty, false otherwise.

- ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    class ArrayStack {
    private:
        // Array to hold elements
        int* stackArray;
        // Maximum capacity
        int capacity; 
        // Index of top element  
        int topIndex;   

    public:
        // Constructor
        ArrayStack(int size = 1000) {
            capacity = size;
            stackArray = new int[capacity];
            // Initialize stack as empty
            topIndex = -1; 
        }

        // Destructor
        ~ArrayStack() {
            delete[] stackArray;
        }

        // Pushes element x 
        void push(int x) {
            if (topIndex >= capacity - 1) {
                cout << "Stack overflow" << endl;
                return;
            }
            stackArray[++topIndex] = x;
        }

        // Removes and returns top element
        int pop() {
            if (isEmpty()) {
                cout << "Stack is empty" << endl;
                // Return invalid value
                return -1; 
            }
            return stackArray[topIndex--];
        }

        // Returns top element
        int top() {
            if (isEmpty()) {
                cout << "Stack is empty" << endl;
                return -1; 
            }
            return stackArray[topIndex];
        }

    /* Returns true if the 
    stack is empty, false otherwise*/
        bool isEmpty() {
            return topIndex == -1;
        }
    };

    // Main Function
    int main() {
        ArrayStack stack;
        vector<string> commands = {"ArrayStack", "push", "push", "top", "pop", "isEmpty"};
        vector<vector<int>> inputs = {{}, {5}, {10}, {}, {}, {}};

        for (size_t i = 0; i < commands.size(); ++i) {
            if (commands[i] == "push") {
                stack.push(inputs[i][0]);
                cout << "null ";
            } else if (commands[i] == "pop") {
                cout << stack.pop() << " ";
            } else if (commands[i] == "top") {
                cout << stack.top() << " ";
            } else if (commands[i] == "isEmpty") {
                cout << (stack.isEmpty() ? "true" : "false") << " ";
            } else if (commands[i] == "ArrayStack") {
                cout << "null ";
            }
        }

        return 0;
    }
    ```

### ***Implement Queue using Arrays***

- Implement a First-In-First-Out (FIFO) queue using an array. The implemented queue should support the following operations: push, dequeue, pop, and isEmpty. Implement the ArrayQueue class:
    - void push(int x): Adds element x to the end of the queue.
    - int pop(): Removes and returns the front element of the queue.
    - int peek(): Returns the front element of the queue without removing it.
    - boolean isEmpty(): Returns true if the queue is empty, false otherwise.

- ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    // Class implementing Queue using Arrays
    class ArrayQueue {
        // Array to store queue elements
        int* arr;
        // Indices for start and end of the queue
        int start, end;
        // Current size and maximum size of the queue
        int currSize, maxSize;

    public:
        // Constructor
        ArrayQueue() {
            arr = new int[10];
            start = -1;
            end = -1;
            currSize = 0;
            maxSize = 10;
        }

        // Method to push an element into the queue
        void push(int x) {
            // Check if the queue is full
            if (currSize == maxSize) {
                cout << "Queue is full\nExiting..." << endl;
                exit(1);
            }
            
            // If the queue is empty, initialize start and end
            if (end == -1) {
                start = 0;
                end = 0;
            } 
            else {
                // Circular increment of end
                end = (end + 1) % maxSize;
            }
                
            arr[end] = x;
            currSize++;
        }

        // Method to pop an element from the queue
        int pop() {
            // Check if the queue is empty
            if (start == -1) {
                cout << "Queue Empty\nExiting..." << endl;
                exit(1);
            }
            int popped = arr[start];
            
            // If the queue has only one element, reset start and end
            if (currSize == 1) {
                start = -1;
                end = -1;
            }
            else {
                // Circular increment of start
                start = (start + 1) % maxSize;
            }
            
            currSize--;
            return popped;
        }

        // Method to get the front element of the queue
        int peek() {
            // Check if the queue is empty
            if (start == -1) {
                cout << "Queue is Empty" << endl;
                exit(1);
            }
            return arr[start];
        }

        // Method to determine whether the queue is empty
        bool isEmpty() {
            return (currSize == 0);
        }
    };

    int main() {
        ArrayQueue queue;
        vector<string> commands = {"ArrayQueue", "push", "push", 
                                "peek", "pop", "isEmpty"};
        vector<vector<int>> inputs = {{}, {5}, {10}, {}, {}, {}};

        for (int i = 0; i < commands.size(); ++i) {
            if (commands[i] == "push") {
                queue.push(inputs[i][0]);
                cout << "null ";
            } else if (commands[i] == "pop") {
                cout << queue.pop() << " ";
            } else if (commands[i] == "peek") {
                cout << queue.peek() << " ";
            } else if (commands[i] == "isEmpty") {
                cout << (queue.isEmpty() ? "true" : "false") << " ";
            } else if (commands[i] == "ArrayQueue") {
                cout << "null ";
            }
        }

        return 0;
    }
    ```

### ***Implement Stack using Queue***

- Implement a Last-In-First-Out (LIFO) stack using a single queue. The implemented stack should support the following operations: push, pop, top, and isEmpty.
- ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    // Stack implementation using Queue
    class QueueStack {
        // Queue
        queue<int> q;

    public:
        // Method to push element in the stack
        void push(int x) {
            // Get size 
            int s = q.size(); 
            // Add element
            q.push(x); 

            // Move elements before new element to back
            for (int i = 0; i < s; i++) {
                q.push(q.front()); 
                q.pop(); 
            }
        }

        // Method to pop element from stack
        int pop() {
            // Get front element 
            int n = q.front(); 
            // Remove front element
            q.pop(); 
            // Return removed element
            return n; 
        }

        // Method to return the top of stack
        int top() {
            // Return front element
            return q.front(); 
        }

        // Method to check if the stack is empty
        bool isEmpty() {
            return q.empty(); 
        }
    };

    int main() {
        QueueStack st;
        
        // List of commands
        vector<string> commands = {"QueueStack", "push", "push", 
                                "pop", "top", "isEmpty"};
        // List of inputs
        vector<vector<int>> inputs = {{}, {4}, {8}, {}, {}, {}};

        for (int i = 0; i < commands.size(); ++i) {
            if (commands[i] == "push") {
                st.push(inputs[i][0]);
                cout << "null ";
            } else if (commands[i] == "pop") {
                cout << st.pop() << " ";
            } else if (commands[i] == "top") {
                cout << st.top() << " ";
            } else if (commands[i] == "isEmpty") {
                cout << (st.isEmpty() ? "true" : "false") << " ";
            } else if (commands[i] == "QueueStack") {
                cout << "null ";
            }
        }

        return 0;
    }
    ```

### ***Implement Queue using Stack***

- Implement a First-In-First-Out (FIFO) queue using two stacks. The implemented queue should support the following operations: push, pop, peek, and isEmpty.


- Using two Stacks where push operation is O(N)
- ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    // Queue implementation using stack
    class StackQueue {
    private:
        stack <int> st1, st2;

    public: 
        // Empty Constructor
        StackQueue () {
            
        }
        
        // Method to push elements in the queue
        void push(int x) {
            /* Pop out elements from the first stack 
            and push on top of the second stack */
            while (!st1.empty()) {
                st2.push(st1.top());
                st1.pop();
            }
            
            // Insert the desired element
            st1.push(x);
            
            /* Pop out elements from the second stack 
            and push back on top of the first stack */
            while (!st2.empty()) {
                st1.push(st2.top());
                st2.pop();
            }
        }
        
        // Method to pop element from the queue
        int pop() {
            // Edge case
            if (st1.empty()) {
                cout << "Stack is empty";
                return -1; // Representing empty stack
            }
            
            // Get the top element
            int topElement = st1.top();
            st1.pop(); // Perform the pop operation
            
            return topElement; // Return the popped value
        }
        
        // Method to get the front element from the queue 
        int peek() {
            // Edge case
            if (st1.empty()) {
                cout << "Stack is empty";
                return -1; // Representing empty stack
            }
            
            // Return the top element
            return st1.top();
        }
        
        // Method to find whether the queue is empty
        bool isEmpty() {
            return st1.empty();
        }
    };

    int main() {
        StackQueue q;
        
        // List of commands
        vector<string> commands = {"StackQueue", "push", "push", 
                                "pop", "peek", "isEmpty"};
        // List of inputs
        vector<vector<int>> inputs = {{}, {4}, {8}, {}, {}, {}};

        for (int i = 0; i < commands.size(); ++i) {
            if (commands[i] == "push") {
                q.push(inputs[i][0]);
                cout << "null ";
            } else if (commands[i] == "pop") {
                cout << q.pop() << " ";
            } else if (commands[i] == "peek") {
                cout << q.peek() << " ";
            } else if (commands[i] == "isEmpty") {
                cout << (q.isEmpty() ? "true" : "false") << " ";
            } else if (commands[i] == "StackQueue") {
                cout << "null ";
            }
        }
        
        return 0;
    }
    ```

- Using Two Stacks Where Push Operation is O(1)
- ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    class StackQueue {
    public:
        stack<int> input, output;

        // Initialize your data structure here
        StackQueue() {}

        // Push element x to the back of queue
        void push(int x) {
            input.push(x);
        }

        // Removes the element from in front of queue and returns that element
        int pop() {
            // Shift input to output if output is empty
            if (output.empty()) {
                while (!input.empty()) {
                    output.push(input.top());
                    input.pop();
                }
            }

            // If queue is still empty, return -1 (or throw an error if preferred)
            if (output.empty()) {
                cout << "Queue is empty, cannot pop." << endl;
                return -1;
            }

            int x = output.top();
            output.pop();
            return x;
        }

        // Get the front element
        int peek() {
            // Shift input to output if output is empty
            if (output.empty()) {
                while (!input.empty()) {
                    output.push(input.top());
                    input.pop();
                }
            }

            // If queue is still empty, return -1 (or throw an error if preferred)
            if (output.empty()) {
                cout << "Queue is empty, cannot peek." << endl;
                return -1;
            }

            return output.top();
        }

        // Returns true if the queue is empty, false otherwise
        bool isEmpty() {
            return input.empty() && output.empty();
        }
    };

    int main() {
        StackQueue q;
        q.push(3);
        q.push(4);
        cout << "The element popped is " << q.pop() << endl;
        q.push(5);
        cout << "The front of the queue is " << q.peek() << endl;
        cout << "Is the queue empty? " << (q.isEmpty() ? "Yes" : "No") << endl;
        cout << "The element popped is " << q.pop() << endl;
        cout << "The element popped is " << q.pop() << endl;
        cout << "Is the queue empty? " << (q.isEmpty() ? "Yes" : "No") << endl;

        return 0;
    }
    ```


### ***Implement Stack using LinkedList***

- Implement a Last-In-First-Out (LIFO) stack using a singly linked list. The implemented stack should support the following operations: push, pop, top, and isEmpty.
- ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    // Node structure
    struct Node {
        int val;
        Node *next;
        Node(int d) {
            val = d;
            next = NULL;
        }
    };

    // Structure to represent stack
    class LinkedListStack {
    private:
        Node *head; // Top of Stack
        int size; // Size

    public:
        // Constructor
        LinkedListStack() {
            head = NULL;
            size = 0;
        }

        // Method to push an element onto the stack
        void push(int x) {
            // Creating a node 
            Node *element = new Node(x);
            
            element->next = head; // Updating the pointers
            head = element; // Updating the top
            
            // Increment size by 1
            size++;
        }

        // Method to pop an element from the stack
        int pop() {
            // If the stack is empty
            if (head == NULL) {
                return -1; // Pop operation cannot be performed
            }
            
            int value = head->val; // Get the top value
            Node *temp = head; // Store the top temporarily
            head = head->next; // Update top to next node
            delete temp; // Delete old top node
            size--; // Decrement size
            
            return value; // Return data
        }
        
        // Method to get the top element of the stack
        int top() {
            // If the stack is empty
            if (head == NULL) {
                return -1; // Top element cannot be accessed
            }
            
            return head->val; // Return the top
        }

        // Method to check if the stack is empty
        bool isEmpty() {
            return (size == 0);
        }
    };

    int main() {
        // Creating a stack
        LinkedListStack st;

        // List of commands
        vector<string> commands = {"LinkedListStack", "push", "push", 
                                "pop", "top", "isEmpty"};
        // List of inputs
        vector<vector<int>> inputs = {{}, {3}, {7}, {}, {}, {}};

        for (int i = 0; i < commands.size(); ++i) {
            if (commands[i] == "push") {
                st.push(inputs[i][0]);
                cout << "null ";
            } else if (commands[i] == "pop") {
                cout << st.pop() << " ";
            } else if (commands[i] == "top") {
                cout << st.top() << " ";
            } else if (commands[i] == "isEmpty") {
                cout << (st.isEmpty() ? "true" : "false") << " ";
            } else if (commands[i] == "LinkedListStack") {
                cout << "null ";
            }
        }

        return 0;
    }
    ```


### ***Implement Queue using LinkedList***

- Implement a First-In-First-Out (FIFO) queue using a singly linked list. The implemented queue should support the following operations: push, pop, peek, and isEmpty.
- ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    // Node structure
    struct Node {
        int val;
        Node *next;
        Node(int d) {
            val = d;
            next = NULL;
        }
    };

    // Structure to represent stack
    class LinkedListQueue {
    private:
        Node *start; // Start of the queue
        Node *end; // End of the queue
        int size; // Size of the queue

    public:
        // Constructor
        LinkedListQueue() {
            start = end = NULL;
            size = 0;
        }

        // Method to push an element in the queue
        void push(int x) {
            // Creating a node 
            Node *element = new Node(x);
            
            // If it is the first element being pushed
            if(start == NULL) {
                // Initialise the pointers
                start = end = element;
            }
            else {
                end->next = element; // Updating the pointers
                end = element; // Updating the end
            }
            
            // Increment size by 1
            size++;
        }

        // Method to pop an element from the queue
        int pop() {
            // If the queue is empty
            if (start == NULL) {
                return -1; // Pop operation cannot be performed
            }
            
            int value = start->val; // Get the front value
            Node *temp = start; // Store the front temporarily
            start = start->next; // Update front to next node
            delete temp; // Delete old front node
            size--; // Decrement size
            
            return value; // Return data
        }
        
        // Method to get the front element in the queue
        int peek() {
            // If the stack is empty
            if (start == NULL) {
                return -1; // Top element cannot be accessed
            }
            
            return start->val; // Return the top
        }

        // Method to check if the queue is empty
        bool isEmpty() {
            return (size == 0);
        }
    };

    int main() {
        // Creating a queue
        LinkedListQueue q;

        // List of commands
        vector<string> commands = {"LinkedListQueue", "push", "push", 
                                "peek", "pop", "isEmpty"};
        // List of inputs
        vector<vector<int>> inputs = {{}, {3}, {7}, {}, {}, {}};

        for (int i = 0; i < commands.size(); ++i) {
            if (commands[i] == "push") {
                q.push(inputs[i][0]);
                cout << "null ";
            } else if (commands[i] == "pop") {
                cout << q.pop() << " ";
            } else if (commands[i] == "peek") {
                cout << q.peek() << " ";
            } else if (commands[i] == "isEmpty") {
                cout << (q.isEmpty() ? "true" : "false") << " ";
            } else if (commands[i] == "LinkedListQueue") {
                cout << "null ";
            }
        }

        return 0;
    }
    ```

### ***Balanced Paranthesis***

- Check Balanced Parentheses. Given string str containing just the characters '(', ')', '{', '}', '[' and ']', check if the input string is valid and return true if the string is balanced otherwise return false. Open brackets must be closed by the same type of brackets. Open brackets must be closed in the correct order.
- `( )[ { } ( ) ]` is True. `[ ( )` is False.
- ```cpp
    bool isValid(string s) {
        stack<char> st;  // Stack to store opening brackets

        for (auto it : s) {
            if (it == '(' || it == '{' || it == '[')
                st.push(it);  // Push opening brackets to stack
            else {
                if (st.empty()) return false;  // No matching opening bracket
                char ch = st.top();
                st.pop();

                // Check for matching pair
                if ((it == ')' && ch == '(') ||
                    (it == ']' && ch == '[') ||
                    (it == '}' && ch == '{'))
                    continue;
                else
                    return false;
            }
        }
        return st.empty();  // True if all brackets matched
    }
    ```

### ***Implement Min Stack***

- Design a stack that supports the following operations in constant time: push, pop, top, and retrieving the minimum element.
- Implement the MinStack class:
    - MinStack(): Initializes the stack object.
    - void push(int val): Pushes the element val onto the stack.
    - void pop(): removes the element on the top of the stack.
    - int top(): gets the top element of the stack.
    - int getMin(): retrieves the minimum element in the stack.

- Brute Force
- ```cpp
    class MinStack {
    private:
        stack <pair<int,int>> st;
    public:
        MinStack() {}
        
        void push(int value) {
            if(st.empty()) {
                st.push( {value, value} );
                return;
            }
            int mini = min(getMin(), value);
            st.push({value, mini});
        }
        
        void pop() {
            st.pop(); 
        }
        
        int top() {
            return st.top().first;
        }
        
        int getMin() {
            return st.top().second;
        }
    };
    ```

- Optimal
- ```cpp
    class MinStack {
    private:
        stack <int> st;
        int mini; 
    public:
        MinStack() {}
        
        void push(int value) {
            if(st.empty()) {
                mini = value;
                st.push(value);
                return;
            }
            
            if(value > mini) { st.push(value); }
            else {
                st.push(2 * value - mini);
                mini = value;
            }
        }
        
        void pop() {
            if(st.empty()) return;
            int x = st.top();
            if(x < mini) { mini = 2 * mini - x; }
        }
        
        int top() {
            if(st.empty()) return -1;
            int x = st.top();
            if(mini < x) return x;
            return mini;
        }
        
        int getMin() {
            return mini;
        }
    };
    ```

## ***Part 2 - Prefix, Infix, Postfix conversion***

### ***Infix to Postfix***

- Given an infix expression, Your task is to convert the given infix expression to a postfix expression.
- Example 1: Input: `a + b * (c^d - e) ^ (f + g * h) - i`  Output: `abcd^e-fgh*+^*+i-`
- Example 2: Input: `(p + q) * (m - n)` Output: `pq+mn-*` 

- ```cpp
    int prec(char c) {
        if (c == '^') return 3;
        else if (c == '/' || c == '*') return 2;
        else if (c == '+' || c == '-') return 1;
        else return -1;
    }

    void infixToPostfix(string s) {
        stack<char> st;
        string result;

        for (int i = 0; i < s.length(); i++) {
            char c = s[i];
            if ((c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z') || (c >= '0' && c <= '9')) {result += c;}
            else if (c == '(') {st.push('(');}

            // If the scanned character is a ‘)’, pop from stack until an ‘(‘ is encountered
            else if (c == ')') {
                while (st.top() != '(') {
                    result += st.top();
                    st.pop();
                }
                st.pop();  // Pop the ‘(‘ from the stack
            }

            // If an operator is scanned
            else {
                while (!st.empty() && prec(s[i]) <= prec(st.top())) {
                    result += st.top();
                    st.pop();
                }
                st.push(c);  // Push the current operator to the stack
            }
        }

        // Pop all the remaining elements from the stack
        while (!st.empty()) {
            result += st.top();
            st.pop();
        }

        cout << "Postfix expression: " << result << endl;  // Output the result
    }
    ```

### ***Infix to Prefix***

- Given an infix expression, Your task is to convert the given infix expression to a prefix expression.
- `x + y * z / w + u` to `++x/*yzwu`
- `a + b` to `+ab`

- ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    // Function to check if a character is an operator
    bool isOperator(char c) {
        return (!isalpha(c) && !isdigit(c));  // If the character is neither alphabetic nor numeric, it's an operator
    }

    // Function to return the precedence of operators
    int getPriority(char C) {
        if (C == '-' || C == '+')  // Addition and subtraction have lowest precedence
            return 1;
        else if (C == '*' || C == '/')  // Multiplication and division have higher precedence
            return 2;
        else if (C == '^')  // Exponent operator has highest precedence
            return 3;
        return 0;
    }

    // Function to convert infix expression to postfix expression
    string infixToPostfix(string infix) {
        infix = '(' + infix + ')';  // Add parentheses to handle edge cases
        int l = infix.size();
        stack<char> char_stack;  // Stack to store operators
        string output;  // String to store the resulting postfix expression

        for (int i = 0; i < l; i++) {

            // If the scanned character is an operand, add it to output
            if (isalpha(infix[i]) || isdigit(infix[i]))
                output += infix[i];

            // If the scanned character is ‘(’, push it to the stack
            else if (infix[i] == '(')
                char_stack.push('(');

            // If the scanned character is ‘)’, pop and output from the stack until an ‘(‘ is encountered
            else if (infix[i] == ')') {
                while (char_stack.top() != '(') {
                    output += char_stack.top();
                    char_stack.pop();
                }
                char_stack.pop();  // Remove '(' from the stack
            }

            // If an operator is found
            else {
                if (isOperator(char_stack.top())) {
                    if (infix[i] == '^') {
                        while (getPriority(infix[i]) <= getPriority(char_stack.top())) {
                            output += char_stack.top();
                            char_stack.pop();
                        }
                    } else {
                        while (getPriority(infix[i]) < getPriority(char_stack.top())) {
                            output += char_stack.top();
                            char_stack.pop();
                        }
                    }
                    // Push current operator on stack
                    char_stack.push(infix[i]);
                }
            }
        }
        
        // Pop all remaining elements from the stack
        while (!char_stack.empty()) {
            output += char_stack.top();
            char_stack.pop();
        }
        return output;  // Return the postfix expression
    }

    // Function to convert infix expression to prefix expression
    string infixToPrefix(string infix) {
        int l = infix.size();

        // Reverse the infix expression
        reverse(infix.begin(), infix.end());

        // Replace '(' with ')' and vice versa
        for (int i = 0; i < l; i++) {
            if (infix[i] == '(') {
                infix[i] = ')';
                i++;
            } else if (infix[i] == ')') {
                infix[i] = '(';
                i++;
            }
        }

        string prefix = infixToPostfix(infix);  // Convert the modified infix to postfix

        // Reverse the postfix expression to get the prefix
        reverse(prefix.begin(), prefix.end());

        return prefix;  // Return the prefix expression
    }

    int main() {
        string s = "(p+q)*(c-d)";  // Infix expression
        cout << "Infix expression: " << s << endl;
        cout << "Prefix Expression: " << infixToPrefix(s) << endl;  // Output the prefix expression
        return 0;
    }
    ```

### ***Prefix to Infix***

- You are given a valid arithmetic expression in prefix notation. Your task is to convert it into a fully parenthesized infix expression. Prefix notation (also known as Polish notation) places the operator before its operands. In contrast, infix notation places the operator between operands.

- `+ab` to `(a+b)`
- `*+ab-cd` to `((a+b)*(c-d))`

- ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    // Function to convert prefix to infix
    string prefixToInfix(string prefix) {
        stack<string> s;
        int n = prefix.size();

        // Traverse the prefix expression from right to left
        for (int i = n - 1; i >= 0; i--) {
            char c = prefix[i];

            // If the character is an operand, push it to the stack
            if (isalnum(c)) {
                s.push(string(1, c));
            } else {
                // Pop two operands from the stack
                string op1 = s.top(); s.pop();
                string op2 = s.top(); s.pop();

                // Form the new infix expression and push back to stack
                s.push("(" + op1 + c + op2 + ")");
            }
        }

        // The final element in the stack is the result
        return s.top();
    }

    int main() {
        string prefix = "*-A/BC-/AKL";
        cout << "Infix Expression: " << prefixToInfix(prefix) << endl;
        return 0;
    }
    ```

### ***Prefix to Postfix***

- You are given a valid prefix expression consisting of binary operators and single-character operands. Your task is to convert it into a valid postfix expression.Prefix (Polish) notation places the operator before operands. Postfix (Reverse Polish) notation places the operator after operands.

- `+ab` to `ab+`
- `*+ab-c` to `ab+cd-*`

- ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    // Function to convert prefix to postfix
    string prefixToPostfix(string prefix) {
        stack<string> s;
        int n = prefix.size();

        // Traverse the prefix expression from right to left
        for (int i = n - 1; i >= 0; i--) {
            char c = prefix[i];

            // If the character is an operand, push it to the stack
            if (isalnum(c)) {
                s.push(string(1, c));
            } else {
                // Pop two operands from the stack
                string op1 = s.top(); s.pop();
                string op2 = s.top(); s.pop();

                // Form the new postfix expression and push back to stack
                s.push(op1 + op2 + c);
            }
        }

        // The final element in the stack is the result
        return s.top();
    }

    int main() {
        string prefix = "*-A/BC-/AKL";
        cout << "Postfix Expression: " << prefixToPostfix(prefix) << endl;
        return 0;
    }
    ```

### ***Postfix to Prefix***

- You are given a valid postfix expression as a string, where: Operands are single lowercase English letters ('a' to 'z'). Operators are binary: '+', '-', '*', '/'. The expression contains no spaces and is guaranteed to be valid. Write a function to convert the postfix expression into a prefix expression, also as a string without spaces.

- `ab+` to `+ab`
- `abc*+d-` to `-+a*bcd`

- ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    // Function to convert postfix to prefix
    string postfixToPrefix(string postfix) {
        stack<string> s;
        int n = postfix.size();

        // Traverse the postfix expression from left to right
        for (int i = 0; i < n; i++) {
            char c = postfix[i];

            // If the character is an operand, push it to the stack
            if (isalnum(c)) {
                s.push(string(1, c));
            } else {
                // Pop two operands from the stack
                string op2 = s.top(); s.pop();
                string op1 = s.top(); s.pop();

                // Form the new prefix expression and push back to stack
                s.push(c + op1 + op2);
            }
        }

        // The final element in the stack is the result
        return s.top();
    }

    int main() {
        string postfix = "ABC/-AK/L-*";
        cout << "Prefix Expression: " << postfixToPrefix(postfix) << endl;
        return 0;
    }
    ```

### ***Postfix to Infix***

- Given a postfix expression (a string), convert it into an equivalent infix expression. The postfix expression is evaluated from left to right. The infix expression should have the proper parentheses to ensure correct operator precedence.

- `ab+c*` to `(a+b)*c`
- `ab*cd/+` to `(a*b)+(c/d)`

- ```cpp
    #include <iosbits/stdc++.htream>
    using namespace std;

    // Function to convert postfix to infix
    string postfixToInfix(string postfix) {
        stack<string> s;
        int n = postfix.size();

        // Traverse the postfix expression from left to right
        for (int i = 0; i < n; i++) {
            char c = postfix[i];

            // If the character is an operand, push it to the stack
            if (isalnum(c)) {
                s.push(string(1, c));
            } else {
                // Pop two operands from the stack
                string op2 = s.top(); s.pop();
                string op1 = s.top(); s.pop();

                // Form the new infix expression and push back to stack
                s.push("(" + op1 + c + op2 + ")");
            }
        }

        // The final element in the stack is the result
        return s.top();
    }

    int main() {
        string postfix = "AB*C+";
        cout << "Infix Expression: " << postfixToInfix(postfix) << endl;
        return 0;
    }
    ```

## Part 3 - Monotonic Stack Queue Problem

- Next Greater Element: Given an array, for each element find next greater element. [6,8,0,1,3]->[8,-1,1,3,-1]. Brute force is easy just iterate and find but it will take N*N time. So we use stack as optimal. So I solved this using stack and coded and was able to code but it took me time because not practicing in C++. Basically iterate array from backwards and for each element write what to do when stack is empty and stack is not empty. when stack is not empty what happen when current element is less than stack top and greater than stack top. If you can dry run using copy pen, you can code. Also I have written a lot of redundant line. Compare with online solution.

- Next Greater Element-2: Similar as above but here the arrary is circular. *****

- Next Smaller Element: Similar to NGE just the condition for greater than smaller than sign change in code.

- Number of NGE to right: Not on striver but worth solving because it is hard and require some kind of merge sort.

- Trapping Rainwater: 

- Sum of subarray minimums:

- Asteroid Collision: Doable if you can catch its using stack. Code it though for practice.

- Sum of subarray ranges: Same as subarray minimum logic.

- Remove K digits: 

- Largest Rectangle in a histogram: 

- Maximum Rectangle: 

## ***Part 4 - Implementation Problem***

### ***Sliding Window Maximum***

- Given an array of integers arr, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.

- Input: arr = [4,0,-1,3,5,3,6,8], k = 3. Output: [4,3,5,5,6,8]. For each window of size k=3, we find the maximum element in the window and add it to our output array.

- Brute: Every time the sliding window moves, we manually check all the elements within that window and pick the maximum.
- ```cpp
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> result;
        for (int i = 0; i <= nums.size() - k; i++) {
            int maxVal = nums[i];
            for (int j = i; j < i + k; j++) {
                maxVal = max(maxVal, nums[j]);
            }
            result.push_back(maxVal);
        }
        return result;
    }
    ```

- Optimal in O(N): The real concern is only when the outgoing element was the maximum. To optimize, we use a double-ended queue (deque) to maintain elements in a way that always keeps track of the current maximum efficiently. When a new element enters, we push it to the back of the deque, but before that, we remove all smaller elements from the back since they're not useful anymore. Also, if the element at the front is outside the window's range, we remove it. This ensures that the element at the front of the deque always represents the maximum of the current window.
- ```cpp
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> dq; // Deque to store indices of useful elements in the current window
        vector<int> result;

        for (int i = 0; i < nums.size(); i++) {
            // Remove elements from the front if they are out of this window's range
            if (!dq.empty() && dq.front() <= i - k) { dq.pop_front(); }

            // Remove all elements from the back that are smaller than current element
            while (!dq.empty() && nums[dq.back()] < nums[i]) { dq.pop_back(); }

            // Add the current index to the deque
            dq.push_back(i);

            // Once the first window is completed, add front element to result
            if (i >= k - 1) { result.push_back(nums[dq.front()]); }
        }

        return result;
    }
    ```

### ***Stock Span Problem***

- Given an array arr of size n, where each element arr[i] represents the stock price on day i. Calculate the span of stock prices for each day. The span Sᵢ for a specific day i is defined as the maximum number of consecutive previous days (including the current day) for which the stock price was less than or equal to the price on day i.

- ```
    Example 1:
    Input:
    n = 7, arr = [120, 100, 60, 80, 90, 110, 115]
    Output:
    1 1 1 2 3 5 6
    Explanation:

    Traversing the given input span:
    120 is greater than or equal to 120 and there are no more elements behind it so the span is 1,
    100 is greater than or equal to 100 and smaller than 120 so the span is 1,
    60 is greater than or equal to 60 and smaller than 100 so the span is 1,
    80 is greater than or equal to 60, 80 and smaller than 100 so the span is 2,
    90 is greater than or equal to 60, 80, 90 and smaller than 100 so the span is 3,
    110 is greater than or equal to 60, 80, 90, 100, 110 and smaller than 120 so the span is 5,
    115 is greater than or equal to all previous elements and smaller than 120 so the span is 6.
    Hence the output will be 1 1 1 2 3 5 6.

    Example 2:
    Input:
    n = 6, arr = [15, 13, 12, 14, 16, 20]
    Output:
    1 1 1 3 5 6
    Explanation:

    Traversing the given input span:
    15 is greater than or equal to 15 and there are no more elements behind it, so the span is 1.
    13 is smaller than 15, so the span is 1.
    12 is smaller than 13, so the span is 1.
    14 is greater than or equal to 12 and 13, but smaller than 15, so the span is 3 (days with values 12, 13, and 14).
    16 is greater than or equal to 14, 12, 13, and 15, so the span is 5.
    20 is greater than or equal to all previous elements, so the span is 6.
    Hence the output will be 1 1 1 3 5 6.
    ```

- Brute in O(N^2). Just use two loops.
- ```cpp
    vector <int> stockSpan(vector<int> arr, int n) {
        vector<int> ans(n);
        for(int i=0; i < n; i++) {
            int currSpan = 0;
            for(int j=i; j >= 0; j--) {
                if(arr[j] <= arr[i]) { currSpan++; }
                else break;
            }
            ans[i] = currSpan;
        }
        return ans;
    }
    ```

- Optimal in O(N)
- ```cpp
    vector<int> findPGE(vector<int> arr) {
        int n = arr.size();
        vector<int> ans(n);
        stack<int> st;

        for(int i=0; i < n; i++) {
            int currEle = arr[i];
            
            /* Pop the elements in the stack until the stack is not empty and the top element is not the greater element */
            while(!st.empty() && arr[st.top()] <= currEle) { st.pop(); }
            
            if(st.empty()) {ans[i] = -1;} /* If the greater element is not found, stack will be empty */
            else {ans[i] = st.top();} // Else store the answer 

            st.push(i);// Push the current index in the stack
        }
        return ans;
    }
    
    vector <int> stockSpan(vector<int> arr, int n) {
        // Get the indices of previous greater elements
        vector<int> PGE = findPGE(arr);
        vector<int> ans(n);
        for(int i=0; i < n; i++) { ans[i] = i - PGE[i]; }
        return ans;
    }
    ```

### ***Celebrity Problem***

- A celebrity is a person who is known by everyone else at the party but does not know anyone in return. Given a square matrix M of size N x N where M[i][j] is 1 if person i knows person j, and 0 otherwise, determine if there is a celebrity at the party. Return the index of the celebrity or -1 if no such person exists. Note that M[i][i] is always 0.

- Brute in O(N^2)
- ```cpp
    int celebrity(vector<vector<int>> &M){
        int n = M.size();
        
        /* To store count of people who know person of index i */
        vector<int> knowMe(n, 0);
        
        /* To store count of people who the person of index i knows */
        vector<int> Iknow(n, 0);
        
        for(int i=0; i < n; i++) {
            for(int j=0; j < n; j++) {
                if(M[i][j] == 1) {
                    knowMe[j]++;
                    Iknow[i]++;
                }
            }
        }
        
        for(int i=0; i < n; i++) {
            if(knowMe[i] == n-1 && Iknow[i] == 0) {
                return i;  
            }
        }
        
        return -1;
    }
    ```

- Optimal in O(N)
- ```cpp
    int celebrity(vector<vector<int>> &M){
        int n = M.size();
        int top = 0, down = n-1;
        
        // Traverse for all the people
        while(top < down) {
            
            /* If top knows down, it can not be a celebrity */
            if(M[top][down] == 1) {
                top = top + 1;
            }
            
            /* If down knowns top, it can not be a celebrity */
            else if(M[down][top] == 1) {
                down = down - 1;
            }
            
            /* If both does not know each other, 
            both cannot be the celebrity */
            else {
                top++;
                down--;
            }
        }
        
        // Return -1 if no celebrity is found
        if(top > down) return -1;
        
        /* Check if the person pointed by top is celebrity */
        for(int i=0; i < n; i++) {
            if(i == top) continue;
            // Check if it is not a celebrity
            if(M[top][i] == 1 || M[i][top] == 0) {
                return -1;
            }
        }

        // Return the index of celebrity
        return top;
    }
    ```

### ***LRU Cache***

- Design a data structure that follows the constraints of a Least Recently Used (LRU) cache. The functions get and put must each run in O(1) average time complexity. Implement the LRUCache class:
    - LRUCache(int capacity): Initialize the LRU cache with positive size capacity.
    - int get(int key): Return the value of the key if the key exists, otherwise return -1.
    - void put(int key, int value): Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.

- The intuition behind an LRU (Least Recently Used) Cache is that we want to store only a fixed number of items in memory and quickly evict the item that hasn’t been used for the longest time. This is useful when memory is limited and we want to keep the most relevant data available for fast retrieval. The key idea is to maintain quick lookups to check if a value exists in the cache, and also maintain the usage order so we can remove the least recently used item efficiently when the cache is full.

- To implement it efficiently, we combine two data structures: a HashMap for O(1) lookup of keys, and a Doubly Linked List to maintain the order of usage. The most recently used items are kept at one end (head), and the least recently used items at the other end (tail). When we access or insert a key, we move it to the head whereas when the cache is full, we remove the tail node. This combination ensures both O(1) access and O(1) insertion/deletion for LRU operations.

- ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    // Class representing the LRU Cache
    class LRUCache {
    public:
        // Doubly linked list node class
        class Node {
        public:
            int key;
            int val;
            Node* next;
            Node* prev;
            // Constructor to initialize node
            Node(int _key, int _val) {
                key = _key;
                val = _val;
            }
        };

        // Head and tail dummy nodes
        Node* head = new Node(-1, -1);
        Node* tail = new Node(-1, -1);

        // Capacity of cache
        int cap;
        // Hash map to store key-node mapping
        unordered_map<int, Node*> m;

        // Constructor to initialize LRU cache
        LRUCache(int capacity) {
            cap = capacity;
            head->next = tail;
            tail->prev = head;
        }

        // Function to add a node right after head
        void addNode(Node* newNode) {
            Node* temp = head->next;
            newNode->next = temp;
            newNode->prev = head;
            head->next = newNode;
            temp->prev = newNode;
        }

        // Function to remove a given node from list
        void deleteNode(Node* delNode) {
            Node* delPrev = delNode->prev;
            Node* delNext = delNode->next;
            delPrev->next = delNext;
            delNext->prev = delPrev;
        }

        // Function to get value from cache
        int get(int key_) {
            // If key exists in cache
            if (m.find(key_) != m.end()) {
                Node* resNode = m[key_];
                int res = resNode->val;
                // Remove old mapping
                m.erase(key_);
                // Move accessed node to front
                deleteNode(resNode);
                addNode(resNode);
                // Update map
                m[key_] = head->next;
                return res;
            }
            // If not found
            return -1;
        }

        // Function to put key-value into cache
        void put(int key_, int value) {
            // If key already exists
            if (m.find(key_) != m.end()) {
                Node* existingNode = m[key_];
                m.erase(key_);
                deleteNode(existingNode);
            }
            // If capacity reached
            if (m.size() == cap) {
                m.erase(tail->prev->key);
                deleteNode(tail->prev);
            }
            // Insert new node at front
            addNode(new Node(key_, value));
            m[key_] = head->next;
        }
    };

    // Driver code
    int main() {
        // Create cache with capacity 2
        LRUCache cache(2);

        // Put values in cache
        cache.put(1, 1);
        cache.put(2, 2);

        // Get value for key 1
        cout << cache.get(1) << endl; 

        // Insert another key (evicts key 2)
        cache.put(3, 3);

        // Key 2 should be evicted
        cout << cache.get(2) << endl; 

        // Insert another key (evicts key 1)
        cache.put(4, 4);

        // Key 1 should be evicted
        cout << cache.get(1) << endl; 

        // Key 3 should be present
        cout << cache.get(3) << endl; 

        // Key 4 should be present
        cout << cache.get(4) << endl; 

        return 0;
    }
    ```

### ***LFU Cache***

- Design and implement a data structure for a Least Frequently Used (LFU) cache. Implement the LFUCache class with the following functions:
    - LFUCache(int capacity): Initialize the object with the specified capacity.
    - int get(int key): Retrieve the value of the key if it exists in the cache; otherwise, return -1.
    - void put(int key, int value): Update the value of the key if it is present in the cache, or insert the key if it is not already present. If the cache has reached its capacity, invalidate and remove the least frequently used key before inserting a new item. In case of a tie (i.e., two or more keys with the same frequency), invalidate the least recently used key.

- A use counter is maintained for each key in the cache to determine the least frequently used key. The key with the smallest use counter is considered the least frequently used.

- When a key is first inserted into the cache, its use counter is set to 1 due to the put operation. The use counter for a key in the cache is incremented whenever a get or put operation is called on it. Ensure that the functions get and put run in O(1) average time complexity.

- ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    /* To implement a node in doubly linked 
    list that will store data items */
    struct Node {
    int key, value, cnt;
    Node *next; 
    Node *prev;
    Node(int _key, int _value) {
        key = _key;
        value = _value; 
        cnt = 1; 
    }
    }; 

    // To implement the doubly linked list
    struct List {
    int size; // Size 
    Node *head; // Dummy head
    Node *tail; // Dummy tail
    
    // Constructor
    List() {
        head = new Node(0, 0); 
        tail = new Node(0,0); 
        head->next = tail;
        tail->prev = head; 
        size = 0;
    }
    
    // Function to add node in front 
    void addFront(Node *node) {
        Node* temp = head->next;
        node->next = temp;
        node->prev = head;
        head->next = node;
        temp->prev = node;
        size++; 
    }
    
    // Function to remove node from the list
    void removeNode(Node* delnode) {
        Node* prevNode = delnode->prev;
        Node* nextNode = delnode->next;
        prevNode->next = nextNode;
        nextNode->prev = prevNode;
        size--; 
    }
    };

    // Class to implement LFU cache
    class LFUCache {
    private:

    // Hashmap to store the key-nodes pairs
    map<int, Node*> keyNode; 
    
    /* Hashmap to maintain the lists 
    having different frequencies */
    map<int, List*> freqListMap; 
    
    int maxSizeCache; // Max size of cache
    
    /* To store the frequency of least 
    frequently used data-item */
    int minFreq; 
    
    // To store current size of cache
    int curSize; 
    
    public:

    // Constructor
    LFUCache(int capacity) {
        // Set the capacity
        maxSizeCache = capacity; 
        minFreq = 0; // Set minimum frequency
        curSize = 0; // Set current frequency
    }
    
    // Method to update frequency of data-items
    void updateFreqListMap(Node *node) {
        
        // Remove from Hashmap
        keyNode.erase(node->key); 
        
        // Update the frequency list hashmap
        freqListMap[node->cnt]->removeNode(node); 
        
        // If node was the last node having it's frequency
        if(node->cnt == minFreq && 
            freqListMap[node->cnt]->size == 0) {
                
            // Update the minimum frequency
            minFreq++; 
        }
        
        // Creating a dummy list for next higher frequency
        List* nextHigherFreqList = new List();
        
        // If the next higher frequency list already exists
        if(freqListMap.find(node->cnt + 1) != 
            freqListMap.end()) {
                
            // Update pointer to already existing list
            nextHigherFreqList = freqListMap[node->cnt + 1];
        } 
        
        // Increment the count of data-item
        node->cnt += 1; 
        
        // Add the node in front of higher frequency list
        nextHigherFreqList->addFront(node); 
        
        // Update the 
        freqListMap[node->cnt] = nextHigherFreqList; 
        keyNode[node->key] = node;
    }
    
    // Method to get the value of key from LFU cache
    int get(int key) {
        
        // Return the value if key exists
        if(keyNode.find(key) != keyNode.end()) {
            Node* node = keyNode[key]; // Get the node
            int val = node->value; // Get the value
            updateFreqListMap(node); // Update the frequency
            
            // Return the value
            return val; 
        }
        
        // Return -1 if key is not found
        return -1; 
    }
    
    void put(int key, int value) {
        /* If the size of Cache is 0, 
        no data-items can be inserted */
        if (maxSizeCache == 0) {
            return;
        }
        
        // If key already exists
        if(keyNode.find(key) != keyNode.end()) {
            
            // Get the node
            Node* node = keyNode[key]; 
            
            // Update the value
            node->value = value; 
            
            // Update the frequency
            updateFreqListMap(node); 
        }
        
        // Else if the key does not exist
        else {
            
            // If cache limit is reached
            if(curSize == maxSizeCache) {
                
                // Remove the least frequently used data-item
                List* list = freqListMap[minFreq]; 
                keyNode.erase(list->tail->prev->key); 
                
                // Update the frequency map 
                freqListMap[minFreq]->removeNode(
                    list->tail->prev
                );
                
                // Decrement the current size of cache
                curSize--; 
            }
            
            // Increment the current cache size
            curSize++; 
            
            // Adding new value to the cache
            minFreq = 1; // Set its frequency to 1
            
            // Create a dummy list
            List* listFreq = new List(); 
            
            // If the list already exist
            if(freqListMap.find(minFreq) != 
                freqListMap.end()) {
                
                // Update the pointer to already present list
                listFreq = freqListMap[minFreq]; 
            }
            
            // Create the node to store data-item
            Node* node = new Node(key, value); 
            
            // Add the node to dummy list
            listFreq->addFront(node);
            
            // Add the node to Hashmap
            keyNode[key] = node; 
            
            // Update the frequency list map 
            freqListMap[minFreq] = listFreq; 
        }
    }
    };

    int main() {
    // LFU Cache
    LFUCache cache(2);

    // Queries
    cache.put(1, 1);
    cache.put(2, 2);
    cout << cache.get(1) << " ";
    cache.put(3, 3);
    cout << cache.get(2) << " ";
    cout << cache.get(3) << " ";
    cache.put(4, 4);
    cout << cache.get(1) << " ";
    cout << cache.get(3) << " ";
    cout << cache.get(4) << " ";

    return 0;
    }
    ```

