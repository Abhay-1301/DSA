# ***Sliding Window & Two Pointers***

- Pattern of problem statement:
    - Constant Window
    - Longest subarray/substring where `<condition>`
        - Brute, Better, Optimal Approach
    - Number of subarray/substring where `<condition>`
        - Kind of difficult. Will use pattern 2 to solve.
        - Difficult because whether to expand or shrink window ? Tough to decide. 
        - Example: No of subarrays with sum = k.
            - This is solved by: (No of subarrays with sum <= k) = x, (No of subarrays with sum <= k-1) = y. Answer is x - y.
    - Shortest/Minimum window where `<condition>`
        - Start from max window and shrink.

### ***Length of Longest Substring without any Repeating Character***

- Given a string, S. Find the length of the longest substring without repeating characters.

- Input: S = "abcddabac" Output: 4. (abcd)
- Input: S = "aaabbbccc" Output: 2. (ab or bc)

- Brute force is starting from each character check how many upcoming character are different by inserting into map and return the answer.
- Optimal using two pointers.

- What the hell is hash array storing ?

- Use an array hash of size 256 (assuming ASCII characters) to store the last occurrence index of each character in the string. Initialize all elements of hash to -1, indicating that no characters have been encountered yet.

- check if current character has occured before using hash array. If so, updadate the left pointer to index of current character plus 1. This ensures that l moves past the last occurrence of of repeated character, effectively removing the repeated character from the window.

- ```cpp
    int longestNonRepeatingSubstring(string& s){
        int n = s.size(); int HashLen = 256; int hash[HashLen];
        for (int i = 0; i < HashLen; ++i) hash[i] = -1;
        int l = 0, r = 0, maxLen = 0;

        while (r < n){
            if (hash[s[r]] != -1) {
                l = max(hash[s[r]] + 1, l);
            }
            int len = r - l + 1;
            maxLen = max(len, maxLen);
            hash[s[r]] = r;
            r++;
        }
        return maxLen;
    }
    ```

### ***Max Consecutive Ones III***

- Given a binary array nums and an integer k, return the maximum number of consecutive 1's in the array if you can flip at most k 0's.

- Input : nums = [1, 1, 1, 0, 0, 0, 1, 1, 1, 1, 0] , k = 3, Output : 10
- Input : nums = [0, 0, 1, 1, 1, 0, 1, 1, 1, 0, 0, 0, 0, 1, 1, 1, 1] , k = 3, Output : 9

- Brute
- ```cpp
    int longestOnes(vector<int>& nums, int k){
        int maxLen = 0;
        for (int i = 0; i < nums.size(); i++){
            int zeros = 0;
            for (int j = i; j < nums.size(); j++){
                if (nums[j] == 0) zeros++;
                if (zeros > k) break;
                maxLen = max(maxLen, j - i + 1);
            }
        }
        return maxLen;
    }
    ```

- Better
- ```cpp
    int longestOnes(vector<int>& nums, int k) {
        int left = 0; int zeros = 0; int maxLen = 0;

        for (int right = 0; right < nums.size(); right++){
            if (nums[right] == 0) zeros++;
            while (zeros > k) {
                if (nums[left] == 0) zeros--;
                left++; 
            }
            maxLen = max(maxLen, right - left + 1);
        }
        return maxLen;
    }
    ```

- Optimal
- ```cpp
    int longestOnes(vector<int>& nums, int k){
        int left = 0; int zerocount = 0; int maxlen = 0;

        for (int right = 0; right < nums.size(); right++){
            if (nums[right] == 0) zerocount++;
            if (zerocount > k){
                if (nums[left] == 0) zerocount--;
                left++; 
            }
            maxlen = max(maxlen, right - left + 1);
        }
        return maxlen;
    }
    ```


### ***Fruit Into Baskets***

- There is only one row of fruit trees on the farm, oriented left to right. An integer array called fruits represents the trees, where fruits[i] denotes the kind of fruit produced by the ith tree. The goal is to gather as much fruit as possible, adhering to the owner's stringent rules. There are two baskets available, and each basket can only contain one kind of fruit. The quantity of fruit each basket can contain is unlimited. Start at any tree, but as you proceed to the right, select exactly one fruit from each tree, including the starting tree. One of the baskets must hold the harvested fruits. Once reaching a tree with fruit that cannot fit into any basket, stop. Return the maximum number of fruits that can be picked.

