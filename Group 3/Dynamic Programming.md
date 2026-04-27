# ***Dynamic Programming***

- Dynamic Programming can be described as storing answers to various sub-problems to be used later whenever required to solve the main problem.
- The two common dynamic programming approaches are:
    - Memoization: Known as the “top-down” dynamic programming, usually the problem is solved in the direction of the main problem to the base cases.
    - Tabulation: Known as the "bottom-up" dynamic programming, usually the problem is solved in the direction of solving the base cases to the main problem.
- Always think of recursion, iteration, and if space can be optimized.
- Always try to think what dp[i][j] means.

## Part 1: 1D DP

## Part 2: 2D/3D DP

## ***Part 3: DP on Subsequences***

### ***Subset sum equal to target***

- We are given an array 'ARR' with N positive integers. We need to find if there is a subset in "ARR" with a sum equal to K. If there is, return true else return false.

- Take not take approach came in my mind and recursive only came in my mind.

- ```cpp
    bool subsetSumToK(int n, int k, vector<int>& arr){
        vector<vector<int>> dp(n, vector<int>(k + 1, -1));
        return subsetSumUtil(n - 1, k, arr, dp);
    }

    bool subsetSumUtil(int ind, int target, vector<int>& arr, vector<vector<int>>& dp){
        if (target == 0) return true;
        if (ind == 0) return arr[0] == target;
        if (dp[ind][target] != -1) return dp[ind][target];

        bool notTaken = subsetSumUtil(ind - 1, target, arr, dp);
        bool taken = false;
        if (arr[ind] <= target) taken = subsetSumUtil(ind - 1, target - arr[ind], arr, dp);
        return dp[ind][target] = notTaken || taken;
    }
    ```

- What I just did is hardcoding logic in my mind which will not work if I see a new problem solvable by DP. So lets think again in terms of DP only. What that dp array is.

- DP array is nothing but f(ind,target) = 0 or 1 which is a 2D table. It is of n rows and k+1 column.

- dp[i][j] answers this question: "Using only the first i elements (indices 0 to i-1), can we form a subset with sum exactly equal to j?"

- What is f(ind, target)? This is the recursive formulation: "Starting at index ind, can we achieve remaining sum target using elements from ind to n-1?"

- ```cpp
    bool subsetSumToK(int n, int k, vector<int> &arr){
        vector<vector<bool>> dp(n, vector<bool>(k + 1, false));
        for (int i = 0; i < n; i++) dp[i][0] = true;
        if (arr[0] <= k) dp[0][arr[0]] = true;

        for (int ind = 1; ind < n; ind++){
            for (int target = 1; target <= k; target++){
                bool notTaken = dp[ind - 1][target];
                bool taken = false;
                if (arr[ind] <= target) taken = dp[ind - 1][target - arr[ind]];
                dp[ind][target] = notTaken || taken;
            }
        }
        return dp[n - 1][k];
    }
    ```

- ```cpp
    bool subsetSumToK(int n, int k, vector<int> &arr){
        vector<bool> prev(k + 1, false);
        prev[0] = true;
        if (arr[0] <= k) prev[arr[0]] = true;

        for (int ind = 1; ind < n; ind++){
            vector<bool> cur(k + 1, false);
            cur[0] = true;
            for (int target = 1; target <= k; target++){
                bool notTaken = prev[target];
                bool taken = false;
                if (arr[ind] <= target) taken = prev[target - arr[ind]];
                cur[target] = notTaken || taken;
            }
            prev = cur;
        }

        return prev[k];
    }
    ```

- I think you do not need to memorize the code here. If you can visualize and have intution for tabulation and 2D dp matrix iteratively, I trust my coding skill to remove that need of 2D matrix and keep one array only. But yeah memorizing the code would help in faster implementation.

### ***Partition equal subset sum***

- Given an array arr of n integers, return true if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal else return false.

- Similar as above. K = half of array sum.

### ***Partition Set Into 2 Subsets With Min Absolute Sum Diff***

- Given an array of n integers, partition the array into two subsets such that the absolute difference between their sums is minimized.

- Think about this problem again. It is simple only based on subset sum equal to K. But you have to know what does dp[i][j] array stores. This will make it easy.

- We want to divide the array into two subsets and return the smallest possible absolute difference between their sums.

-  ```cpp
    int minSubsetSumDifference(vector<int>& arr, int n){
        int totSum = 0;
        for (int i = 0; i < n; i++) totSum += arr[i];
        vector<vector<int>> dp(n, vector<int>(totSum + 1, -1));

        for (int i = 0; i <= totSum; i++) bool dummy = subsetSumUtil(n - 1, i, arr, dp);
        int mini = 1e9;
        for (int i = 0; i <= totSum; i++)
            if (dp[n - 1][i] == true) int diff = abs(i - (totSum - i)); mini = min(mini, diff);
        return mini;
    }

    bool subsetSumUtil(int ind, int target, vector<int>& arr, vector<vector<int>>& dp){
        if (target == 0) return dp[ind][target] = true;
        if (ind == 0) return dp[ind][target] = (arr[0] == target);
        if (dp[ind][target] != -1) return dp[ind][target];

        bool notTaken = subsetSumUtil(ind - 1, target, arr, dp);
        bool taken = false;
        if (arr[ind] <= target) taken = subsetSumUtil(ind - 1, target - arr[ind], arr, dp);

        return dp[ind][target] = notTaken || taken;
    }
    ```

