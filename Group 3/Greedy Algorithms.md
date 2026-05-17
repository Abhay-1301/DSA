# ***Greedy***

- Assign Cookies
- Fractional Knapsack Problem : Greedy Approach
- Lemonade Change
- Valid Paranthesis Checker
- N meetings in one room
- Jump Game - I
- Jump Game - II
- Minimum number of platforms required for a railway
- Job Sequencing Problem
- Candy
- Shortest Job First (or SJF) CPU Scheduling
- Program for Least Recently Used (LRU) Page Replacement Algorithm
- Insert Interval
- Merge Overlapping Sub-intervals
- Non-overlapping Intervals
___
### ***Assign Cookies***

- Problem Statement: Consider a scenario where a teacher wants to distribute cookies to students, with each student receiving at most one cookie. Given two arrays, student and cookie, the ith value in the student array describes the minimum size of cookie that the ith student can be assigned. The jth value in the cookie array represents the size of the jth cookie. If cookie[j] >= student[i], the jth cookie can be assigned to the ith student. Maximize the number of students assigned with cookies and output the maximum number.

- DP: O(N*M)
- ```cpp
    int findContentChildren(vector<int>& student, vector<int>& cookie) {
        sort(student.begin(), student.end());
        sort(cookie.begin(), cookie.end());

        vector<vector<int>> memo(student.size(), vector<int>(cookie.size(), -1));
        return helper(0, 0, student, cookie, memo);
    }

    int helper(int studentIndex, int cookieIndex, vector<int>& student, vector<int>& cookie, vector<vector<int>>& memo) {
        if (studentIndex >= student.size() || cookieIndex >= cookie.size())
            return 0;

        if (memo[studentIndex][cookieIndex] != -1) return memo[studentIndex][cookieIndex];

        int result = 0;

        if (cookie[cookieIndex] >= student[studentIndex]) {
            result = max(result, 1 + helper(studentIndex + 1, cookieIndex + 1, student, cookie, memo));
        } // Option 1: assign this cookie and move to next student and cookie
        // Option 2: skip this cookie and try the next one for the same student
        result = max(result, helper(studentIndex, cookieIndex + 1, student, cookie, memo));

        return memo[studentIndex][cookieIndex] = result;
    }
    ```

- Greedy: O(N * logN + M * logM)
- ```cpp
    int findContentChildren(vector<int>& student, vector<int>& cookie) {
        sort(student.begin(), student.end());
        sort(cookie.begin(), cookie.end());

        int studentIndex = 0; 
        int cookieIndex = 0;  

        // Try to assign cookies until any one list is fully processed
        while (studentIndex < student.size() && cookieIndex < cookie.size()) {
            if (cookie[cookieIndex] >= student[studentIndex]) { studentIndex++; }
            cookieIndex++; // Move to next cookie in both cases
        }
        return studentIndex; // Number of students satisfied is equal to studentIndex
    }
    ```

### ***Fractional Knapsack Problem : Greedy Approach***

- Problem Statement: The weight of N items and their corresponding values are given. We have to put these items in a knapsack of weight W such that the total value obtained is maximized.Note: We can either take the item as a whole or break it into smaller units.

- ```cpp
    struct Item {
        int value;
        int weight;
    };

    class Solution {
    public:
        // Comparator function to sort items by value/weight ratio
        bool static comp(Item a, Item b) {
            double r1 = (double) a.value / (double) a.weight;
            double r2 = (double) b.value / (double) b.weight;
            return r1 > r2;  // Return true if the ratio of item a is greater than item b
        }

        // Function to calculate the maximum value we can get with fractional knapsack
        double fractionalKnapsack(int W, Item arr[], int n) {
            sort(arr, arr + n, comp); // Sort items based on the value/weight ratio

            int curWeight = 0;  // Current weight of knapsack
            double finalvalue = 0.0;  // Final value we can achieve

            // Iterate through the sorted items
            for (int i = 0; i < n; i++) {
                if (curWeight + arr[i].weight <= W) { // If the current item can be fully added to the knapsack
                    curWeight += arr[i].weight;
                    finalvalue += arr[i].value;  // Add the full value of the item
                }
                else { // If the current item can't be fully added, take the fractional part
                    int remain = W - curWeight;
                    finalvalue += (arr[i].value / (double) arr[i].weight) * (double) remain;
                    break;  // Break as we have filled the knapsack
                }
            }
            return finalvalue;  // Return the maximum value that can be carried
        }
    };
    ```

