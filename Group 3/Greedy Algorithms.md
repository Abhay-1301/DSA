# Greedy Algorithms

- ### Assign Cookies
    - abc

- ### Fractional Knapsack
    - The weight of N items and their corresponding values are given. We have to put these items in a knapsack of weight W such that the total value obtained is maximized. We can either take the item as a whole or break it into smaller units.
        - val = [60, 100, 120], wt = [10, 20, 30], capacity = 50 Output: 240.000000
            - per_wt_val = [6, 5, 4], 6 * 10 + 20 * 5 + 4* 20 = 60+100+80 = 240
        - val = [60, 100], wt = [10, 20], capacity = 50 Output: 160.000000
    - By looking at input and output, I figured out that sort descending by per weight value and then greedily pick based on how much weight is left.
    - Algorithm:
        - Do the coding.

- ### Lemonade Change
    - Given an array representing a queue of customers and the value of bills they hold, determine if it is possible to provide correct change to each customer. Customers can only pay with 5$, 10$ or 20$ bills and we initially do not have any change at hand. Return true, if it is possible to provide correct change for each customer otherwise return false.
    - bills = [5, 5, 5, 10, 20] -> True ; bills = [5, 5, 10, 10, 20] -> false. One input is missing here that each customer has to 5 dollar bill only. 
        - We sort it. We maintain a map of 5,10,20 bill count with us. For each item in an array, if it is 5, do map[5]++, if its 10, check if map[5] is more than 0, if yes do map[5]-- and do map[10]++, if its 20, check if map[10] and map[5] both are greater than 0, if yes do map[5]-- and map[10]-- if no then check if map[5] is >= 3 then do map[5] = map[5]-3 and move ahead. if. we reach to the end of array return true and at any point of time where we are decreasing map value and we do not have enough than false.
    -  I thought it correctly. Also treat it as queue not array and proceed linearly, no need of sorting. Do coding.