- This is doing nothing but checking the last row of our dp array. Total column = Total Sum of array. So if any value is true, it shows that value of sum is possible. So just find the total difference value abs(i - (totSum - i)) and compare with a counter mini. Done.

### ***Count Subsets with Sum K***

- Given an array arr of n integers and an integer K, count the number of subsets of the given array that have a sum equal to K.

-  ```cpp
    int countSubsets(vector<int>& nums, int target) {
        vector<vector<int>> dp(nums.size(), vector<int>(target + 1, -1));
        return solve(nums.size() - 1, target, nums, dp);
    }

    int solve(int index, int target, vector<int>& nums, vector<vector<int>>& dp) {
        if (target == 0) return 1;
        if (index == 0) return (nums[0] == target ? 1 : 0);
        if (dp[index][target] != -1) return dp[index][target];

        int notTake = solve(index - 1, target, nums, dp);
        int take = 0;
        if (nums[index] <= target) take = solve(index - 1, target - nums[index], nums, dp);

        return dp[index][target] = take + notTake;
    }
    ```

- Here, instead of || operator, + operator is used. Function value returned is int and not bool.

- Basically the dp[i][j] value tells us how many counts are possible.

### ***Count Partitions with Given Difference***

- Given an array with N positive integers and an integer D, count the number of ways we can partition the given array into two subsets, S1 and S2 such that S1 - S2 = D and S1 is always greater than or equal to S2.

- Count subset sum with sum K.

- S1 - S2 = d ; S1 + S2 = totalSum ; S1 = (d + totalSum) / 2 ; Count subsets of the array whose sum is (d + totalSum)/2

- If (d + totalSum) is odd, then division by 2 is not possible, thus, there is no valid partitions. If d is greater than totalSum, then it is also impossible, thus, there is no valid partitions.

### ***Assign Cookies***

- Consider a scenario where a teacher wants to distribute cookies to students, with each student receiving at most one cookie. Given two arrays, student and cookie, the ith value in the student array describes the minimum size of cookie that the ith student can be assigned. The jth value in the cookie array represents the size of the jth cookie. If cookie[j] >= student[i], the jth cookie can be assigned to the ith student. Maximize the number of students assigned with cookies and output the maximum number.

- Input : Student = [1, 2, 3] , Cookie = [1, 1] , Output :1 , Explanation : Only the first cookie (1) satisfies the first student (1), therefore only 1 student is content.

- Input : Student = [1, 2] , Cookie = [1, 2, 3] , Output : 2 , Explanation : Cookie 1 satisfies student 1 and cookie 2 satisfies student 2. Therefore, 2 students are content.

- First of all here assume that this question can be solved by DP because it say find max out of all options. So here we have to find what that dp[i][j] array will signify. Here one dimension is student and other dimension is cookie. So the question can be reframed as, in how many max ways..... ok wait wait if I can not even solve it via brute force approach why even thinking in terms of DP.

- So what is brute force ? Max student can be satisfied is student.size(). Maybe sort it first and use two pointers ? I was thinking in the right direction. But in real world scenario, I would have been confused.

- Not able to solve by thinking brute force so here is hint. We use recursion to try all ways of assigning cookies to children. At each step, if the current cookie can satisfy the current child, we try both: assigning it or skipping it, and take the better result. If it can't satisfy, we skip. To avoid recalculating the same subproblems, we store results in a 2D dp[child][cookie] array. This saves time by reusing already computed answers.

- Ok ok ok. Based on hint. Think like a take not-take approach.

- I am still not very confident if I can code or not. I can if I have memorized the take not take format. With the operator max here.

-  ```cpp
    int findContentChildren(vector<int>& student, vector<int>& cookie){
        sort(student.begin(), student.end());
        sort(cookie.begin(), cookie.end());
        vector<vector<int>> memo(student.size(), vector<int>(cookie.size(), -1));
        return helper(0, 0, student, cookie, memo);
    }

    int helper(int studentIndex, int cookieIndex, vector<int>& student, vector<int>& cookie, vector<vector<int>>& memo){
        if (studentIndex >= student.size() || cookieIndex >= cookie.size()) return 0;
        if (memo[studentIndex][cookieIndex] != -1) return memo[studentIndex][cookieIndex];

        int result = 0;
        if (cookie[cookieIndex] >= student[studentIndex]) result = max(result, 1 + helper(studentIndex + 1, cookieIndex + 1, student, cookie, memo));
        result = max(result, helper(studentIndex, cookieIndex + 1, student, cookie, memo));
        return memo[studentIndex][cookieIndex] = result;
    }
    ```

- Seeing the code, apply this dp if you can think of problem statement like yes or not for a given index. Like does my answer contain this index yes or no.

- Format is same, coding won't be problem but when and how to map your problem to this format, you have to think deeply.

### ***Minimum Coins***

- Given an integer array of coins representing coins of different denominations and an integer amount representing a total amount of money. Return the fewest number of coins that are needed to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1. There are infinite numbers of coins of each type.

- Easily thinkable as take not take ? You have a total amount. You either take a coin at a given index or not take a coin. If you not take a coin then decrease index by 1 but no decrease in amount. If you take the coin, check if you are eligible to take like amount > current index value of array. if yes then subtract the total money BUT do not subtract the index, you can take that index again as it has infinite value. And when you take, do 1 + recursive function. At last return min of take and not take. Lets see algo.

