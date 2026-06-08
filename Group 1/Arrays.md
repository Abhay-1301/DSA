# Arrays

### ***Two Sum : Check if a pair with given sum exists in Array***

- Easy brute force in quadratic time.
- Better using hashmaps with linear time and space.
- Another approach using sorting with nlogn time.

### ***Sort an array of 0s, 1s and 2s***

- We are given an array containing only 0s, 1s, and 2s. Since the values are fixed and known, the simplest approach is to first count how many 0s, 1s, and 2s are present in the array. After counting, we overwrite the original array based on the frequency of these values first fill it with 0s, then 1s, then 2s. This does not require any extra array and modifies the input array in-place.

- Use two pointers and push 0 to left, 2 to right and 1 automatically comes at place.

### ***Find the Majority Element that occurs more than N/2 times***

- Given an integer array nums of size n, return the majority element of the array. The majority element of an array is an element that appears more than n/2 times in the array. The array is guaranteed to have a majority element.

- One way using sorting. Other using hashmaps. Brute is just quadratic for each element run loop to count occurance.

# hehehuhuArr

### ***Remove Duplicates in-place from Sorted Array***

- Given an integer array sorted in non-decreasing order, remove the duplicates in place such that each unique element appears only once. The relative order of the elements should be kept the same. If there are k elements after removing the duplicates, then the first k elements of the array should hold the final result. It does not matter what you leave beyond the first k elements.

- Input: arr=1,1,2,2,2,3,3 Output: 1,2,3,_,_,_,_
- Input: arr=1,1,1,2,2,3,3,3,3,4,4 Output: 1,2,3,4,_,_,_,_,_,_,_

- Instead of using a set to store the unique elements, we can implement a two pointer strategy to optimize the space. Since the array is sorted, we know that all the duplicate values will be adjacent to each other.
    - Begin at the first position, which will always be part of the final unique list.
    - Move through the list one item at a time, comparing the current item with the most recently kept unique item.
    - If the current item is the same as the last kept one, skip it because it’s a duplicate.
    - If it’s different, place it right after the last kept unique item to keep all unique values grouped at the front. 
    - Continue until every element in the list has been checked. The first part of the list now contains all the unique values in their original order, and the rest can be ignored.

### ***Rotate array by K elements***

- Examples:
    - Input : nums = [1, 2, 3, 4, 5, 6, 7], k = 2, right
    - Output : [6, 7, 1, 2, 3, 4, 5]

    - Input : nums = [1, 2, 3, 4, 5, 6], k=2, left
    - Output : [3, 4, 5, 6, 1, 2]

- I thought about brute force approach using an extra array to store some element and then rotate and use value from extra array to populate remaining.

- Instead of simulating each rotation one by one, we can get the rotated array in-place by reversing specific parts of the array. This works because rotating is just rearranging sections of the array.
    - Normalize k by doing k = k % N
    - For Right Rotation by k steps:
        - Reverse the entire array [7 6 5 4 3 2 1]
        - Reverse the first k elements [6 7 5 4 3 2 1]
        - Reverse the rest (from k to end) remaining n - k elements [6 7 1 2 3 4 5]
    - For Left Rotation by k steps:
        - Reverse the first k elements [2 1 3 4 5 6]
        - Reverse the rest (from k to end) remaining n - k elements [2 1 6 5 4 3]
        - Reverse the entire array [3 4 5 6 1 2]

### ***Move all Zeros to the end of the array***

- Example:
    - Input: 1 ,0 ,2 ,3 ,0 ,4 ,0 ,1
    - Output: 1 ,2 ,3 ,4 ,1 ,0 ,0 ,0

    - Input : 1,2,0,1,0,4,0
    - Output: 1,2,1,4,0,0,0

- Brute force is making a copy of array.
- We can optimize the approach using 2 pointers i.e. i and j. The pointer j will point to the first 0 in the array and i will point to the next index.

- Assume, the given array is {1, 0, 2, 3, 2, 0, 0, 4, 5, 1}. Now, initially, we will place the 2-pointers like the following:
    - First, we iterate through the array to locate the position of the first zero, using a pointer j. If no zero is found, no further steps are needed.
    - Next, we set a second pointer i to j + 1 and start moving it forward through the array.
    - While moving i, whenever we encounter a non-zero element a[i], we swap it with the element at index j. After the swap, since j now holds a non-zero value, we increment j to point to the next zero.

### ***Find missing number***

- Given an array arr[] of size n-1 with distinct integers in the range of [1, n]. This array represents a permutation of the integers from 1 to n with one element missing. Find the missing element in the array.
- Input: arr[] = 8, 2, 4, 5, 3, 7, 1 Output: 6
- Input: arr = 1, 2, 3, 5 Output: 4

- [Naive Approach] Linear Search for Missing Number - O(n^2) Time and O(1) Space
- [Better Approach] Using Hashing - O(n) Time and O(n) Space
- [Expected Approach 1] Using Sum of n terms Formula - O(n) Time and O(1) Space.
    - The sum of the first n natural numbers is given by the formula (n * (n + 1)) / 2. The idea is to compute this sum and subtract the sum of all elements in the array from it to get the missing number.