- Input :fruits = [1, 2, 1] Output :3. We will start from first tree. The first tree produces the fruit of kind '1' and we will put that in the first basket. The second tree produces the fruit of kind '2' and we will put that in the second basket. The third tree produces the fruit of kind '1' and we have first basket that is already holding fruit of kind '1'. So we will put it in first basket. Hence we were able to collect total of 3 fruits.

- Input : fruits = [1, 2, 3, 2, 2] Output : 4. we will start from second tree. The first basket contains fruits from second , fourth and fifth. The second basket will contain fruit from third tree. Hence we collected total of 4 fruits.

- Brute Force Approach:
- ```cpp
    int totalFruit(vector<int>& fruits){
        int maxFruits = 0;
        for (int start = 0; start < fruits.size(); ++start){
            unordered_map<int, int> basket;
            int currentCount = 0;
            for (int end = start; end < fruits.size(); ++end){
                basket[fruits[end]]++;
                if (basket.size() > 2) break;
                currentCount++;}
            maxFruits = max(maxFruits, currentCount);}
        return maxFruits;
    }
    ```

- Better Approach:
- ```cpp
    int totalFruit(vector<int>& fruits) {
        unordered_map<int, int> basket;
        int maxFruits = 0;
        int left = 0;

        for (int right = 0; right < fruits.size(); right++) {
            basket[fruits[right]]++;
            while (basket.size() > 2) {
                basket[fruits[left]]--;
                if (basket[fruits[left]] == 0) basket.erase(fruits[left]);
                left++;}
            maxFruits = max(maxFruits, right - left + 1);}
        return maxFruits;}
    ```

- Optimal Approach
- ```cpp
    int totalFruit(vector<int>& fruits){
        int maxlen = 0;
        int lastfruit = -1, secondlastfruit = -1;
        int currcount = 0, lastfruitstreak = 0;

        for (int fruit : fruits){
            if (fruit == lastfruit || fruit == secondlastfruit) currcount++;
            else {currcount = lastfruitstreak + 1;}

            if (fruit == lastfruit) lastfruitstreak++;
            else {
                lastfruitstreak = 1;
                secondlastfruit = lastfruit;
                lastfruit = fruit;
            }
            maxlen = max(maxlen, currcount);}
        return maxlen;}
    ```

### ***Longest repeating character replacement***

- Given an integer k and a string s, any character in the string can be selected and changed to any other uppercase English character. This operation can be performed up to k times. After completing these steps, return the length of the longest substring that contains the same letter.

- Input: s = "BAABAABBBAAA", k = 2. Output: 6. Explanation: We can change the B at index 0 and 3 (0-based indexing) to A. The new string becomes "AAAAAABBBAAA". The substring "AAAAAA" is the longest substring with the same letter, and its length is 6.
- Input: s = "AABABBA", k = 1. Output: 4. Explanation: We can change one character to get the new string "AABBBBA". The substring "BBBB" is the longest with the same character. There are other ways to achieve this result as well.

- Brute Force Approach:
- ```cpp
    int characterReplacement(string s, int k) {
        
        // Variable to store the maximum length of valid substring
        int maxLength = 0;
        
        // Traverse all possible substrings
        for (int i = 0; i < s.length(); i++) {
            
            // Initialize frequency array for current substring
            vector<int> freq(26, 0);
            
            // Track max frequency character in the current substring
            int maxFreq = 0;
            
            // Expand substring starting from index i
            for (int j = i; j < s.length(); j++) {
                
                // Update frequency of current character
                freq[s[j] - 'A']++;
                
                // Update the most frequent character seen so far
                maxFreq = max(maxFreq, freq[s[j] - 'A']);
                
                // Calculate total length of current substring
                int windowLength = j - i + 1;
                
                // Check how many characters we need to replace
                int replace = windowLength - maxFreq;
                
                // If number of replacements is within allowed k, update answer
                if (replace <= k) {
                    maxLength = max(maxLength, windowLength);
                }
            }
        }
        
        return maxLength;
    }
    ```

- Better Approach:
- ```cpp
    int characterReplacement(string s, int k) {
        
        // Map to count frequency of characters in current window
        unordered_map<char, int> freq;

        // Left pointer of the sliding window
        int left = 0;

        // Tracks the count of the most frequent character in the window
        int max_freq = 0;

        // Stores the result (maximum length found)
        int max_len = 0;

        // Traverse the string with right pointer
        for (int right = 0; right < s.length(); right++) {

            // Increase frequency of the current character
            freq[s[right]]++;

            // Update max frequency seen so far in the window
            max_freq = max(max_freq, freq[s[right]]);

            // If window is invalid (needs more than k replacements)
            while ((right - left + 1) - max_freq > k) {

                // Decrease frequency of the character at left
                freq[s[left]]--;

                // Shrink the window from the left
                left++;
            }

            // Update max_len with current valid window size
            max_len = max(max_len, right - left + 1);
        }

        // Return the final result
        return max_len;
    }
    ```