-  ```cpp
    int coinChange(vector<int>& coins, int amount){
        vector<int> dp(amount + 1, -2);
        return helper(coins, amount, dp);
    }

    int helper(vector<int>& coins, int rem, vector<int>& dp){
        if (rem == 0) return 0;
        if (rem < 0) return -1;
        if (dp[rem] != -2) return dp[rem];
        int mini = INT_MAX;

        for (int coin : coins){
            int res = helper(coins, rem - coin, dp);
            if (res >= 0 && res < mini) mini = 1 + res;
        }

        dp[rem] = (mini == INT_MAX) ? -1 : mini;
        return dp[rem];
    }
    ```

- My thinking and this solution both are correct. My thinking is 2D DP. This solution is 1D DP. Both reach the same answer, but 1D is more space-efficient for unbounded problems.

- Key insight: Since coins are infinite and interchangeable, we don't need to track "which coin index are we at". At any amount, we can try any coin — the order doesn't matter.

### ***Target Sum***

- We are given an array 'ARR' of size 'N' and a number 'Target'. Our task is to build an expression from the given array where we can place a '+' or '-' sign in front of an integer. We want to place a sign in front of every integer of the array and get our required target. We need to count the number of ways in which we can achieve our required target.

- We have two option either add or subtract the given number while iterating through array. So iterate through array and first add the number to total sum initialized with 0 and move ahead and then subtract from total sum and move ahead. count both ways.

-  ```cpp
    int findTargetSumWays(vector<int>& nums, int target) {
        int totalSum = accumulate(nums.begin(), nums.end(), 0);
        if ((totalSum - target) < 0 || (totalSum - target) % 2 != 0) return 0;
        int subsetSum = (totalSum - target) / 2;
        vector<vector<int>> dp(nums.size(), vector<int>(subsetSum + 1, -1));
        return countSubsets(nums, nums.size() - 1, subsetSum, dp);
    }

    int countSubsets(vector<int>& nums, int ind, int target, vector<vector<int>>& dp){
        if (ind == 0) {
            if (target == 0 && nums[0] == 0) return 2;
            if (target == 0 || target == nums[0]) return 1;
            return 0;
        }

        if (dp[ind][target] != -1) return dp[ind][target];

        int notPick = countSubsets(nums, ind - 1, target, dp);
        int pick = 0;
        if (nums[ind] <= target) pick = countSubsets(nums, ind - 1, target - nums[ind], dp);

        return dp[ind][target] = pick + notPick;
    }
    ```

- Problem with my thinking: currentSum can be negative → can't use simple 2D array, need map or offset. Reduced problem: Count subsets with sum = (totalSum - target) / 2.

- ```
    Let S1 = sum of elements with '+'
    Let S2 = sum of elements with '-'

    S1 - S2 = target        (given)
    S1 + S2 = totalSum      (all elements)

    Adding: 2*S1 = target + totalSum  →  S1 = (target + totalSum) / 2
    OR
    Subtracting: 2*S2 = totalSum - target  →  S2 = (totalSum - target) / 2
    ```

### ***Coin Change 2***

- We are given an array Arr with N distinct coins and a target. We have an infinite supply of each coin denomination. We need to find the number of ways we sum up the coin values to give us the target.

- Isn't this same as coin change 1 but here we have to use + operator instead of min. ?

- ```cpp
    long countWaysToMakeChangeUtil(vector<int>& arr, int ind, int T, vector<vector<long>>& dp){
        if (ind == 0) return (T % arr[0] == 0);
        if (dp[ind][T] != -1) return dp[ind][T];

        long notTaken = countWaysToMakeChangeUtil(arr, ind - 1, T, dp);
        long taken = 0;
        if (arr[ind] <= T) taken = countWaysToMakeChangeUtil(arr, ind, T - arr[ind], dp);
        return dp[ind][T] = notTaken + taken;
    }

    long countWaysToMakeChange(vector<int>& arr, int n, int T) {
        vector<vector<long>> dp(n, vector<long>(T + 1, -1));
        return countWaysToMakeChangeUtil(arr, n - 1, T, dp);
    }
    ```

- Yes it is same, although think of base cases, these are changing as per changing question.

### ***Unbounded Knapsack***

- A thief wants to rob a store. He is carrying a bag of capacity W. The store has 'n' items of infinite supply. Its weight is given by the 'wt' array and its value by the 'val' array. He can either include an item in its knapsack or exclude it but can't partially have it as a fraction. We need to find the maximum value of items that the thief can steal. He can take a single item any number of times he wants and put it in his knapsack.

- Same as min coins my thought process ? If taken do not decrease index but decrease only weight. If not taken decrease only index. return using max operator.

-  ```cpp
    int knapsackUtil(vector<int>& wt, vector<int>& val, int ind, int W, vector<vector<int>>& dp){
        if (ind == 0) return (W / wt[0]) * val[0];
        if (dp[ind][W] != -1) return dp[ind][W];

        int notTaken = knapsackUtil(wt, val, ind - 1, W, dp);
        int taken = INT_MIN;
        if (wt[ind] <= W) taken = val[ind] + knapsackUtil(wt, val, ind, W - wt[ind], dp);
        return dp[ind][W] = max(notTaken, taken);
    }

    int unboundedKnapsack(int n, int W, vector<int>& val, vector<int>& wt){
        vector<vector<int>> dp(n, vector<int>(W + 1, -1));
        return knapsackUtil(wt, val, n - 1, W, dp);
    }
    ```

### ***Rod Cutting Problem***

- Given a rod of length N inches and an array price[] where price[i] denotes the value of a piece of rod of length i inches (1-based indexing). Determine the maximum value obtainable by cutting up the rod and selling the pieces. Make any number of cuts, or none at all, and sell the resulting pieces.