- ### Valid Paranthesis Checker
    - Find the validity of an input string s that only contains the letters '(', ')' and '*'. A string entered is legitimate if (1) Any left parenthesis '(' must have a corresponding right parenthesis ')'. (2) right parenthesis ')' must have a corresponding left parenthesis '('. (3) Left parenthesis '(' must go before the corresponding right parenthesis ')'. (4) * could be treated as a single right parenthesis ')' or a single left parenthesis '(' or an empty string "".
    - s = (*)) -> true ; s = *(() -> false
    - Stack comes in my mind. But you have only few type of brackets so it can be solved in any other way. 
    - Brute force approach is given that treat * as all three possible type of things and check for each type if it is valid. We can just check this based on counts +1 for '(' and -1 for ')'. If count go negative then false if postive till end false. true if zero.
    - See the code it is 3^n complexity. Not a simple loop problem. Look at implementation. 
    - What is optimal approach using greedy ?


- ### N meetings in one room
    - There is one meeting room in a firm. You are given two arrays, start and end each of size N. For an index ‘i’, start[i] denotes the starting time of the ith meeting while end[i] will denote the ending time of the ith meeting. Find the maximum number of meetings that can be accommodated if only one meeting can happen in the room at a particular time. Print the order in which these meetings will be performed.
    - Input: N = 6,  start[] = {1,3,0,5,8,5}, end[] =  {2,4,5,7,9,9} Output: [1, 2, 4, 5]
        - partition = {(1,2),(3,4),(0,5),(5,7),(8,9),(5,9)}. Sort by end time. Check if end time of prev is less than or equal start time of current. If yes that meeting can happen.
    - Input: N = 2, start[] = {1,5}, end[] = {7,8} Output: [1]
    - Algorithm:
        ```cpp
        vector<int> maxMeetings(vector<int>& start, vector<int>& end){
            vector<tuple<int, int, int>> meetings;
            for (int i = 0; i < start.size(); i++) meetings.push_back({end[i], start[i], i + 1});
            sort(meetings.begin(), meetings.end());
            vector<int> result; int lastEnd = -1;

            for (auto& m : meetings){
                int e = get<0>(m); int s = get<1>(m); int idx = get<2>(m);
                if (s > lastEnd) result.push_back(idx); lastEnd = e;}
            return result;
        }
        ```

- ### Jump Game - I
    - Given an array where each element represents the maximum number of steps you can jump forward from that element, return true if we can reach the last index starting from the first index. Otherwise, return false.
        - Input:nums = [2, 3, 1, 0, 4] Output: True
        - Input:nums = [3, 2, 1, 0, 4] Output: False
    - The problem here I am seeing is zero. If no zero then you can reach. No need of even counting the total sum of array. 
    - If there is zero, our goal is to avoid that. How to check if I can avoid that or not.
    - Algorithm:
        ```cpp
        bool canJump(vector<int>& nums){
            int maxIndex = 0;
            for (int i = 0; i < nums.size(); i++){
                if (i > maxIndex) return false;
                maxIndex = max(maxIndex, i + nums[i]);}
            return true;}
        ```

- ### Jump Game - II
    - You are given a 0-indexed array nums of length n representing your maximum jump capability from each index. You start at index 0. Each element nums[i] represents the maximum number of steps you can jump forward from index i. Your goal is to reach the last index of the array (nums[n - 1]) using the minimum number of jumps. Return the minimum number of jumps required to reach the last index. You can assume that it is always possible to reach the last index.
    - Input: nums = [2, 3, 1, 1, 4] Output: 2
    - Input: nums = [2, 3, 0, 1, 4] Output: 2
    - ans = [0, 0, 0, 0, 0] -> [0, 1, 1, 0, 0] -> [0, 1, 1, 2, 2]

- ### Job Sequencing Problem
    - You are given a set of N jobs where each job comes with a deadline and profit. The profit can only be earned upon completing the job within its deadline. Find the number of jobs done and the maximum profit that can be obtained. Each job takes a single unit of time and only one job can be performed at a time.
    - N = 4, Jobs = {(1, 4, 20), (2, 1, 10), (3, 1, 40), (4, 1, 30)} Output  2 60. The 3rd job with a deadline of 1 is performed during the first unit of time. The 1st job is performed during the second unit of time as its deadline is 4. Profit = 40 + 20 = 60. So, the result is 2 jobs with a total profit of 60.
        - (2, 1, 10), (3, 1, 40), (4, 1, 30), (1, 4, 20)
    - N = 5, Jobs = {(1, 2, 100), (2, 1, 19), (3, 2, 27), (4, 1, 25), (5, 1, 15)} Output 2 127. The first and third jobs, both having a deadline of 2, give the highest profit. Profit = 100 + 27 = 127. So, the result is 2 jobs with a total profit of 127.
        - (4, 1, 25), (1, 2, 100), (3, 2, 27) NOOOOOOO
        - [(1, 2, 100), (2, 1, 19), (3, 2, 27), (4, 1, 25), (5, 1, 15)] ---Sort by profit---> [(1, 2, 100), (3, 2, 27), (4, 1, 25), (2, 1, 19), (5, 1, 15)]
        - [(1, 4, 20), (2, 1, 10), (3, 1, 40), (4, 1, 30)] ---Sort by profit---> [(3, 1, 40), (4, 1, 30), (1, 4, 20), (2, 1, 10)]
    - My thinking is wrong. Have to rethink. Second rethinking is maybe correct. sort by profit. Use a counter to track time.
    - Algorithm:
        ```cpp
        struct Job { int id; int dead; int profit;};
        bool static comparison(Job a, Job b) {return (a.profit > b.profit);} // Sort by profit in descending order

        pair < int, int > JobScheduling(Job arr[], int n){
            sort(arr, arr + n, comparison);
            int maxi = arr[0].dead;
            for (int i = 1; i < n; i++) maxi = max(maxi, arr[i].dead);
            int slot[maxi + 1];
            for (int i = 0; i <= maxi; i++) slot[i] = -1;
            int countJobs = 0, jobProfit = 0;

            for (int i = 0; i < n; i++){
                for (int j = arr[i].dead; j > 0; j--){
                    if (slot[j] == -1){
                        slot[j] = i; countJobs++; jobProfit += arr[i].profit; break;
                    }
                }
            }
            return make_pair(countJobs, jobProfit);
        }
        ```

- ### Shortest Job First (or SJF) CPU Scheduling
    - Given a list of job durations representing the time it takes to complete each job. Implement the Shortest Job First algorithm to find the average waiting time for these jobs.
        - Input:jobs = [3, 1, 4, 2, 5] Output: 4
        - Input: jobs = [4, 3, 7, 1, 2] Output: 4
            - [1, 2, 3, 4, 7] -> [0, 1, 1+2, 1+2+3, 1+2+3+4] -> 1 * 4 + 2 * 3 + 3 * 2 + 4 * 1 / 5
    - Simple implementation if you look at input and output.
    - Algorithm:
        ```cpp
        float calculateAverageWaitTime(vector<int>& jobs){
            sort(jobs.begin(), jobs.end());
            float waitTime = 0; int totalTime = 0; int n = jobs.size();
            for (int i = 0; i < n; i++) waitTime += totalTime; totalTime += jobs[i];
            return waitTime / n;}
        ```

- ### AA