- [Expected Approach 2] Using XOR Operation - O(n) Time and O(1) Space
    - XOR of a number with itself is 0 i.e. x ^ x = 0 and the given array arr[] has numbers in range [1, n]. This means that the result of XOR of first n natural numbers with the XOR of all the array elements will be the missing number. To do so, calculate XOR of first n natural numbers and XOR of all the array arr[] elements, and then our result will be the XOR of both the resultant values.

### ***Find the number that appears once, and the other numbers twice***

- Input: arr = {4,1,2,1,2}, Result: 4
- Do linear search for every element. N square.
- Sort and answer in NlogN.
- Map and answer in N but also extra space N.
- Optimal answer is using XOR
    - Two important properties of XOR are the following:
        - XOR of two same numbers is always 0 i.e. a ^ a = 0.
        - XOR of a number with 0 will result in the number itself i.e. 0 ^ a = a.
    - We will just perform the XOR of all elements of the array using a loop and the final XOR will be the answer.
    - ```cpp
        int getSingleElement(vector<int>& arr) {
            int n = arr.size();
            int xorr = 0;

            // XOR all elements. Duplicates cancel out, leaving the single element.
            for (int i = 0; i < n; i++) {
                xorr = xorr ^ arr[i];
            }

            return xorr;
        }
        ```

### ***Longest Subarray with given Sum K (Positives)***

- Given an array nums of size n and an integer k, find the length of the longest sub-array that sums to k. If no such sub-array exists, return 0.
- Input: nums = [10, 5, 2, 7, 1, 9], k = 15, Output: 4
- Input: nums = [-3, 2, 1], k = 6, Output: 0

- Brute force in cubic N. Find all subarrays and check for sum again. Although it can be optimized to quadratic.
- Maybe two pointers ? Because it's subarray.
    - Two pointers, left and right, are used to maintain the current window of elements in the array. These pointers represent the start and end of the current subarray.
    - A variable, sum, is used to keep track of the sum of the elements in the current window between left and right.
    - The right pointer expands the window by including new elements, increasing the sum.
    - If the sum of the window exceeds k, the left pointer shrinks the window by removing elements from the start until the sum is less than or equal to k.
    - If the sum of the current window equals k, the maximum length of such a subarray is updated.
    - The process continues until the right pointer traverses the entire array.
    - Finally, the maximum length of the subarray with sum k is returned as the result.

### ***Length of the longest subarray with zero Sum***

- Given an array containing both positive and negative integers, we have to find the length of the longest subarray with the sum of all elements equal to zero.
- Examples:
    - Input: N = 6, array[] = {9, -3, 3, -1, 6, -5}, Result: 5  
        - Explanation: The following subarrays sum to zero:
            - {-3, 3}
            - {-1, 6, -5}
            - {-3, 3, -1, 6, -5}
            - The length of the longest subarray with sum zero is 5.
    - Input: N = 8, array[] = {6, -2, 2, -8, 1, 7, 4, -10}, Result: 8
        - Explanation: Subarrays with sum zero:
            - {-2, 2}
            - {-8, 1, 7}
            - {-2, 2, -8, 1, 7}
            - {6, -2, 2, -8, 1, 7, 4, -10}
            - The length of the longest subarray with sum zero is 8.

- Brute Force in Quadratic Time:
    - Initialize a variable max = 0, which stores the length of the longest subarray with a sum of 0.
    - Traverse the array from the start and initialize a variable sum = 0, which stores the sum of the subarray starting with the current index.
    - Traverse from the next element of the current index up to the end of the array. Each time, add the element to the sum and check if it is equal to 0.
    - If sum = 0, check if the length of the subarray so far is greater than max, and if yes, update max.
    - Continue adding elements and repeat the above step until the outer loop completes traversing all elements.
    - Finally, return the max which holds the length of the longest subarray with a sum of 0.

- Optimal in Linear Time:
    - Initialize a variable sum = 0, which stores the sum of elements traversed so far, and another variable max = 0, which stores the length of the longest subarray with sum zero.
    - Declare a `HashMap<Integer, Integer>` to store the prefix sum of every element as a key and its index as a value.
    - Traverse the array and add the array element to the sum.
    - If sum = 0, update max with the maximum value between max and current_index + 1, as the subarray from the start to the current index has a sum of 0.
    - If sum is not equal to zero, check the HashMap to see if we've encountered this sum before.
    - If the HashMap contains the sum, this indicates that a subarray with the same sum exists, so update max accordingly.
    - If the sum is not found in the HashMap, insert (sum, current_index) into the HashMap to store the prefix sum until the current index.
    - After traversing the entire array, the max variable will hold the length of the longest subarray with a sum equal to zero. Return max.

### ***Find the Majority Element that occurs more than N/2 times***

- Given an integer array nums of size n, return the majority element of the array. The majority element of an array is an element that appears more than n/2 times in the array. The array is guaranteed to have a majority element.