- Simple solution I can think of cut and notCut. If cut price at that index + recursive for index-1. If not cut recursive for index-1. At last max of cut and not cut.

-  ```cpp
    int rodCutting(vector<int> price, int n) {
        vector<vector<int>> dp(n, vector<int>(n + 1, 0));
        for(int length = 0; length <= n; length++) dp[0][length] = length * price[0];

        for(int i = 1; i < n; i++){
            for(int length = 0; length <= n; length++){
                int notTake = dp[i - 1][length];
                int take = INT_MIN;
                int rodLength = i + 1;
                if(rodLength <= length) take = price[i] + dp[i][length - rodLength];
                dp[i][length] = max(take, notTake);
            }
        }
        return dp[n - 1][n];
    }
    ```

- Kind of different implementation. Wait, It is same only. It's not there on Striver's code properly I guess.


## ***Part 4: DP on Strings***

### ***Longest Common Subsequence***

- Given two strings str1 and str2, find the length of their longest common subsequence. A subsequence is a sequence that appears in the same relative order but not necessarily contiguous and a common subsequence of two strings is a subsequence that is common to both strings.

- It is also based on take not take at each char of the string. At last you find max of take and not take. If there is match of character you do +1, otherwise take non take options.

- ```cpp
    int lcs(string str1, string str2){
        int n = str1.size();
        int m = str2.size();
        vector<vector<int>> dp(n, vector<int>(m, -1));
        return func(str1, str2, n - 1, m - 1, dp);}

    int func(string& s1, string& s2, int ind1, int ind2, vector<vector<int>>& dp){
        if (ind1 < 0 || ind2 < 0) return 0;
        if (dp[ind1][ind2] != -1) return dp[ind1][ind2];
        if (s1[ind1] == s2[ind2]) return dp[ind1][ind2] = 1 + func(s1, s2, ind1 - 1, ind2 - 1, dp);
        else {return dp[ind1][ind2] = max(func(s1, s2, ind1, ind2 - 1, dp), func(s1, s2, ind1 - 1, ind2, dp))};}
    ```

- DP table for abcde and bdqek
    ```txt
    x b d q e k
    a 0 0 0 0 0
    b 1 1 1 1 1
    c 1 1 1 1 1
    d 1 2 2 2 2
    e 1 2 2 3 3
    ```

### ***Print Longest Common Subsequence***

- Given two strings str1 and str2, print the longest common subsequence of the two strings.
- We populate the array like above. But how to print using dp array values ? 
- I think we just iterate last row and see if the value is increased at any index, if the value is increased at any index means at that index whatever character is, it is in LCS, so include it from the string. NOOOOOO, we do not traverse last row, we traverse like below.
- ```cpp
    int i = n, j = m;
    string lcs = "";

    // Traverse dp table from bottom-right to top-left
    while (i > 0 && j > 0) {
        if (text1[i - 1] == text2[j - 1]) lcs += text1[i - 1]; i--; j--;
        else if (dp[i - 1][j] > dp[i][j - 1]) i--;
        else j--;
        }

    reverse(lcs.begin(), lcs.end());
    return lcs;}
    ```

### ***Longest Common Substring***

- Given two strings str1 and str2, find the length of their longest common substring. A substring is a contiguous sequence of characters within a string.
- Same as above but we are removing that max part that we do like take and not take. So here just fill the dp table iteratively.
- ```cpp
    int longestCommonSubstr(string str1, string str2) {
        int n = str1.size(); int m = str2.size();
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
        int ans = 0;

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (str1[i - 1] == str2[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                    ans = max(ans, dp[i][j]);
                }
                else {
                    dp[i][j] = 0;
                }
            }
        }
        return ans;
    }
    ```

### ***Longest Palindromic Subsequence***

- Given a string, Find the longest palindromic subsequence length in given string. A palindrome is a sequence that reads the same backwards as forward.
- LPS(string) = LCS(string,reversed_string)
- ```cpp
    int func(string s1, string s2) {
        int n = s1.size(); int m = s2.size();
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, -1));

        for (int i = 0; i <= n; i++) dp[i][0] = 0;
        for (int i = 0; i <= m; i++) dp[0][i] = 0;

        for (int ind1 = 1; ind1 <= n; ind1++) {
            for (int ind2 = 1; ind2 <= m; ind2++) {
                if (s1[ind1 - 1] == s2[ind2 - 1]) dp[ind1][ind2] = 1 + dp[ind1 - 1][ind2 - 1];
                else dp[ind1][ind2] = max(dp[ind1 - 1][ind2], dp[ind1][ind2 - 1]);
            }
        }
        return dp[n][m];
    }

    int longestPalinSubseq(string s){
        string t = s;
        reverse(s.begin(), s.end());
        return lcs(s, t);
    }
    ```

### ***Minimum insertions to make string palindrome***

- Given a string s, find the minimum number of insertions needed to make it a palindrome. A palindrome is a sequence that reads the same backward as forward. You can insert characters at any position in the string.
- To make string abc palindromic insert cba at last like abccba. But do not add which is already there which is length of LPS. So our answer is len(string)minus len(LPS).

### ***Minimum Insertions/Deletions to Convert String***

- We are given two strings, str1 and str2. We are allowed the following operations. Delete any number of characters from string str1. Insert any number of characters in string str1. Return the minimum number of operations required to convert str1 to str2.