- Optimal Approach
- ```cpp
    int characterReplacement(string s, int k) {
        // Frequency array for A-Z
        vector<int> freq(26, 0);
        
        // Left and right pointers of sliding window
        int left = 0, right = 0;

        // Tracks the count of the most frequent character in current window
        int maxCount = 0;

        // Stores the maximum length of valid window
        int maxLength = 0;

        // Iterate through the string with right pointer
        while (right < s.size()) {

            // Increment the frequency of current character
            freq[s[right] - 'A']++;

            // Update maxCount with the max frequency seen so far
            maxCount = max(maxCount, freq[s[right] - 'A']);

            // If the current window needs more than k replacements, move left
            while ((right - left + 1) - maxCount > k) {
                freq[s[left] - 'A']--;
                left++;
            }

            // Update the maximum window length
            maxLength = max(maxLength, right - left + 1);
            
            // Move right pointer forward
            right++;
        }

        // Return the maximum valid window length
        return maxLength;
    }
    ```

### ***Binary subarray with sum***

- You are given a binary array nums (containing only 0s and 1s) and an integer goal. Return the number of non-empty subarrays of nums that sum to goal. A subarray is a contiguous part of the array.

- Input: nums = [1, 0, 0, 1, 1, 0], goal = 2 ; Output: 6 ; Explanation: There are 6 subarrays with sum exactly equal to 2: [1, 0, 0, 1], [0, 0, 1, 1], [0, 1, 1], [1, 1], [1, 1, 0], [0,0,1,1,0]
- Input: nums = [0,0,0,0,0,0], goal = 0 ; Output: 21 ; Explanation: All subarrays with only 0s will have sum = 0. There are 21 such subarrays in total (n(n+1)/2 = 6*7/2 = 21).

- Brute Force Approach:
- ```cpp
    int numSubarraysWithSum(vector<int>& nums, int goal) {
        // Variable to store the final count of valid subarrays
        int count = 0;

        // Outer loop to fix the starting index of subarray
        for (int start = 0; start < nums.size(); ++start) {
            // Variable to store sum of current subarray
            int sum = 0;

            // Inner loop to fix the ending index of subarray
            for (int end = start; end < nums.size(); ++end) {
                // Add the current element to sum
                sum += nums[end];

                // If subarray sum equals goal, increment count
                if (sum == goal) {
                    count++;
                }
            }
        }

        // Return the total count of valid subarrays
        return count;
    }
    ```

- Better Approach:
- ```cpp
    int numSubarraysWithSum(vector<int>& nums, int goal) {
        // Hashmap to store prefix sum frequencies
        unordered_map<int, int> prefixSumCount;

        // Initialize count of valid subarrays and current sum
        int count = 0, sum = 0;

        // Add base case: prefix sum 0 has frequency 1
        prefixSumCount[0] = 1;

        // Iterate through the array
        for (int num : nums) {
            // Add current element to prefix sum
            sum += num;

            // If (sum - goal) exists in map, add its frequency to count
            if (prefixSumCount.find(sum - goal) != prefixSumCount.end()) {
                count += prefixSumCount[sum - goal];
            }

            // Increment frequency of current prefix sum
            prefixSumCount[sum]++;
        }

        // Return total count of valid subarrays
        return count;
    }
    ```

- Optimal Approach
- ```cpp
    int numSubarraysWithSum(vector<int>& nums, int goal) {
        // Return difference between subarrays with sum at most goal and at most (goal - 1)
        return atMost(nums, goal) - atMost(nums, goal - 1);
    }

    // Helper function to compute number of subarrays with sum at most k
    int atMost(vector<int>& nums, int k) {
        // If k is negative, no such subarrays exist
        if (k < 0) return 0;

        int left = 0;
        int sum = 0;
        int count = 0;

        // Traverse the array using right pointer
        for (int right = 0; right < nums.size(); right++) {
            // Add current element to sum
            sum += nums[right];

            // Shrink the window from the left if sum exceeds k
            while (sum > k) {
                sum -= nums[left];
                left++;
            }

            // Add the number of valid subarrays ending at right
            count += (right - left + 1);
        }

        return count;
    }
    ```