- Can think of linear time and linear space solution with map.
- Given brute force in quadratic time. For each element, run loop for count.
- Better is my thought process above.
- Optimal in linear time with constant space.
    - Initialize two variables: count to track the count of elements, and element to keep track of the element being counted.
    - Traverse through the given array. If count is 0, store the current value of the array as element.
    - If the current element in the array is the same as element, increment the count by 1.
    - If the current element is different from element, decrement the count by 1.
    - At the end of the traversal, the integer stored in element will be the expected result (the majority element).
        - 2 2 1 1 1 2 2, Count = 0, Element = null
            - 1 2
            - 2 2
            - 1 2
            - 0 2 When count is zero, update element in next pass
            - 1 1 
            - 0 1
            - 1 2 Answer is 2.

### ***Kadane's Algorithm : Maximum Subarray Sum in an Array***

- Given an integer array nums, find the subarray with the largest sum and return the sum of the elements present in that subarray.
    - Input: nums = [2, 3, 5, -2, 7, -4], Output: 15, Explanation: The subarray from index 0 to index 4 has the largest sum = 15, which is the maximum sum of any contiguous subarray.
    - Input: nums = [-2, -3, -7, -2, -10, -4], Output: -2

- Brute force in cubic. One loop for each element. Another loop to find out all possible subarray. Third loop for sum.
- Better is losing the third loop and tracking the sum with second loop only. Brings down to quadratic complexity.
- Optimal in linear with constant space.
    - Iterate through the array using a variable i. During each iteration, add the current element arr.i to a running sum variable.
    - Keep track of the maximum sum encountered during the iteration by comparing the current sum with the previous maximum sum, and update it if the current sum is greater.
    - If at any point the sum becomes negative, reset it to 0, as a negative sum won't contribute positively to the overall maximum sum.
    - Continue the iteration until all elements in the array are processed.
    - Finally, return the maximum sum encountered during the iteration.
    - ```cpp
        int maxSubArray(vector<int>& nums) {
            long long maxi = LLONG_MIN;
            long long sum = 0; 
            for (int i = 0; i < nums.size(); i++) {
                sum += nums[i];
                if (sum > maxi) {maxi = sum;}
                if (sum < 0) {sum = 0;}
            }
            return maxi;
        }
        ```
    - Can you print the subarray that has the maximum sum?
        - Start by iterating through the array using a variable i. During each iteration, add the current element arr[i] to a running sum variable.
        - Initialize a start variable to keep track of the starting index of the current subarray.
        - Use ansStart and ansEnd to store the starting and ending indices of the subarray with the maximum sum found so far. Initially, set both to -1.
        - If the current sum is greater than the previous maximum sum, update ansStart to start and ansEnd to i.
        - If the sum becomes negative at any point, reset it to 0 and set start to i + 1 to start a new subarray.
        - After processing all elements, ansStart and ansEnd will point to the starting and ending indices of the subarray with the maximum sum.
        - Return the subarray from arr.ansStart to arr.ansEnd.

### ***Rearrange Array Elements by Sign***

- There’s an array ‘A’ of size ‘N’ with an equal number of positive and negative elements. Without altering the relative order of positive and negative elements, you must return an array of alternately positive and negative values.

    - Example 1: Input: arr[] = {1,2,-4,-5}, N = 4, Output: 1 -4 2 -5, Explanation: Positive elements = 1,2, Negative elements = -4,-5, To maintain relative ordering, 1 must occur before 2, and -4 must occur before -5.

    - Example 2: Input: arr[] = {1,2,3,-1,-2,-3}, N = 6, Output: 1 -3 2 -1 3 -2, Explanation: Positive elements = 1,2,3. Negative elements = -3,-1,-2. To maintain relative ordering, 1 must occur before 2, and 2 must occur before 3. Also, -3 should come before -1, and -1 should come before -2.

- Brute in Linear Time and Space
    - In this simple approach, since the number of positive and negative elements are the same, we put positives into an array called “pos” and negatives into an array called “neg”.
    - After segregating each of the positive and negative elements, we start putting them alternatively back into array A.
    - Since the array must begin with a positive number and the start index is 0, so all the positive numbers would be placed at even indices (2*i) and negatives at the odd indices (2*i+1), where i is the index of the pos or neg array while traversing them simultaneously.
    - This approach uses O(N+N/2) of running time due to multiple traversals which we’ll try to optimize in the optimized approach given below.

- Optimal in Same only but only one traversal.
    - In this optimal approach, we will try to solve the problem in a single pass and try to arrange the array elements in the correct order in that pass only.
    - We know that the resultant array must start from a positive element so we initialize the positive index as 0 and negative index as 1 and start traversing the array such that whenever we see the first positive element, it occupies the space at 0 and then posIndex increases by 2 (alternate places).
    - Similarly, when we encounter the first negative element, it occupies the position at index 1, and then each time we find a negative number, we put it on the negIndex and it increments by 2.
    - When both the negIndex and posIndex exceed the size of the array, we see that the whole array is now rearranged alternatively according to the sign.
    - ```cpp
        vector<int> rearrangeBySign(vector<int>& A) {
            int n = A.size();
            vector<int> ans(n, 0);
            int posIndex = 0, negIndex = 1;

            for (int i = 0; i < n; i++) {
                if (A[i] < 0) {
                    ans[negIndex] = A[i];
                    negIndex += 2;
                }
                else {
                    ans[posIndex] = A[i];
                    posIndex += 2;
                }
            }
            return ans;
        }
        ```