- Input:  str1 = "kitten", str2 = "sitting" Output: 5 Explanation: To transform "kitten" to "sitting", delete "k" and insert "s" to get "sitten", then delete 'e' and insert "i" to get "sittin", and insert "g" at the end to get "sitting". Think in terms of LCS. LCS is 'ittn'. So delete 'ke' and insert 'sig'. This makes our answer as (n-k)+(m-k). Here n and m are the length of str1 and str2 respectively and k is the length of the longest common subsequence of str1 and str2.

### ***Shortest Common Supersequence***

- We are given two strings ‘S1’ and ‘S2’. We need to return their shortest common supersequence. A supersequence is defined as the string which contains both the strings S1 and S2 as subsequences.

- Input : str1 = "mno", str2 = "nop", Output :"mnop". Explanation : The shortest common supersequence is "mnop". It contains "mno" as the first three characters and "nop" as the last three characters, thus including both strings as subsequences.

- Input :str1 = "dynamic", str2 = "program", Output : "dynprogramic". Explanation :The shortest common supersequence is "dynprogramic". It includes all characters from both "dynamic" and "program", with minimal overlap. For example, "dynamic" appears as "dyn...amic" and "program" appears as "...program..." within "dynprogramic".

- Concatenate (if no shortest) ? But LCS is same in both. So we don't add LCS. So Length of SCS will be 'n+m-k'.

- But how to find the actual supersequence ? Same as how we printed LCS.

- ```cpp
    string shortestSupersequence(string s1, string s2) {
        int n = s1.size(); int m = s2.size();
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
        for (int i = 0; i <= n; i++) dp[i][0] = 0;
        for (int i = 0; i <= m; i++) dp[0][i] = 0;


        for (int ind1 = 1; ind1 <= n; ind1++) {
            for (int ind2 = 1; ind2 <= m; ind2++) {
                if (s1[ind1 - 1] == s2[ind2 - 1]) dp[ind1][ind2] = 1 + dp[ind1 - 1][ind2 - 1];
                else dp[ind1][ind2] = max(dp[ind1 - 1][ind2], dp[ind1][ind2 - 1]);}}

        // Start from bottom-right of the DP table to build the supersequence
        int i = n; int j = m; string ans = "";

        while (i > 0 && j > 0) {
            // If characters are equal, include it once
            if (s1[i - 1] == s2[j - 1]) {
                ans += s1[i - 1];
                i--; j--;}

            // Move in the direction of greater value
            else if (dp[i - 1][j] > dp[i][j - 1]) {
                ans += s1[i - 1];
                i--;}
            else {
                ans += s2[j - 1];
                j--;}
        }

        // If any characters are left in s1, add them
        while (i > 0) {
            ans += s1[i - 1];
            i--;
        }

        // If any characters are left in s2, add them
        while (j > 0) {
            ans += s2[j - 1];
            j--;
        }

        reverse(ans.begin(), ans.end());
        return ans;}
    ```

### ***Distinct Subsequences***

- Given two strings s and t, return the number of distinct subsequences of s that equal t. The task is to count how many different ways we can form t from s by deleting some (or no) characters from s.

- Input: s = "axbxax", t = "axa". Output: 2. Explanation: In the string "axbxax", there are two distinct subsequences "axa":(a)(x)bx(a)x & (a)xb(x)(a)x.
- Input: s = "babgbag", t = "bag". Output: 5. Explanation: In the string "babgbag", there are five distinct subsequences "bag": (ba)(b)(ga)(g) & (ba)(bg)(ag) & (bab)(ga)(g) & (bab)(g)(ag) & (babg)(a)(g).

- ```cpp
    int helper(int i, int j, string &s, string &t, vector<vector<int>> &dp) {
        if (j == t.size()) return 1;
        if (i == s.size()) return 0;

        if (dp[i][j] != -1) return dp[i][j];

        if (s[i] == t[j]) {
            int take = helper(i + 1, j + 1, s, t, dp);
            int notTake = helper(i + 1, j, s, t, dp);
            return dp[i][j] = take + notTake;}
        else {
            return dp[i][j] = helper(i + 1, j, s, t, dp);}
    }

    int numDistinct(string s, string t) {
        vector<vector<int>> dp(s.size(), vector<int>(t.size(), -1));
        return helper(0, 0, s, t, dp);}
    ```

- ```cpp
    int numDistinct(string s, string t) {
        int n = s.size(); int m = t.size();
        vector<vector<long long>> dp(n + 1, vector<long long>(m + 1, 0));

        for (int i = 0; i <= n; i++) dp[i][m] = 1;

        for (int i = n - 1; i >= 0; i--) {
            for (int j = m - 1; j >= 0; j--) {
                if (s[i] == t[j]) dp[i][j] = dp[i + 1][j + 1] + dp[i + 1][j];
                else { dp[i][j] = dp[i + 1][j]; }
            }
        }

        return dp[0][0];}
    ```

### ***Edit Distance***

- We are given two strings ‘S1’ and ‘S2’. We need to convert S1 to S2. The following three operations are allowed: Deletion of a character. Replacement of a character with another one. Insertion of a character. We have to return the minimum number of operations required to convert S1 to S2 as our answer.

- Example 1: Input: start = "planet", target = "plan". Output: 2
- Example 2: Input: start = "abcdefg", target = "azced". Output: 4