### ***Count number of nice subarrays***

- Given an array nums and an integer k. An array is called nice if and only if it contains k odd numbers. Find the number of nice subarrays in the given array nums. A subarray is continuous part of the array.

- Input :nums = [1, 1, 2, 1, 1] , k = 3 Output :2 Explanation :The subarrays with three odd numbers are [1, 1, 2, 1] [1, 2, 1, 1]
- Input : nums = [4, 8, 2] , k = 1 Output :0 Explanation :The array does not contain any odd number.

- Brute Force Approach:
- ```cpp
    int numberOfSubarrays(vector<int>& nums, int k) {
        // Initialize counter for total nice subarrays
        int count = 0;

        // Loop over all starting indices
        for (int start = 0; start < nums.size(); start++) {
            // Track number of odd elements in current subarray
            int oddCount = 0;

            // Loop over ending indices starting from 'start'
            for (int end = start; end < nums.size(); end++) {
                // Check if current number is odd
                if (nums[end] % 2 != 0)
                    oddCount++;

                // If odd count exceeds k, break (not nice)
                if (oddCount > k) break;

                // If odd count is exactly k, count this subarray
                if (oddCount == k)
                    count++;
            }
        }

        // Return total nice subarrays
        return count;
    }
    ```

- Better Approach:
- ```cpp
    int numberOfSubarrays(vector<int>& nums, int k) {

        // Frequency map to track how often a certain odd count has occurred
        unordered_map<int, int> freq;

        // Initialize with 0 count of odd numbers seen so far
        freq[0] = 1;

        // Running count of odd numbers in the current prefix
        int oddCount = 0;

        // Total number of nice subarrays
        int result = 0;

        // Traverse through each element in the array
        for (int num : nums) {

            // Check if current number is odd and update count
            if (num % 2 == 1) oddCount++;

            // If there exists a prefix with (current odd count - k), add its frequency to result
            if (freq.count(oddCount - k)) {
                result += freq[oddCount - k];
            }

            // Update the frequency of current odd count
            freq[oddCount]++;
        }

        // Return the total number of valid subarrays
        return result;
    }

- Optimal Approach
- ```cpp
    int countAtMost(vector<int>& nums, int k) {
        // Initialize variables
        int left = 0, res = 0;

        // Traverse through the array
        for (int right = 0; right < nums.size(); right++) {
            // If current number is odd, reduce k
            if (nums[right] % 2 != 0)
                k--;

            // Shrink the window until k is valid
            while (k < 0) {
                if (nums[left] % 2 != 0)
                    k++;
                left++;
            }

            // Add valid subarrays ending at right
            res += (right - left + 1);
        }

        // Return the count of valid subarrays
        return res;
    }

    // Main function to get number of subarrays with exactly k odd numbers
    int numberOfSubarrays(vector<int>& nums, int k) {
        return countAtMost(nums, k) - countAtMost(nums, k - 1);
    }
    ```


     - ### Number of Substrings Containing All Three Characters
    - Given a string s , consisting only of characters 'a' , 'b' , 'c'.Find the number of substrings that contain at least one occurrence of all these characters 'a' , 'b' , 'c'.
        - Input : s = "abcba" Output :  5 Explanation : The substrings containing at least one occurrence of the characters 'a' , 'b' , 'c' are "abc" , "abcb" , "abcba" , "bcba" , "cba".
        - Input : s = "ccabcc" Output : 8 Explanation : The substrings containing at least one occurrence of the characters 'a' , 'b' , 'c' are "ccab" , "ccabc" , "ccabcc" , "cab" , "cabc" , "cabcc" , "abc" , "abcc".
    - Brute Force Approach:
        ```cpp
        int numberOfSubstrings(string s) {
            // Variable to store the final count
            int count = 0;
            // Length of the input string
            int n = s.length();

            // Outer loop to fix the start of the substring
            for (int i = 0; i < n; i++) {
                // Array to track the count of 'a', 'b', and 'c'
                vector<int> freq(3, 0);

                // Inner loop to fix the end of the substring
                for (int j = i; j < n; j++) {
                    // Update frequency for current character
                    freq[s[j] - 'a']++;

                    // Check if all three characters are present
                    if (freq[0] > 0 && freq[1] > 0 && freq[2] > 0) {
                        // Add valid substring
                        count++;
                    }
                }
            }
            return count;
        }
        ```
    - Optimal Approach
        ```cpp
        int numberOfSubstrings(string s) {
            // Initialize frequency map for 'a', 'b', and 'c'
            vector<int> freq(3, 0);

            // Initialize result to store count of valid substrings
            int res = 0;

            // Initialize left pointer of the sliding window
            int left = 0;

            // Traverse the string using right pointer
            for (int right = 0; right < s.length(); right++) {
                // Increment frequency of current character
                freq[s[right] - 'a']++;

                // Shrink the window from the left while all three characters are present
                while (freq[0] > 0 && freq[1] > 0 && freq[2] > 0) {
                    // Count all substrings from current right to end
                    res += (s.length() - right);

                    // Decrease frequency of character at left and move left forward
                    freq[s[left] - 'a']--;
                    left++;
                }
            }

            return res;
        }
        ```

### ***Maximum point you can obtain from cards***

- Given N cards arranged in a row, each card has an associated score denoted by the cardScore array. Choose exactly k cards. In each step, a card can be chosen either from the beginning or the end of the row. The score is the sum of the scores of the chosen cards.

- Input :cardScore = [1, 2, 3, 4, 5, 6] , k = 3 Output : 15 Explanation :Choosing the rightmost cards will maximize your total score. So optimal cards chosen are the rightmost three cards 4 , 5 , 6. The score is 4 + 5 + 6 => 15.
- Input :cardScore = [5, 4, 1, 8, 7, 1, 3 ] , k = 3 Output :12 Explanation : In first step we will choose card from beginning with score of 5. In second step we will choose the card from beginning again with score of 4. In third step we will choose the card from end with score of 3. The total score is 5 + 4 + 3 => 12

- Brute Force Approach:
- ```cpp
    int maxScore(vector<int>& cardPoints, int k) {
        // Get total number of cards
        int n = cardPoints.size();

        // Initialize the answer to 0
        int maxSum = 0;

        // Try taking i cards from the start and (k-i) from the end
        for (int i = 0; i <= k; i++) {
            // Initialize temporary sum for this combination
            int tempSum = 0;

            // Sum of i cards from the front
            for (int j = 0; j < i; j++) {
                tempSum += cardPoints[j];
            }

            // Sum of (k - i) cards from the back
            for (int j = 0; j < k - i; j++) {
                tempSum += cardPoints[n - 1 - j];
            }

            // Update max if this is a better combination
            maxSum = max(maxSum, tempSum);
        }

        // Return the best total found
        return maxSum;
    }
    ```

- Optimal Approach
- ```cpp
    int maxScore(vector<int>& cardPoints, int k) {
        // Get the total number of cards
        int n = cardPoints.size();

        // Calculate initial sum by picking first k cards from front
        int total = 0;
        for (int i = 0; i < k; ++i) {
            total += cardPoints[i];
        }

        // Store current max score
        int maxPoints = total;

        // Move the window from front to back k times
        for (int i = 0; i < k; ++i) {
            // Subtract card from front
            total -= cardPoints[k - 1 - i];

            // Add card from back
            total += cardPoints[n - 1 - i];

            // Update max score if needed
            maxPoints = max(maxPoints, total);
        }

        // Return the best score
        return maxPoints;
    }
    ```

### ***Longest Substring with At Most K Distinct Characters***

- Given a string s and an integer k.Find the length of the longest substring with at most k distinct characters.

- Input :s = "aababbcaacc" , k = 2 Output :6 Explanation :The longest substring with at most two distinct characters is "aababb". The length of the string 6
- Input : s = "abcddefg" , k = 3 Output : 4 Explanation : The longest substring with at most three distinct characters is "bcdd". The length of the string 4.

- Brute Force Approach:
- ```cpp
    int lengthOfLongestSubstringKDistinct(string s, int k) {
        // Store the maximum length of valid substring
        int maxLength = 0;

        // Try every possible substring
        for (int i = 0; i < s.size(); i++) {
            // Use set/map to track distinct characters in current substring
            unordered_map<char, int> freq;

            for (int j = i; j < s.size(); j++) {
                // Add current character to map and update frequency
                freq[s[j]]++;

                // If number of distinct characters exceeds k, break
                if (freq.size() > k) break;

                // Update maximum length if valid
                maxLength = max(maxLength, j - i + 1);
            }
        }

        // Return the maximum valid substring length
        return maxLength;
    }
    ```

- Optimal Approach
- ```cpp
    int lengthOfLongestSubstringKDistinct(string s, int k) {
        // Edge case: if k is 0 or string is empty
        if (k == 0 || s.empty()) return 0;

        // Hash map to store frequency of characters in current window
        unordered_map<char, int> freq;

        // Initialize left pointer of sliding window
        int left = 0;

        // Initialize variable to store maximum length
        int maxLen = 0;

        // Loop through the string using right pointer
        for (int right = 0; right < s.length(); right++) {
            // Include current character into frequency map
            freq[s[right]]++;

            // Shrink window if number of distinct characters exceeds k
            while (freq.size() > k) {
                freq[s[left]]--;

                // If character count becomes zero, erase from map
                if (freq[s[left]] == 0) {
                    freq.erase(s[left]);
                }

                // Move left pointer ahead
                left++;
            }

            // Update maxLen with current valid window size
            maxLen = max(maxLen, right - left + 1);
        }

        // Return the final answer
        return maxLen;
    }
    ```

### ***Subarray with k different integers***

- You are given an integer array nums and an integer k. Return the number of good subarrays of nums. A good subarray is defined as a contiguous subarray of nums that contains exactly k distinct integers. A subarray is a contiguous part of the array.

- Input: nums = [1, 2, 1, 2, 3], k = 2 Output: 7 Explanation: The 7 subarrays with exactly 2 different integers are: [1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2]
- Input: nums = [1, 2, 1, 3, 4], k = 3 Output: 3 Explanation: The 3 subarrays with exactly 3 different integers are: [1,2,1,3], [2,1,3], [1,3,4]

- Brute Force Approach:
- ```cpp
    int subarraysWithKDistinct(vector<int>& nums, int k) {
        // Get the size of the array
        int n = nums.size();

        // Variable to store the final result
        int count = 0;

        // Loop through all possible starting indices
                for (int i = 0; i < n; i++) {

            // Map to keep track of frequency of elements
            unordered_map<int, int> freq;

            // Loop through all possible ending indices
            for (int j = i; j < n; j++) {

                // Increment frequency of current element
                freq[nums[j]]++;

                // If the number of distinct integers equals k, increment count
                if (freq.size() == k)
                    count++;

                // If it exceeds k, break out
                if (freq.size() > k)
                    break;
            }
        }

        // Return the result
        return count;
    }
    ```

- Optimal Approach
- ```cpp
    int atMostK(vector<int>& nums, int K) {
        unordered_map<int, int> freq;
        int left = 0, count = 0;

        // Traverse the array with right pointer
        for (int right = 0; right < nums.size(); right++) {
            // If it's a new unique element, decrease K
            if (freq[nums[right]] == 0) {
                K--;
            }

            // Increment frequency of current element
            freq[nums[right]]++;

            // Shrink the window if distinct count > K
            while (K < 0) {
                freq[nums[left]]--;
                if (freq[nums[left]] == 0) {
                    K++;
                }
                left++;
            }

            // Count all subarrays ending at current right
            count += (right - left + 1);
        }

        return count;
    }

    // Main function to return number of subarrays with exactly K distinct integers
    int subarraysWithKDistinct(vector<int>& nums, int k) {
        return atMostK(nums, k) - atMostK(nums, k - 1);
    }
    ```

### Minimum Window Substring (Only problem - No Notes)

- Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "". The testcases will be generated such that the answer is unique.

- Example 1: Input: s = "ADOBECODEBANC", t = "ABC" Output: "BANC" Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
- Example 2: Input: s = "a", t = "a" Output: "a" Explanation: The entire string s is the minimum window.
- Example 3: Input: s = "a", t = "aa" Output: "" Explanation: Both 'a's from t must be included in the window. Since the largest window of s only has one 'a', return empty string.



### Minimum Window Subsequence (Only problem - No Notes)

- Given strings s1 and s2, return the minimum contiguous substring part of s1, so that s2 is a subsequence of the part. If there is no such window in s1 that covers all characters in s2, return the empty string "". If there are multiple such minimum-length windows, return the one with the left-most starting index.

- Example 1: Input: s1 = "abcdebdde", s2 = "bde". Output: "bcde". Explanation: "bcde" is the answer because it occurs before "bdde" which has the same length. "deb" is not a smaller window because the elements of s2 in the window must occur in order.
- Example 2: Input: s1 = "jmeqsiwvaovvnbstl", s2 = "u". Output: ""