### ***Next Permutation***

- Given an array Arr[] of integers, rearrange the numbers of the given array into the lexicographically next greater permutation of numbers. If such an arrangement is not possible, it must rearrange to the lowest possible order (i.e., sorted in ascending order).
    - Input: Arr[] = {1,3,2}, Output: {2,1,3}, Explanation: All permutations of {1,2,3} are {{1,2,3} , {1,3,2}, {2,13} , {2,3,1} , {3,1,2} , {3,2,1}}. So, the next permutation just after {1,3,2} is {2,1,3}.
    - Input : Arr[] = {3,2,1}, Output: {1,2,3}, Explanation : As we see all permutations of {1,2,3}, we find {3,2,1} at the last position. So, we have to return the lowest permutation.

- Brute force in N factorial cross N time and N factorial space. The brute force approach to find the next permutation is to find all possible permutations of the array and then look for next permutation. Find all possible permutations of elements present and store them. Sort the permutations and search input from all possible permutations. Print the next permutation present right after it. If the current permutation is the last, return the first permutation in the list.

    - ```cpp
        vector<int> nextPermutation(vector<int>& nums) {
            vector<vector<int>> all;
            sort(nums.begin(), nums.end());
            do {
                all.push_back(nums);
            } while (next_permutation(nums.begin(), nums.end()));

            for (int i = 0; i < all.size(); i++) {
                if (all[i] == nums) {
                    if (i == all.size() - 1) // If it's the last permutation
                        return all[0];
                    return all[i + 1];
                }
            }            
            return nums; // Return original if not found (shouldn't happen)
        }
        ```

- Optimal in Linear Time with constant space
    - We want to rearrange the array to form the next greater permutation. If that's not possible (i.e., it's the last permutation), we return the smallest one (i.e., sorted ascendingly). To find this next permutation with minimal change, we need to find a digit that can be increased slightly to make the number bigger and then rearrange the remaining part to be the smallest possible.
        - Traverse from the end and find the first index where the current digit is smaller than the next one (this is the "breaking point").
        - Then again traverse from the end to find the first digit greater than the breaking point digit and swap them.
        - Finally, reverse the part of the array to the right of the breaking point to get the smallest next permutation.
        - If no such breaking point exists (entire array is descending), just reverse the whole array.
    - ```cpp
        void nextPermutation(vector<int>& nums) {
            int index = -1;

            // Find the first decreasing element from end
            for (int i = nums.size() - 2; i >= 0; i--) {
                if (nums[i] < nums[i + 1]) {
                    index = i;
                    break;
                }
            }

            if (index == -1) {
                reverse(nums.begin(), nums.end());
                return;
            }

            // Find element just greater than nums[index]
            for (int i = nums.size() - 1; i > index; i--) {
                if (nums[i] > nums[index]) {
                    swap(nums[i], nums[index]);
                    break;
                }
            }

            // Reverse the part after index
            reverse(nums.begin() + index + 1, nums.end());
        }
        ```

### ***Leaders in an Array***

- Example 1: Input: arr = [4, 7, 1, 0], Output: 7 1 0, Explanation: The rightmost element (0) is always a leader. 7 and 1 are greater than the elements to their right, making them leaders as well.
- Example 2: Input: arr = [10, 22, 12, 3, 0, 6], Output: 22 12 6, Explanation: 6 is a leader because there are no elements after it. 12 is greater than all the elements to its right (3, 0, 6), and 22 is greater than 12, 3, 0, 6, making them leaders as well.

- Iterate from last. In a variable max store max till that point from last obviously. An element is leader if it is greater than max. Optimal in linear time no space. Store in list and reverse the leaders answer array.
- Brute can be in quadratic check one by one, compare one by one.
	
### ***Longest Consecutive Sequence in an Array***

- Given an array nums of n integers. Return the length of the longest sequence of consecutive integers. The integers in this sequence can appear in any order.
    - Example 1: Input: nums = [100, 4, 200, 1, 3, 2], Output: 4, Explanation: The longest sequence of consecutive elements in the array is [1, 2, 3, 4], which has a length of 4. This sequence can be formed regardless of the initial order of the elements in the array.
    - Example 2: Input: nums = [0, 3, 7, 2, 5, 8, 4, 6, 0, 1], Output: 9, Explanation: The longest sequence of consecutive elements in the array is [0, 1, 2, 3, 4, 5, 6, 7, 8], which has a length of 9.