### ***Lemonade Change***

- Problem Statement: Given an array representing a queue of customers and the value of bills they hold, determine if it is possible to provide correct change to each customer. Customers can only pay with 5$, 10$ or 20$ bills and we initially do not have any change at hand. Return true, if it is possible to provide correct change for each customer otherwise return false.

- ```cpp
    bool lemonadeChange(vector<int>& bills) {
        int five = 0; // Counter for $5 bills
        int ten = 0;  // Counter for $10 bills

        // Process each customer's bill
        for (int bill : bills) {
            if (bill == 5) {
                // Customer pays with $5 -> no change needed
                five++;
            }
            else if (bill == 10) {
                // Customer pays with $10 -> needs $5 as change
                if (five > 0) {
                    five--; // Give one $5 as change
                    ten++;  // Accept the $10 bill
                } else {
                    return false; // Cannot provide change
                }
            }
            else { // bill == 20
                // Customer pays with $20 -> needs $15 as change
                if (five > 0 && ten > 0) {
                    five--; // Use one $5
                    ten--;  // Use one $10
                } 
                else if (five >= 3) {
                    five -= 3; // Use three $5 bills
                } 
                else {
                    return false; // Cannot provide change
                }
            }
        }
        return true; // Successfully gave change to all customers
    }
    ```


### ***Valid Paranthesis Checker***

- Problem Statement: Find the validity of an input string s that only contains the letters `(`, `)` and `*`. A string entered is legitimate if Any left parenthesis `(` must have a corresponding right parenthesis `)`. right parenthesis `)` must have a corresponding left parenthesis `(`. Left parenthesis `(` must go before the corresponding right parenthesis `)`.could be treated as a single right parenthesis `)` or a single left parenthesis `(` or an empty string "".

- Brute: O(3^N)
- ```cpp
    bool isValid(string& s, int i, int open) {
        // If open becomes negative, the string is invalid
        if (open < 0) return false;

        // If we reach the end, check if all opens are closed
        if (i == s.length()) return open == 0;

        // If current character is '(', treat it as an opening bracket
        if (s[i] == '(') {
            return isValid(s, i + 1, open + 1);
        } 
        // If current character is ')', treat it as a closing bracket
        else if (s[i] == ')') {
            return isValid(s, i + 1, open - 1);
        } 
        // If character is '*', try all 3 options
        else {
              // Treat '*' as empty
              // Treat '*' as '('
              // Treat '*' as ')'
            return isValid(s, i + 1, open) ||       
                   isValid(s, i + 1, open + 1) ||    
                   isValid(s, i + 1, open - 1);      
        }
    }
    ```

- Optimal: O(N)
- ```cpp
    bool isValid(string s) {
        // Track minimum and maximum open brackets
        int minOpen = 0, maxOpen = 0;

        // Traverse each character in the string
        for (char c : s) {
            if (c == '(') {
                minOpen++;
                maxOpen++;
            } else if (c == ')') {
                minOpen--;
                maxOpen--;
            } else {
                // Treat '*' as '(', ')' or ''
                minOpen--;
                maxOpen++;
            }

            // If maxOpen goes negative, too many closing brackets
            if (maxOpen < 0) return false;

            // minOpen can't be negative
            minOpen = max(minOpen, 0);
        }

        // String is valid if all opens are closed
        return minOpen == 0;
    }
    ```

### ***N meetings in one room***