- ```cpp
    int editDistanceUtil(string& S1, string& S2, int i, int j, vector<vector<int>>& dp) {
        if (i < 0) return j + 1;
        if (j < 0) return i + 1;

        if (dp[i][j] != -1) return dp[i][j];

        if (S1[i] == S2[j]) return dp[i][j] = 0 + editDistanceUtil(S1, S2, i - 1, j - 1, dp);
        else
            return dp[i][j] = 1 + min(editDistanceUtil(S1, S2, i - 1, j - 1, dp),
                                    min(editDistanceUtil(S1, S2, i - 1, j, dp),
                                        editDistanceUtil(S1, S2, i, j - 1, dp)));}

    int editDistance(string& S1, string& S2) {
        int n = S1.size(); int m = S2.size();
        vector<vector<int>> dp(n, vector<int>(m, -1));
        return editDistanceUtil(S1, S2, n - 1, m - 1, dp);
    }
    ```

- ```cpp
    int editDistance(string& S1, string& S2) {
        int n = S1.size(); int m = S2.size();
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));

        for (int i = 0; i <= n; i++) { dp[i][0] = i; }
        for (int j = 0; j <= m; j++) { dp[0][j] = j; }

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (S1[i - 1] == S2[j - 1]) { dp[i][j] = dp[i - 1][j - 1]; }
                else { dp[i][j] = 1 + min(dp[i - 1][j - 1], min(dp[i - 1][j], dp[i][j - 1])); }
            }}
        return dp[n][m];}
    ```

### ***Wildcard Matching***

- We are given two strings ‘S1’ and ‘S2’. String S1 can have the following two special characters. ‘?’ can be matched to a single character of S2. ‘*’ can be matched to any sequence of characters of S2. (sequence can be of length zero or more). We need to check whether strings S1 and S2 match or not.

- Input: S1 = "* a * b", S2 = "aaab". Output: true

- ```cpp
    bool isAllStars(string &S1, int i) {
        // Loop from 0 to i to verify all characters are '*'
        for (int j = 0; j <= i; j++) {
            if (S1[j] != '*')
                return false;
        }
        return true;
    }

    bool wildcardMatchingUtil(string &S1, string &S2, int i, int j, vector<vector<int>> &dp) {
        
        if (i < 0 && j < 0) return true; // Base Case 1: Both strings are exhausted
        if (i < 0 && j >= 0) return false; // Base Case 2: Pattern exhausted but text remains
        if (j < 0 && i >= 0) return isAllStars(S1, i); // Base Case 3: Text exhausted but pattern may still have '*'

        if (dp[i][j] != -1) return dp[i][j];

        // If characters match or pattern has '?'
        if (S1[i] == S2[j] || S1[i] == '?') return dp[i][j] = wildcardMatchingUtil(S1, S2, i - 1, j - 1, dp);

        // If pattern has '*', we have two choices:
        // 1. Treat '*' as matching empty sequence -> move pattern index i-1
        // 2. Treat '*' as matching one or more characters -> move text index j-1
        if (S1[i] == '*')
            return dp[i][j] = wildcardMatchingUtil(S1, S2, i - 1, j, dp) || wildcardMatchingUtil(S1, S2, i, j - 1, dp);

        return dp[i][j] = false;
    }

    bool wildcardMatching(string &S1, string &S2) {
        int n = S1.size(); int m = S2.size();
        vector<vector<int>> dp(n, vector<int>(m, -1));
        return wildcardMatchingUtil(S1, S2, n - 1, m - 1, dp);}
    ```

- ```cpp
    bool isAllStars(string &S1, int i) {
        // Loop from first character up to i-th character
        for (int j = 1; j <= i; j++) {
            // If any character is not '*', return false
            if (S1[j - 1] != '*')
                return false;
        }
        // All characters were '*', so return true
        return true;
    }

    // Function to perform wildcard pattern matching using tabulation DP
    bool wildcardMatching(string &S1, string &S2) {
        int n = S1.size();
        int m = S2.size();

        // dp[i][j] = true if pattern[0...i-1] matches text[0...j-1]
        vector<vector<bool>> dp(n + 1, vector<bool>(m + 1, false));

        // Base case: empty pattern matches empty string
        dp[0][0] = true;

        // Base case: empty pattern cannot match a non-empty string
        for (int j = 1; j <= m; j++) { dp[0][j] = false; }

        // Base case: pattern can match empty string only if it is made of all '*'
        for (int i = 1; i <= n; i++) { dp[i][0] = isAllStars(S1, i); }

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {

                // Case 1: Exact match or '?', which matches any single character
                if (S1[i - 1] == S2[j - 1] || S1[i - 1] == '?') {
                    // Take value from the diagonal cell
                    dp[i][j] = dp[i - 1][j - 1];
                }

                // Case 2: '*' can match zero or more characters
                else if (S1[i - 1] == '*') {
                    // Match zero characters → take value from above
                    // Match one or more characters → take value from left
                    dp[i][j] = dp[i - 1][j] || dp[i][j - 1];
                }

                // Case 3: No match
                else {
                    dp[i][j] = false;
                }
            }
        }

        // Final result is stored in dp[n][m]
        return dp[n][m];
    }
    ```

## ***Part 5: DP on Stocks***

### ***Stock Buy And Sell 1***

- You are given an array of prices where prices[i] is the price of a given stock on an ith day. You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock. Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

- Input: prices = [7,1,5,3,6,4]. Output: 5. Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.

- In O(N). What's the lowest price seen so far ? What's the profit if sold today ? Is it better than best so far ?

- ```cpp
    int stockbuySell(vector<int>& prices) {
        int minPrice = INT_MAX;
        int maxProfit = 0;

        for (int price : prices) {
            if (price < minPrice) { minPrice = price; }
            else { maxProfit = max(maxProfit, price - minPrice); }
        }
        return maxProfit;
    }
    ```