- Brute is sort and find out.
    - ```cpp
        int longestConsecutive(vector<int>& nums) {
            int n = nums.size();
            if (n == 0) return 0; 

            sort(nums.begin(), nums.end()); 

            int lastSmaller = INT_MIN; 
            int cnt = 0; // Count current sequence length
            int longest = 1; // Track longest sequence length

            for (int i = 0; i < n; i++) {
                if (nums[i] - 1 == lastSmaller) { // If consecutive number exists
                    cnt += 1; // Increment sequence count
                    lastSmaller = nums[i]; // Update last smaller element
                }

                // If consecutive number doesn't exits
                else if (nums[i] != lastSmaller) {
                    cnt = 1; // Reset count for new sequence
                    lastSmaller = nums[i]; // Update last smaller element
                }

                longest = max(longest, cnt); // Update longest if needed
            }
            return longest;
        }
        ```

- Optimal is linear time with linear space.
    - We will use two variables: cnt to store the length of the current sequence and longest to store the maximum length found.
    - First, place all the array elements into a set data structure to allow efficient lookups for consecutive numbers.
    - For each element x that can start a sequence (i.e., x - 1 does not exist in the set), we follow these steps:
        - Initialize cnt to 1, indicating the starting element of a new sequence.
        - Use the set to search for consecutive elements such as x + 1, x + 2, and so on, to determine the maximum possible length of the current sequence. Update cnt accordingly.
    - Compare cnt with longest and update longest to hold the maximum value: longest = max(longest, cnt).
    - Finally, longest will contain the length of the longest consecutive sequence found in the array.
    - ```cpp
        int longestConsecutive(vector<int>& a) {
            int n = a.size();
            if (n == 0) return 0; 
        
            int longest = 1; 
            unordered_set<int> st;
        
            for (int i = 0; i < n; i++) {
                st.insert(a[i]);
            }
        
            for (auto it : st) {
                // Check if 'it' is a starting number of a sequence
                if (st.find(it - 1) == st.end()) {
                    int cnt = 1; // Initialize the count of the current sequence
                    int x = it; // Starting element of the sequence
        
                    // Find consecutive numbers in the set
                    while (st.find(x + 1) != st.end()) {
                        x = x + 1; // Move to the next element in the sequence
                        cnt = cnt + 1; // Increment the count of the sequence
                    }

                    // Update the longest sequence length
                    longest = max(longest, cnt);
                }
            }
            return longest;
        }
        ```

### Set Matrix Zeroes
	
### ***Rotate matrix by 90 degrees***

- Given an N * N 2D integer matrix, rotate the matrix by 90 degrees clockwise. The rotation must be done in place, meaning the input 2D matrix must be modified directly.
    ```
    Input
    1 2 3
    4 5 6
    7 8 9

    Transpose
    1 4 7
    2 5 8
    3 6 9

    Reverse Column
    7 4 1
    8 5 2
    9 6 3

    Output
    7 4 1
    8 5 2
    9 6 3
    
    Input
    0 1 1 2
    2 0 3 1
    4 5 0 5
    5 6 7 0

    Output
    5 4 2 0
    6 5 0 1
    7 0 3 1
    0 5 1 2
    ```

- Brute and Optimal both takes linear time (linear means size of matrix if N*N then N square) but brute take extra matrix space and optimal does not.
- Brute:
    - In a 90-degree clockwise rotation, each element in the matrix moves from its original position to a new position based on a specific pattern. The first column becomes the first row (in reverse order) and second column becomes the second row, and so on. We can simulate this movement by using a new matrix. For each element at position (i, j) in the original matrix, we calculate its new position in the rotated matrix as (j, n - i - 1) where n is the size of the matrix.
        - Initialize an empty matrix of the same size (n x n).
        - Loop through every element of the original matrix using two nested loops.
        - For each element at position (i, j), place it in the new matrix at position (j, n - i - 1).
        - After completing the placement for all elements, return or copy the new matrix.

- Optimal:
    - To rotate a matrix 90 degrees clockwise in-place (without using extra space), we use two key matrix operations:
        - Step 1: Transpose the matrix: swap elements across the diagonal. This converts rows into columns.
        - Step 2: Reverse each row: this turns the new columns into the final rotated rows.

    - This works because:
        - Transposing moves elements from (i, j) to (j, i), effectively rotating across the diagonal.
        - Reversing the rows repositions the elements in each row, finalizing the clockwise rotation.
        - Get the size of the square matrix (number of rows or columns).
        - Start the first phase: Transpose the matrix
            - For each row starting from the top to bottom:
            - For each column starting from the diagonal element (excluding already visited elements):
            - Swap the current element with the element that is diagonally opposite across the main diagonal.
            - This effectively flips the matrix over its top-left to bottom-right diagonal, converting rows into columns.
        - Move to the second phase: Reverse each row
            - For every row in the matrix:
            - Reverse the order of the elements in that row (first element becomes last, second becomes second last, and so on).
            - This realigns the columns to their correct position for the clockwise rotation.
        - Once both phases are done, the matrix will have been rotated 90 degrees clockwise in-place.

    - ```cpp
        void rotateClockwise(vector<vector<int>>& matrix) {
            int n = matrix.size();

            // Step 1: Transpose the matrix
            for (int i = 0; i < n; ++i) {
                for (int j = i + 1; j < n; ++j) {
                    swap(matrix[i][j], matrix[j][i]); // Swap element at (i, j) with (j, i) to transpose
                }
            }

            // Step 2: Reverse each row
            for (int i = 0; i < n; ++i) {
                reverse(matrix[i].begin(), matrix[i].end()); // Reverse the current row to complete clockwise rotation
            }
        }
        ```