- Problem Statement: There is one meeting room in a firm. You are given two arrays, start and end each of size N. For an index ‘i’, start[i] denotes the starting time of the ith meeting while end[i] will denote the ending time of the ith meeting. Find the maximum number of meetings that can be accommodated if only one meeting can happen in the room at a particular time. Print the order in which these meetings will be performed.

- ```cpp
    vector<int> maxMeetings(vector<int>& start, vector<int>& end) {
        vector<tuple<int, int, int>> meetings; // Store meetings as (end_time, start_time, original_index)
        for (int i = 0; i < start.size(); i++) {
            meetings.push_back({end[i], start[i], i + 1}); // i+1 for 1-based indexing  
        }

        sort(meetings.begin(), meetings.end()); // Sort by end time

        vector<int> result; // To store meeting indices
        int lastEnd = -1;

        // Traverse sorted meetings
        for (auto& m : meetings) {
            int e = get<0>(m); int s = get<1>(m); int idx = get<2>(m);

            // If meeting starts after last one ends
            if (s > lastEnd) {
                result.push_back(idx); // Store index
                lastEnd = e; // Update last end time
            }
        }
        return result;
    }
    ```

### ***Jump Game - I***

- Problem Statement: Given an array where each element represents the maximum number of steps you can jump forward from that element, return true if we can reach the last index starting from the first index. Otherwise, return false.

- ```cpp
    bool canJump(vector<int>& nums) {
        int maxIndex = 0; // The farthest index we can currently reach

        for (int i = 0; i < nums.size(); i++) { 
            if (i > maxIndex) { // If current index is beyond the farthest reachable point. We cannot move further
                return false;
            }            
            maxIndex = max(maxIndex, i + nums[i]); // Update the farthest index we can reach
        }
        return true; // If we finish the loop, we can reach the last index
    }
    ```

### ***Jump Game - II***

- Problem Statement: You are given a 0-indexed array nums of length n representing your maximum jump capability from each index. You start at index 0. Each element nums[i] represents the maximum number of steps you can jump forward from index i. Your goal is to reach the last index of the array (nums[n - 1]) using the minimum number of jumps. Return the minimum number of jumps required to reach the last index. You can assume that it is always possible to reach the last index.

- Brute: O(2^N)
- ```cpp
    int jump(vector<int>& nums) {
        return minJumps(nums, 0);
    }

    // Recursive function to explore all possible jump paths
    int minJumps(vector<int>& nums, int position) {
        if (position >= nums.size() - 1) return 0; // If we are already at or beyond the last index, no more jumps needed
        if (nums[position] == 0) return INT_MAX; // If we can't move from current position

        int minStep = INT_MAX;

        // Try every possible jump from 1 to nums[position]
        for (int jump = 1; jump <= nums[position]; ++jump) {
            int subResult = minJumps(nums, position + jump);
            if (subResult != INT_MAX) { minStep = min(minStep, 1 + subResult); }
        }
        return minStep;
    }
    ```

- Better: O(N^2)
- ```cpp
    int jump(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(n, INT_MAX);
        dp[0] = 0;

        // Iterate through all indices
        for (int i = 0; i < n; ++i) {
            // For each position, calculate max jump
            for (int j = 1; j <= nums[i] && i + j < n; ++j) {
                dp[i + j] = min(dp[i + j], dp[i] + 1);
            }
        }
        // Return the minimum jumps to reach last index
        return dp[n - 1];
    }
    ```


- Optimal: O(N)
- ```cpp
    int jump(vector<int>& nums) {
        // Initialize variables to keep track of range and jumps
        int jumps = 0, currentEnd = 0, farthest = 0;

        // Traverse through the array (excluding the last element)
        for (int i = 0; i < nums.size() - 1; ++i) {
            // Update the farthest index that can be reached so far
            farthest = max(farthest, i + nums[i]);

            // When we reach the end of the current jump range
            if (i == currentEnd) {
                // We need to make a jump
                jumps++;

                // Move the current end to the farthest index we can reach
                currentEnd = farthest;
            }
        }

        // Return the total jumps needed
        return jumps;
    }
    ```