### ***Stock Buy And Sell 2***

- Here, flexibility is that we can buy and sell any number of times. But to sell, we must have bought. Also we can't buy after buying, we must sell first. Like the pattern should be BSBSBS. And not SB, BB, BSS and so on.

- Two conditions. Holding a stock. Not holding a stock. For each, two options, sell/buy or do nothing.

- ```cpp
    long getAns(long *Arr, int ind, int buy, int n, vector<vector<long>> &dp) {
        if (ind == n) { return 0; }
        if (dp[ind][buy] != -1) { return dp[ind][buy]; }

        long profit = 0;

        if (buy == 0) { profit = max(0 + getAns(Arr, ind + 1, 0, n, dp), -Arr[ind] + getAns(Arr, ind + 1, 1, n, dp)); } // We can buy the stock
        if (buy == 1) { profit = max(0 + getAns(Arr, ind + 1, 1, n, dp), Arr[ind] + getAns(Arr, ind + 1, 0, n, dp)); }

        return dp[ind][buy] = profit;}

    long getMaximumProfit(long *Arr, int n) {
        vector<vector<long>> dp(n, vector<long>(2, -1));
        if (n == 0) { return 0; }

        long ans = getAns(Arr, 0, 0, n, dp);
        return ans;}
    ```

- ```cpp
    long getMaximumProfit(long *Arr, int n) {
        vector<vector<long>> dp(n + 1, vector<long>(2, -1));
        dp[n][0] = dp[n][1] = 0;

        long profit;

        for (int ind = n - 1; ind >= 0; ind--) {
            for (int buy = 0; buy <= 1; buy++) {
                if (buy == 0) { profit = max(0 + dp[ind + 1][0], -Arr[ind] + dp[ind + 1][1]); } // We can buy the stock
                if (buy == 1) { profit = max(0 + dp[ind + 1][1], Arr[ind] + dp[ind + 1][0]); }  // We can sell the stock
                dp[ind][buy] = profit;
            }
        }
        return dp[0][0];
    }
    ```

### ***Stock Buy and Sell 3***

- Now new constraint is (apart from just previous one) that we can do at most two transactions.

- Tree to visualize solution/algorithm.
    - buy = 0
        - Do not do any transaction. return 0 + f(ind+1,0,cap)
        - Buy the stock. return -Arr[ind] + f(ind+1,1,cap)
    - buy = 1
        - Do not do any transaction. return 0 + f(ind+1,1,cap)
        - Sell the stock. return Arr[ind] + f(ind+1,0,cap-1)

- ```cpp
    int getAns(vector<int>& Arr, int n, int ind, int buy, int cap, vector<vector<vector<int>>>& dp) {
        if (ind == n || cap == 0) return 0;
        if (dp[ind][buy][cap] != -1) return dp[ind][buy][cap];

        int profit;

        if (buy == 0) { profit = max(0 + getAns(Arr, n, ind + 1, 0, cap, dp), -Arr[ind] + getAns(Arr, n, ind + 1, 1, cap, dp)); }
        if (buy == 1) { profit = max(0 + getAns(Arr, n, ind + 1, 1, cap, dp), Arr[ind] + getAns(Arr, n, ind + 1, 0, cap - 1, dp)); }

        return dp[ind][buy][cap] = profit;}

    int maxProfit(vector<int>& prices, int n) {
        vector<vector<vector<int>>> dp(n, vector<vector<int>>(2, vector<int>(3, -1))); // 3D DP array of size [n][2][3]
        return getAns(prices, n, 0, 0, 2, dp);}
    ```

- ```cpp
    int maxProfit(vector<int>& Arr, int n) {
        // 3D DP array of size [n+1][2][3] initialized to 0. Base case: dp array is already initialized to 0, covering the base case.
        vector<vector<vector<int>>> dp(n + 1, vector<vector<int>>(2, vector<int>(3, 0)));

        for (int ind = n - 1; ind >= 0; ind--) {
            for (int buy = 0; buy <= 1; buy++) {
                for (int cap = 1; cap <= 2; cap++) {
                    if (buy == 0) { dp[ind][buy][cap] = max(0 + dp[ind + 1][0][cap], -Arr[ind] + dp[ind + 1][1][cap]); }
                    if (buy == 1) { dp[ind][buy][cap] = max(0 + dp[ind + 1][1][cap], Arr[ind] + dp[ind + 1][0][cap - 1]); }
                }
            }
        } 
        return dp[0][0][2]; // The result is stored in dp[0][0][2] which represents maximum profit after the final transaction.
    }
    ```

### ***Stock Buy and Sell 4***

- Here the flexibility is we can do at most K transactions instead of only 2.

- Just make a loop for k times in tabulation approach ???? Yes true. Compare this solution with just above solution.

- ```cpp
    int getAns(vector& Arr, int n, int ind, int buy, int cap, vector>>& dp) {
        if (ind == n || cap == 0) return 0;
        if (dp[ind][buy][cap] != -1) return dp[ind][buy][cap];

        int profit;

        if (buy == 0) { profit = max(0 + getAns(Arr, n, ind + 1, 0, cap, dp), -Arr[ind] + getAns(Arr, n, ind + 1, 1, cap, dp));}
        if (buy == 1) { profit = max(0 + getAns(Arr, n, ind + 1, 1, cap, dp), Arr[ind] + getAns(Arr, n, ind + 1, 0, cap - 1, dp));}

        return dp[ind][buy][cap] = profit;
    }

    int maximumProfit(vector& prices, int n, int k) {
        // Creating a 3D DP array of size [n][2][k+1]
        vector<vector<vector<int>>> dp(n, vector<vector<int>>(2, vector<int>(k + 1, -1)));
        return getAns(prices, n, 0, 0, k, dp);}
    ```