### ***Print the matrix in spiral manner | Spiral Traversal of Matrix***

- Given a Matrix, print the given matrix in spiral order.

    ```
    Input
    01 02 03 04
    05 06 07 08
    09 10 11 12
    13 14 15 16

    Output
    01 02 03 04 08 12 16 15 14 13 09 05 06 07 11 10
    ```

- The brute force method simulates movement in four directions: right, down, left, and up while keeping track of which cells have already been visited using a separate matrix. This approach ensures that we never revisit any element and always stay within bounds. After moving in one direction as far as possible, we rotate the direction and keep repeating until all elements are visited.
    - Initialize a 2D boolean matrix `visited` of same size as input to keep track of visited cells.
    - Define direction arrays for right, down, left, and up movements.
    - Start at (0, 0), and begin with direction = 0 (right).
    - For each of the total elements in the matrix:
    - Add the current cell to result and mark it as visited.
    - Check if the next cell in the current direction is valid and not visited.
    - If valid, move to it; else rotate the direction and try the new direction.
    - Repeat this for total number of cells in the matrix.

```cpp
vector<int> spiralOrder(vector<vector<int>>& matrix) {
    vector<int> result;

    // Get number of rows and columns
    int top = 0;
    int bottom = matrix.size() - 1;
    int left = 0;
    int right = matrix[0].size() - 1;

    // Traverse the matrix in spiral order
    while(top <= bottom && left <= right) {

        // Traverse from left to right across the top row
        for(int i = left; i <= right; i++) {
            result.push_back(matrix[top][i]);
        }
        top++; // Move top boundary down

        // Traverse from top to bottom on the right column
        for(int i = top; i <= bottom; i++) {
            result.push_back(matrix[i][right]);
        }
        right--; // Move right boundary left

        // Check if there are rows remaining
        if(top <= bottom) {
            // Traverse from right to left on the bottom row
            for(int i = right; i >= left; i--) {
                result.push_back(matrix[bottom][i]);
            }
            bottom--; // Move bottom boundary up
        }

        // Check if there are columns remaining
        if(left <= right) {
            // Traverse from bottom to top on the left column
            for(int i = bottom; i >= top; i--) {
                result.push_back(matrix[i][left]);
            }
            left++; // Move left boundary right
        }
    }

    // Return the final spiral order
    return result;
}
```
	
### Count subarrays with given sum

- Given an array of integers and an integer k, return the total number of subarrays whose sum equals k. A subarray is a contiguous non-empty sequence of elements within an array.
    - Input: N = 4, array[] = {3, 1, 2, 4}, k = 6, Output: 2
    - Input: N = 3, array[] = {1,2,3}, k = 3, Output: 2

- Brute in cubic. One for starting point of array. One for ending point and the third for sum of subarray.
- Better will be optimize this third loop and keep track of count in second loop only in quadratic time only.
- Optimal is below:

```cpp
// In this approach, we are going to use the concept of the prefix sum to solve this problem. Here, the prefix sum of a subarray ending at index i simply means the sum of all the elements of that subarray.

// Assume, the prefix sum of a subarray ending at index i is x. In that subarray, we will search for another subarray ending at index i, whose sum equals k. Here, we need to observe that if there exists another subarray ending at index i with sum k, then the prefix sum of the rest of the subarray will be x-k. The below image will clarify the concept:

// Now, for a subarray ending at index i with the prefix sum x, if we remove the part with the prefix sum x-k, we will be left with the part whose sum is equal to k. And that is what we want. Now, there may exist multiple subarrays with the prefix sum x-k. So, the number of subarrays with sum k that we can generate from the entire subarray ending at index i, is exactly equal to the number of subarrays with the prefix sum x-k, that we can remove from the entire subarray.

// That is why, instead of searching the subarrays with sum k, we will keep the occurrence of the prefix sum of the subarrays using a map data structure. 

// In the map, we will store every prefix sum calculated, with its occurrence in a `<key, value>` pair. Now, at index i, we just need to check the map data structure to get the number of times that the subarrays with the prefix sum x-k occur. Then we will simply add that number to our answer.

// We will apply the above process for all possible indices of the given array. The possible values of the index i can be from 0 to n-1(where n = size of the array)
// First, we will declare a map to store the prefix sums and their counts.
// Then, we will set the value of 0 as 1 on the map.
// Then we will run a loop(say i) from index 0 to n-1(n = size of the array).
// For each index i, we will do the following:
// We will add the current element i.e. arr[i] to the prefix sum.
// We will calculate the prefix sum i.e. x-k, for which we need the occurrence.
// We will add the occurrence of the prefix sum x-k i.e. mpp[x-k] to our answer.
// Then we will store the current prefix sum in the map increasing its occurrence by 1.

// Question: Why do we need to set the value of 0?
// Let’s understand this using an example. Assume the given array is [3, -3, 1, 1, 1] and k is 3. Now, for index 0, we get the total prefix sum as 3, and k is also 3. So, the prefix sum of the remove-part should be x-k = 3-3 = 0. Now, if the value is not previously set for the key 0 in the map, we will get the default value 0 for the key 0 and we will add 0 to our answer. This will mean that we have not found any subarray with sum 3 till now. But this should not be the case as index 0 itself is a subarray with sum k i.e. 3.
// So, in order to avoid this situation we need to set the value of 0 as 1 on the map beforehand.

int subarraySum(vector<int>& arr, int k) {
    // Size of the array
    int n = arr.size();

    // Map to store frequency of prefix sums
    unordered_map<int, int> prefixSumCount;

    // Initialize prefix sum and count of subarrays
    int prefixSum = 0;
    int count = 0;

    // Base case: prefix sum 0 has occurred once
    prefixSumCount[0] = 1;

    for (int i = 0; i < n; i++) {
        prefixSum += arr[i];

        // Calculate the prefix sum that needs to be removed
        int remove = prefixSum - k;

        // If this prefix sum has been seen before,
        // add its count to the result
        if (prefixSumCount.find(remove) != prefixSumCount.end()) {
            count += prefixSumCount[remove];
        }

        // Update the frequency of the current prefix sum
        prefixSumCount[prefixSum]++;
    }

    // Return the total count of subarrays
    return count;
}
```