### ***Minimum number of platforms required for a railway***

- Problem Statement: We are given two arrays that represent the arrival and departure times of trains that stop at the platform. We need to find the minimum number of platforms needed at the railway station so that no train has to wait.

- Brute: O(N^2)
- ```cpp
    int countPlatforms(int n, int arr[], int dep[]) {

        // Initialize answer to 1
        int ans = 1;

        // Loop over all arrival times
        for (int i = 0; i < n; i++) {

            // Initialize count of overlapping intervals
            int count = 1;

            // Check overlap with every other train
            for (int j = i + 1; j < n; j++) {

                // Check if there is overlap between train i and j
                if ((arr[i] >= arr[j] && arr[i] <= dep[j]) ||
                    (arr[j] >= arr[i] && arr[j] <= dep[i])) {
                    count++;
                }
            }

            // Update maximum platform count
            ans = max(ans, count);
        }

        return ans;
    }
    ```

- Optimal: O(N * logN)
- ```cpp
    int countPlatforms(int n, int arr[], int dep[]) {
        // Sort the arrival and departure times
        sort(arr, arr + n);
        sort(dep, dep + n);

        // Initialize pointers and counters
        int platforms = 1;
        int result = 1;
        int i = 1, j = 0;

        // Traverse both arrays
        while (i < n && j < n) {
            // If next train arrives before current one departs
            if (arr[i] <= dep[j]) {
                // One more platform needed
                platforms++;
                i++;
            } else {
                // One train departs, platform freed
                platforms--;
                j++;
            }

            // Update maximum required platforms
            result = max(result, platforms);
        }

        return result;
    }
    ```

### ***Job Sequencing Problem***

- Problem Statement: You are given a set of N jobs where each job comes with a deadline and profit. The profit can only be earned upon completing the job within its deadline. Find the number of jobs done and the maximum profit that can be obtained. Each job takes a single unit of time and only one job can be performed at a time.

-   ``` txt
    Example 1:
    Input:
    
    N = 4, Jobs = {(1, 4, 20), (2, 1, 10), (3, 1, 40), (4, 1, 30)}  
    Output:
    2 60  
    Explanation:
    
    - The 3rd job with a deadline of 1 is performed during the first unit of time.  
    - The 1st job is performed during the second unit of time as its deadline is 4.  
    Profit = 40 + 20 = 60.  
    So, the result is 2 jobs with a total profit of 60.

    Example 2:
    Input:
    
    N = 5, Jobs = {(1, 2, 100), (2, 1, 19), (3, 2, 27), (4, 1, 25), (5, 1, 15)}  
    Output:
    2 127  
    Explanation:
    
    The first and third jobs, both having a deadline of 2, give the highest profit.  
    Profit = 100 + 27 = 127.  
    So, the result is 2 jobs with a total profit of 127.
    ```

- Optimal: O(N log N) + O(N * M)
- ```cpp
    struct Job { 
    int id;
    int dead;
    int profit;};
    
    class Solution {
    public:
        bool static comparison(Job a, Job b) {
            return (a.profit > b.profit);} // Sort by profit in descending order

        pair < int, int > JobScheduling(Job arr[], int n) {
            int maxi = arr[0].dead; // Find the maximum deadline among all jobs
            for (int i = 1; i < n; i++) { maxi = max(maxi, arr[i].dead); } // Get the latest deadline

            // Initialize the slot array to track which time slots are taken
            int slot[maxi + 1];
            for (int i = 0; i <= maxi; i++) {slot[i] = -1;} // Mark all slots as unoccupied initially
                
            int countJobs = 0, jobProfit = 0;
            for (int i = 0; i < n; i++) {
                // Find a slot for the current job (starting from its deadline)
                for (int j = arr[i].dead; j > 0; j--) {
                    // If the slot is available
                    if (slot[j] == -1) {  
                        slot[j] = i;  // Assign the job to the slot
                        countJobs++;  // Increment the number of jobs done
                        jobProfit += arr[i].profit;  // Add the profit of the job
                    break;
                    }
                }
            }

            // Return the number of jobs done and the total profit
            return make_pair(countJobs, jobProfit);
        }
    };
    ```