- ```cpp
    int maximumProfit(vector& Arr, int n, int k) {
        // Creating a 3D DP array of size [n+1][2][k+1] initialized to 0
        vector<vector<vector<int>>> dp(n+1, vector<vector<int>>(2, vector<int>(k + 1, -1)));

        for (int ind = n - 1; ind >= 0; ind--) {
            for (int buy = 0; buy <= 1; buy++) {
                for (int cap = 1; cap <= k; cap++) {
                    if (buy == 0) { dp[ind][buy][cap] = max(0 + dp[ind + 1][0][cap], -Arr[ind] + dp[ind + 1][1][cap]); }
                    if (buy == 1) { dp[ind][buy][cap] = max(0 + dp[ind + 1][1][cap], Arr[ind] + dp[ind + 1][0][cap - 1]); }
                }
            }
        }
        return dp[0][0][k];
    }
    ```

### ***Stock Buy and Sell With Cooldown***

- There is no that max K transaction constraint. But constraint is we can't buy a stock on the very next day of selling it.

- BS_BS is allowed. SB, BB, BSS, B_SBS is not allowed.

- When selling , increase index by i+2 ???

- ```cpp
    int getAns(vector<int> Arr, int ind, int buy, int n, vector<vector<int>> &dp) {
        if (ind >= n) return 0;
        if (dp[ind][buy] != -1) return dp[ind][buy];
            
        int profit;
        
        if (buy == 0) { profit = max(0 + getAns(Arr, ind + 1, 0, n, dp), -Arr[ind] + getAns(Arr, ind + 1, 1, n, dp)); }
        if (buy == 1) { profit = max(0 + getAns(Arr, ind + 1, 1, n, dp), Arr[ind] + getAns(Arr, ind + 2, 0, n, dp)); }

        return dp[ind][buy] = profit;
    }

    int stockProfit(vector<int> &Arr) {
        int n = Arr.size();
        vector<vector<int>> dp(n, vector<int>(2, -1));
        int ans = getAns(Arr, 0, 0, n, dp);
        return ans;
    }
    ```

- ```cpp
    int stockProfit(vector<int> &Arr) {
    int n = Arr.size();
        // Create a 2D DP array with dimensions (n+2) x 2, initialized to 0
        vector<vector<int>> dp(n + 2, vector<int>(2, 0));
        
        for (int ind = n - 1; ind >= 0; ind--) {
            for (int buy = 0; buy <= 1; buy++) {
                int profit;

                if (buy == 0) { profit = max(0 + dp[ind + 1][0], -Arr[ind] + dp[ind + 1][1]); }
                if (buy == 1) { profit = max(0 + dp[ind + 1][1], Arr[ind] + dp[ind + 2][0]); }

                dp[ind][buy] = profit;
            }
        }

        return dp[0][0];
    }
    ```

### ***Stock Buy and Sell With Transaction Fees***

- Apart from general rules like buy and sell any number of times, buy before selling and can't buy after buying it once. Here the constraint is that after every transaction there is transaction fee associated with it. (No max K transaction, no cooldown constraint.) Also Buying & Selling together count as one transaction. Fee is subtracted only after selling.

- If we do transaction just add the fees ? Like we are doing +/- Arr[ind] ????

- ```cpp
    int getAns(vector<int> &Arr, int ind, int buy, int n, int fee, vector<vector<int>> &dp) {
        if (ind == n) return 0;
        if (dp[ind][buy] != -1) return dp[ind][buy];
            
        int profit;
        
        if (buy == 0) { profit = max(0 + getAns(Arr, ind + 1, 0, n, fee, dp), -Arr[ind] + getAns(Arr, ind + 1, 1, n, fee, dp)); }
        if (buy == 1) { profit = max(0 + getAns(Arr, ind + 1, 1, n, fee, dp), Arr[ind] - fee + getAns(Arr, ind + 1, 0, n, fee, dp)); }
    
        return dp[ind][buy] = profit;}

    int maximumProfit(int n, int fee, vector<int> &Arr) {
        vector<vector<int>> dp(n, vector<int>(2, -1));
        if (n == 0) return 0;
        int ans = getAns(Arr, 0, 0, n, fee, dp);
        return ans;}
    ```

- ```cpp
    int maximumProfit(int n, int fee, vector<int>& Arr) {
        if (n == 0) return 0;
        // Create a 2D DP array with dimensions (n+1) x 2, initialized to 0
        vector<vector<int>> dp(n + 1, vector<int>(2, 0));

        for (int ind = n - 1; ind >= 0; ind--) {
            for (int buy = 0; buy <= 1; buy++) {
                int profit;
                if (buy == 0) { profit = max(0 + dp[ind + 1][0], -Arr[ind] + dp[ind + 1][1]); }
                if (buy == 1) { profit = max(0 + dp[ind + 1][1], Arr[ind] - fee + dp[ind + 1][0]); }
                dp[ind][buy] = profit;
            }
        }
        return dp[0][0];}
    ```

## ***Part 6: DP on Longest Increasing Subsequence

## ***Part 7: Matric Chain Multiplication DP | Partition DP

## ***Part 8: DP on Squares