### ***Pascal's Triangle I***

- Write a program to generate Pascal's triangle. In Pascal’s triangle, each number is the sum of the two numbers directly above it as shown below.
    ```
    1
    11
    121
    1331
    14641
    ```

- Algorithm 1: Time Complexity: O(N^2), we generate all the elements in first N rows sequentially one by one.
    - To generate the entire Pascal’s Triangle for the first N rows, we can start with the first row containing a single 1 and iteratively build each subsequent row using the property that every element (except the first and last) is the sum of the two elements directly above it from the previous row. The first and last elements of each row are always 1. By storing the previous row, we can calculate the next row easily. This process continues until we have constructed all N rows, resulting in the complete Pascal’s Triangle structure.
    ```cpp
    vector<vector<int>> generate(int numRows) {
        // Result vector to hold all rows
        vector<vector<int>> triangle;

        // Loop for each row
        for (int i = 0; i < numRows; i++) {
            // Create a row with size (i+1) and initialize all elements to 1
            vector<int> row(i + 1, 1);

            // Fill elements from index 1 to i-1 (middle values)
            for (int j = 1; j < i; j++) {
                // Each element = sum of two elements above it
                row[j] = triangle[i - 1][j - 1] + triangle[i - 1][j];
            }

            // Add current row to the triangle
            triangle.push_back(row);
        }
        return triangle;
    }
    ```
	
- Algorithm 2: Time Complexity: O(N), we iterate N times to compute each element of the row in O(1) time using the direct relation.
    - To print the Nth row of the pascal triangle we can take advantage of the relationship between Nth element and binomial coefficients. In a pascal's triangle, the Nth row contains the binomial coefficients C(N-1, 0), C(N-1, 1) and so on till C(N-1, N-1). Thus we can simply calculate all these values to return the Nth row of pascal triangle. Instead of computing full factorials, we can start with the first value as 1, and use the relation C(n, k) = C(n, k−1) × (n−k+1) / k to compute the next value from the previous one in constant time.
    ```cpp
    vector<long long> getNthRow(int N) {
        // Result vector to store the row
        vector<long long> row;
        
        // First value of the row is always 1
        long long val = 1;
        row.push_back(val);
        
        // Compute remaining values using the relation:
        // C(n, k) = C(n, k-1) * (n-k) / k
        for (int k = 1; k < N; k++) {
            val = val * (N - k) / k;
            row.push_back(val);
        }
        
        return row;
    }
    ```

- Algorithm 3: Time Complexity: O(min(c,r−c)), The loop runs for min(c−1,r−c) iterations because binomial coefficients are symmetric.
    - To find the element at the coordinates (R,C) where R is the row number and C is the Column number, we can simply simulate the generation of pascal's triangle for R rows. In Pascal’s Triangle, the element at row R and column C corresponds to the binomial coefficient (r-1)C(c-1). To calculate this binomial coefficient, we can simply apply the formula of binomial coefficient i.e. (r-1)!/(c-1)!(r-c)!. Instead of computing full factorials (which can overflow and be slow), we can multiply and divide in a loop to compute the coefficient efficiently.
    ```cpp
    long long findPascalElement(int r, int c) {
        // Element is C(r-1, c-1)
        int n = r - 1;
        int k = c - 1;

        long long result = 1;

        // Compute C(n, k) using iterative formula
        for (int i = 0; i < k; i++) {
            result *= (n - i);
            result /= (i + 1);
        }

        return result;
    }
    ```