### ***Candy***

- Problem Statement: A line of N kids is standing there. The rating values listed in the integer array ratings are assigned to each kid. These kids are receiving candy according to the following criteria: There must be at least one candy for every child. Kids whose scores are higher than their neighbours receive more candies than their neighbours. Return the minimum number of candies needed to distribute among children.

- Input: ratings = [1, 0, 5]. Output: 5. Explanation: The distribution of candies will be 2, 1, 2 to the first, second, and third child respectively.

- Input: ratings = [1, 2, 2]. Output: 4. Explanation: The distribution of candies will be 1, 2, 1 to the first, second, and third child respectively.The third gets only 1 candy because it satisfies both conditions mentioned above.

- Brute: O(N^2)
- ```cpp
    int candy(vector<int>& ratings) {
        // Total number of children
        int n = ratings.size();
        
        // Array to keep track of candies given to each child, initialized to 1
        vector<int> candies(n, 1);
        
        // Boolean flag to track if we made any change in the current iteration
        bool updated = true;

        // Repeat until no changes are made in a full scan
        while (updated) {
            updated = false;

            // Left to right pass to check increasing rating condition
            for (int i = 1; i < n; ++i) {
                if (ratings[i] > ratings[i - 1] && candies[i] <= candies[i - 1]) {
                    candies[i] = candies[i - 1] + 1;
                    updated = true;
                }
            }

            // Right to left pass to check decreasing rating condition
            for (int i = n - 2; i >= 0; --i) {
                if (ratings[i] > ratings[i + 1] && candies[i] <= candies[i + 1]) {
                    candies[i] = candies[i + 1] + 1;
                    updated = true;
                }
            }
        }

        // Return the total candies by summing the array
        return accumulate(candies.begin(), candies.end(), 0);
    }
    ```

- Better: O(N)
- ```cpp
    int candy(vector<int>& ratings) {
        int n = ratings.size();
        vector<int> candies(n, 1);

        for (int i = 1; i < n; ++i) {
            if (ratings[i] > ratings[i - 1]) candies[i] = candies[i - 1] + 1;
        }

        for (int i = n - 2; i >= 0; --i) {
            // If current rating is higher than next, adjust candy count
            if (ratings[i] > ratings[i + 1]) candies[i] = max(candies[i], candies[i + 1] + 1);
        }

        return accumulate(candies.begin(), candies.end(), 0); // Sum up all candies
    }
    ```

- Optimal: O(N). Extra space not used of O(N)
- ```cpp
    int candy(vector<int>& ratings) {

        // Get number of children
        int n = ratings.size();

        // Initially give 1 candy to each child
        int candies = n;

        // Start from second child
        int i = 1;

        while (i < n) {

            // Skip equal ratings, no need to change candy count
            if (ratings[i] == ratings[i - 1]) {
                i++;
                continue;
            }

            // Initialize increasing slope counter
            int peak = 0;

            // Traverse strictly increasing ratings
            while (i < n && ratings[i] > ratings[i - 1]) {
                peak++;
                candies += peak;
                i++;
            }

            // Initialize decreasing slope counter
            int valley = 0;

            // Traverse strictly decreasing ratings
            while (i < n && ratings[i] < ratings[i - 1]) {
                valley++;
                candies += valley;
                i++;
            }

            // Remove extra candy given to peak (overlap of increasing and decreasing)
            candies -= min(peak, valley);
        }

        // Return total minimum candies required
        return candies;
    }
    ```

### ***Shortest Job First (or SJF) CPU Scheduling***

- Problem Statement: Given a list of job durations representing the time it takes to complete each job. Implement the Shortest Job First algorithm to find the average waiting time for these jobs.

-   ```
    Example 1:
    Input:jobs = [3, 1, 4, 2, 5]
    Output: 4           
    Explanation:  
    The first job that will be executed is of duration 1 and the waiting time for it will be 0.
    After the first job, the next shortest job with a duration of 2 will be executed with a waiting time of 1.
    Following the completion of the first two jobs, the next shortest job with a duration of 3 will be executed with a waiting time of 3 (1 + 2).
    Then, the job with a duration of 4 will be executed with a waiting time of 6 (1 + 2 + 3).
    Finally, the job with the longest duration of 5 will be executed with a waiting time of 10 (1 + 2 + 3 + 4). Hence, the average waiting time is calculated as (0 + 1 + 3 + 6 + 10) / 5 = 20 / 5 = 4.


    Example 2:
    Input: jobs = [4, 3, 7, 1, 2]
    Output: 4
    Explanation: The first job that will be executed is of duration 1, and the waiting time for it will be 0.       
    After the first job, the next shortest job with a duration of 2 will be executed with a waiting time of 1.
    Following the completion of the first two jobs, the next shortest job with a duration of 3 will be executed with a waiting time of 3 (1 + 2).
    Then, the job with a duration of 4 will be executed with a waiting time of 6 (1 + 2 + 3).
    Finally, the job with the longest duration of 7 will be executed with a waiting time of 10 (1 + 2 + 3 + 4).
    Hence, the average waiting time is calculated as (0 + 1 + 3 + 6 + 10) / 5 = 20 / 5 = 4.
    ```

- Optimal: O(N * logN + N)
- ```cpp
    float calculateAverageWaitTime(vector<int>& jobs) {
        sort(jobs.begin(), jobs.end());

        float waitTime = 0;  // Stores cumulative waiting time
        int totalTime = 0;   // Tracks elapsed execution time
        int n = jobs.size(); // Number of jobs

        for (int i = 0; i < n; i++) {
            waitTime += totalTime;  // Add current total time to waiting time
            totalTime += jobs[i];   // Execute current job
        }

        // Return the average waiting time
        return waitTime / n;
    }

### ***Program for Least Recently Used (LRU) Page Replacement Algorithm***

- Problem Statement: Design a data structure that follows the constraints of a Least Recently Used (LRU) cache. Implement the LRUCache class:
    - LRUCache(int capacity): Initialize the LRU cache with positive size capacity.
    - int get(int key): Return the value of the key if the key exists, otherwise return -1.
    - void put(int key, int value): Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.
    - The functions get and put must each run in O(1) average time complexity.

- The intuition behind an LRU (Least Recently Used) Cache is that we want to store only a fixed number of items in memory and quickly evict the item that hasn’t been used for the longest time. This is useful when memory is limited and we want to keep the most relevant data available for fast retrieval. The key idea is to maintain quick lookups to check if a value exists in the cache, and also maintain the usage order so we can remove the least recently used item efficiently when the cache is full.

- To implement it efficiently, we combine two data structures: a HashMap for O(1) lookup of keys, and a Doubly Linked List to maintain the order of usage. The most recently used items are kept at one end (head), and the least recently used items at the other end (tail). When we access or insert a key, we move it to the head whereas when the cache is full, we remove the tail node. This combination ensures both O(1) access and O(1) insertion/deletion for LRU operations.

- ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    class LRUCache {
    public:
        class Node {
        public:
            int key; int val; Node* next; Node* prev;
            Node(int _key, int _val) { key = _key; val = _val; }
        };

        int cap;
        Node* head = new Node(-1, -1);
        Node* tail = new Node(-1, -1);
        unordered_map<int, Node*> m; // key-node mapping
        LRUCache(int capacity) { cap = capacity; head->next = tail; tail->prev = head; }

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

        int get(int key_) { // Function to get value from cache
            if (m.find(key_) != m.end()) {
                Node* resNode = m[key_];
                int res = resNode->val;
                m.erase(key_); // Remove old mapping
                deleteNode(resNode); // Move accessed node to front
                addNode(resNode);
                m[key_] = head->next; // Update map
                return res;
            }
            return -1; // If not found
        }

        void put(int key_, int value) { // Function to put key-value into cache
            if (m.find(key_) != m.end()) { // If key already exists
                Node* existingNode = m[key_];
                m.erase(key_);
                deleteNode(existingNode);
            }
            if (m.size() == cap) { // If capacity reached
                m.erase(tail->prev->key);
                deleteNode(tail->prev);
            }
            addNode(new Node(key_, value)); // Insert new node at front
            m[key_] = head->next;
        }
    };

    int main() {
        LRUCache cache(2);
        cache.put(1, 1);
        cache.put(2, 2);
        cout << cache.get(1) << endl; // Get value for key 1
        cache.put(3, 3); // Insert another key (evicts key 2)
        cout << cache.get(2) << endl; // Key 2 should be evicted
        cache.put(4, 4); // Insert another key (evicts key 1)
        cout << cache.get(1) << endl; // Key 1 should be evicted
        cout << cache.get(3) << endl; // Key 3 should be present
        cout << cache.get(4) << endl; // Key 4 should be present
        return 0;
    }
    ```