### ***Majority Element-II***

- Find the elements that appears more than N/3 times in the array. Given an integer array nums of size n. Return all elements which appear more than n/3 times in the array. The output can be returned in any order.
    - 1 2 1 1 3 2 output 1
    - 1 2 1 1 3 2 2 output 1 2
    - Note that mathematically maximum two output is possible.

- Brute in quadratic. Check every element against every others for count.
- Better after sorting in nlogn. Or using map will make it linear but will take space also.
- Optimal in linear time and constant space.
    - Initialize four variables: cnt1 and cnt2 for tracking the counts of elements, and el1 and el2 for storing the potential majority elements.
    - Traverse through the given array:
        - If cnt1 is 0 and the current element is not equal to el2, set el1 to the current element and increment cnt1 by 1.
        - If cnt2 is 0 and the current element is not equal to el1, set el2 to the current element and increment cnt2 by 1.
        - If the current element is equal to el1, increment cnt1 by 1.
        - If the current element is equal to el2, increment cnt2 by 1.
        - In all other cases, decrease cnt1 and cnt2 by 1.
    - After processing all elements, el1 and el2 should be the candidate elements for majority. To confirm:
        - Use another loop to manually check the counts of el1 and el2 in the array.
        - If either el1 or el2's count is greater than floor(N/3), it is considered a valid majority element.

    ```cpp
    vector<int> majorityElementTwo(vector<int>& nums) {
        int n = nums.size(); 
        int cnt1 = 0, cnt2 = 0;
        int el1 = INT_MIN, el2 = INT_MIN;

        /*Find the potential candidates using Boyer Moore's Voting Algorithm*/
        for (int i = 0; i < n; i++) {
            if (cnt1 == 0 && el2 != nums[i]) {cnt1 = 1; el1 = nums[i];}
            else if (cnt2 == 0 && el1 != nums[i]) {cnt2 = 1; el2 = nums[i];} 
            else if (nums[i] == el1) {cnt1++;} 
            else if (nums[i] == el2) {cnt2++;} 
            else {cnt1--; cnt2--;}
        }

        //Validate the candidates by counting occurrences in nums. Reset counts for el1 and el2
        cnt1 = 0, cnt2 = 0; 
        for (int i = 0; i < n; i++) {
            if (nums[i] == el1) {cnt1++;}
            if (nums[i] == el2) {cnt2++;}
        }

        /* Determine the minimum count required for a majority element*/
        int mini = n / 3 + 1;
        vector<int> result;
        if (cnt1 >= mini) {result.push_back(el1);}
        if (cnt2 >= mini && el1 != el2) {result.push_back(el2);}

        // Uncomment the following line if you want to sort the answer array
        // sort(result.begin(), result.end()); // TC --> O(2*log2) ~ O(1);

        return result;
    }
    ```

### 3 Sum

- Given an array of N integers, your task is to find unique triplets that add up to give a sum of zero. In short, you need to return an array of all the unique triplets [arr[a ], arr[b ], arr[c ]] such that i!=j, j!=k, k!=i, and their sum is equal to zero.

- 

### 4 Sum
	
### Largest Subarray with Sum 0
	
### Count subarrays with given xor K
	
<!-- ### Merge Overlapping Subintervals -->

### ***Merge two sorted arrays without extra space***

- Given two sorted integer arrays nums1 and nums2, merge both the arrays into a single array sorted in non-decreasing order. The final sorted array should be stored inside the array nums1 and it should be done in-place. Array nums1 has a length of m + n, where the first m elements denote the elements of nums1 and rest are 0s whereas nums2 has a length of n.

- Since both arrays are sorted in non-decreasing order, the largest elements will be at the end of each array. If we start comparing elements from the back of both arrays and place the largest one at the end of nums1, we won't need to shift anything. To efficiently insert elements at the end, we will use three pointers.
    - Initialize three pointers: One points at the last valid index (excluding zeros) of nums1, one points at the last valid index of nums2 andd the last pointer points to last index of nums1.
    - Compare the elements pointed by the first two pointers and whichever is larger, place it at the third pointer's index.
    - Move the respective pointer one step back and also move the third pointer one step back.
    - If there are any remaining elements in nums2, then copy them in nums1. If any elements remain in nums1, they’re already in place
    - The result is a fully merged and sorted array stored in nums1 itself.

    ```cpp
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;

        // Fill nums1 from the end by comparing nums1[i] and nums2[j]
        while (i >= 0 && j >= 0) {
            // Place larger of the two at nums1[k]
            if (nums1[i] > nums2[j]) {
                nums1[k--] = nums1[i--];
            } else {
                nums1[k--] = nums2[j--];
            }
        }

        // If nums2 has remaining elements, copy them
        while (j >= 0) {
            nums1[k--] = nums2[j--];
        }
        // No need to copy remaining nums1 elements, as they are already in place
    }
    ```

### Find the repeating and missing number
	
### Count Inversions
	
### Reverse Pairs
	
### Maximum Product Subarray in an Array