### ***Insert Interval (Only Question)***

- Given a 2D array Intervals, where Intervals[i] = [start[i], end[i]] represents the start and end of the ith interval, the array represents non-overlapping intervals sorted in ascending order by start[i]. Given another array newInterval, where newInterval = [start, end] represents the start and end of another interval, merge newInterval into Intervals such that Intervals remain non-overlapping and sorted in ascending order by start[i]. Return Intervals after the insertion of newInterval.

- Example 1: Input : Intervals = [ [1, 3] , [6, 9] ] , newInterval = [2, 5]. Output : [ [1, 5] , [6, 9] ]. Explanation : After inserting the newInterval the Intervals array becomes [ [1, 3] , [2, 5] , [6, 9] ]. So to make them non overlapping we can merge the intervals [1, 3] and [2, 5]. So the Intervals array is [ [1, 5] , [6, 9] ].

- Example 2: Input : Intervals = [ [1, 2] , [3, 5] , [6, 7] , [8,10] ] , newInterval = [4, 8]. Output : [ [1, 2] , [3, 10] ]. Explanation: The Intervals array after inserting newInterval is [ [1, 2] , [3, 5] , [4, 8] , [6, 7] , [8, 10] ]. We merge the required intervals to make it non overlapping. So final array is [ [1, 2] , [3, 10] ].

- Brute Force Approach: (GPT Generated)
- ```cpp
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        // Insert the new interval and then sort all intervals
        intervals.push_back(newInterval);
        sort(intervals.begin(), intervals.end());

        // Merge overlapping intervals (same as merge overlapping intervals problem)
        vector<vector<int>> result;
        for (auto interval : intervals) {
            // If result is empty or current interval doesn't overlap with last interval in result
            if (result.empty() || result.back()[1] < interval[0]) {
                result.push_back(interval);
            } else {
                // Merge by extending the end of the last interval
                result.back()[1] = max(result.back()[1], interval[1]);
            }
        }
        return result;
    }
    ```

- Optimal Approach: (GPT Generated)
- ```cpp
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> result;
        int newStart = newInterval[0];
        int newEnd = newInterval[1];

        // Add all intervals that end before the new interval starts
        for (auto interval : intervals) {
            if (interval[1] < newStart) { result.push_back(interval); }
            
            else if (interval[0] <= newEnd) { // Merge intervals that overlap with the new interval
                newStart = min(newStart, interval[0]);
                newEnd = max(newEnd, interval[1]);
            }
            
            else { // Add intervals that start after the new interval ends
                result.push_back(newInterval);
                newInterval = interval;
                newStart = interval[0];
                newEnd = interval[1];
            }
        }

        result.push_back(newInterval);
        return result;
    }
    ```

### ***Merge Overlapping Sub-intervals***

- Problem Statement: Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals and return an array of the non-overlapping intervals that cover all the intervals in the input.

- Input : intervals=[[1,3],[2,6],[8,10],[15,18]]. Output : [[1,6],[8,10],[15,18]]. Explanation : Since intervals [1,3] and [2,6] are overlapping we can merge them to form [1,6] intervals.

- Input : [[1,4],[4,5]]. Output :  [[1,5]]. Explanation :  Since intervals [1,4] and [4,5] are overlapping we can merge them to form [1,5].


- Brute: O(N^2)
- ```cpp
    vector<vector<int>> merge(vector<vector<int>>& intervals) {

        // Sort intervals based on start time
        sort(intervals.begin(), intervals.end());

        // Result array to store merged intervals
        vector<vector<int>> ans;

        // Loop through each interval
        int n = intervals.size();
        for (int i = 0; i < n; ) {

            // Start of current merged interval
            int start = intervals[i][0];
            int end = intervals[i][1];

            // Merge with all overlapping intervals
            int j = i + 1;
            while (j < n && intervals[j][0] <= end) {
                // Update end to the maximum of current end and overlapping interval's end
                end = max(end, intervals[j][1]);
                j++;
            }

            // Add the merged interval to result
            ans.push_back({start, end});

            // Move to the next non-overlapping interval
            i = j;
        }

        return ans;
    }
    ```

- Optimal: O(N * logN) + O(N)
- ```cpp
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        
        sort(intervals.begin(), intervals.end()); // Sort intervals based on starting time
        vector<vector<int>> merged;

        for (auto interval : intervals) {
            if (merged.empty() || merged.back()[1] < interval[0]) { merged.push_back(interval); } // If merged is empty or current interval does not overlap Add current interval as a new non-overlapping block
            else { merged.back()[1] = max(merged.back()[1], interval[1]); } // Overlapping: merge by extending the end time
        }

        return merged;
    }
    ```

### ***Non-overlapping Intervals***

- Problem Statement: Given an array of N intervals in the form of (start[i], end[i]), where start[i] is the starting point of the interval and end[i] is the ending point of the interval, return the minimum number of intervals that need to be removed to make the remaining intervals non-overlapping.

- Input: Intervals = [ [1, 2], [2, 3], [3, 4], [1, 3] ], Output: 1, Explanation: You can remove the interval [1, 3] to make the remaining intervals non-overlapping.

- Input: Intervals = [ [1, 3], [1, 4], [3, 5], [3, 4], [4, 5] ], Output: 2, Explanation: You can remove the intervals [1, 4] and [3, 5] to make the rest non-overlapping.

- Brute: O(2^N × N log N)
- ```cpp
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {

        int n = intervals.size();
        int maxValid = 0; // Store the max size of non-overlapping subset

        for (int mask = 0; mask < (1 << n); ++mask) { // Try all subsets using bitmasking

            // Vector to hold selected subset
            vector<vector<int>> subset;

            // Construct subset from bitmask
            for (int i = 0; i < n; ++i) {
                if (mask & (1 << i))
                    subset.push_back(intervals[i]);
            }

            // Sort subset by start time
            sort(subset.begin(), subset.end());

            // Check if it's non-overlapping
            bool valid = true;
            for (int i = 1; i < subset.size(); ++i) {
                if (subset[i][0] < subset[i-1][1]) {
                    valid = false;
                    break;
                }
            }

            // If valid, update maxValid size
            if (valid)
                maxValid = max(maxValid, (int)subset.size());
        }

        // Answer = total - size of max valid subset
        return n - maxValid;
    }
    ```

- Optimal: O(N * logN)
- ```cpp
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {

        sort(intervals.begin(), intervals.end(), [](auto& a, auto& b) {return a[1] < b[1];}); // Sort intervals based on their end time (greedy strategy)
        int count = 0; int prevEnd = intervals[0][1];

        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] < prevEnd) { count++; } // Overlapping, increase removal count
            else { prevEnd = intervals[i][1]; } // No overlap, update the end of last accepted interval  
        }

        return count;
    }
    ```

    