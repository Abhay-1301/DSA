# BS

## BS on 1D Arrays

- ### Binary Search
    - You are given a sorted array of integers and a target, your task is to search for the target in the given array. Assume the given array does not contain any duplicate numbers.
    - Algorithm (Iterative):
        ```cpp
        int binarySearch(vector& nums, int target) {
            int n = nums.size();
            int low = 0, high = n - 1;

            while (low <= high) {
                int mid = (low + high) / 2;
                if (nums[mid] == target) return mid;
                else if (target > nums[mid]) low = mid + 1;
                else high = mid - 1;}
            return -1;}
        ```
    - Algorithm (Recursive):
        ```cpp
        int binarySearch(vector& nums, int low, int high, int target) {
            if (low > high) return -1;

            int mid = (low + high) / 2;
            if (nums[mid] == target) return mid;
            else if (target > nums[mid]) return binarySearch(nums, mid + 1, high, target);
            return binarySearch(nums, low, mid - 1, target);}

        int search(vector& nums, int target) {
            return binarySearch(nums, 0, nums.size() - 1, target);
        }
        ```

- ### Implement Lower Bound
    - Given a sorted array of N integers and an integer x, write a program to find the lower bound of x. The lower bound algorithm finds the first or the smallest index in a sorted array where the value at that index is greater than or equal to a given key i.e. x. The lower bound is the smallest index, ind, where arr[ind] >= x. But if any such index is not found, the lower bound algorithm returns n i.e. size of the given array.
        - Example 1: Input Format: N = 4, arr[] = {1,2,2,3}, x = 2 Result: 1 Explanation: Index 1 is the smallest index such that arr[1] >= x.
        - Example 2: Input Format: N = 5, arr[] = {3,5,8,15,19}, x = 9 Result: 3 Explanation: Index 3 is the smallest index such that arr[3] >= x.
    - Apply Binary Search

- ### Upper Bound

- ### Search Insert Position
    - You are given a sorted array arr of distinct values and a target value x. You need to search for the index of the target value in the array.

- ### Floor and Ceil in Sorted Array
    - You're given an sorted array arr of n integers and an integer x. Find the floor and ceiling of x in arr[0..n-1]. The floor of x is the largest element in the array which is smaller than or equal to x. The ceiling of x is the smallest element in the array greater than or equal to x

- ### Last occurrence in a sorted array
    - Given a sorted array of N integers, write a program to find the index of the last occurrence of the target key. If the target is not found then return -1. Note: Consider 0 based indexing

- ### Count Occurrences in Sorted Array
    - You are given a sorted array containing N integers and a number X, you have to find the occurrences of X in the given array.

- ### Search Element in a Rotated Sorted Array
    - Given an integer array nums, sorted in ascending order (with distinct values) and a target value k. The array is rotated at some pivot point that is unknown. Find the index at which k is present and if k is not present return -1.

- ### Search Element in Rotated Sorted Array II
    - Given an integer array arr of size N, sorted in ascending order (may contain duplicate values) and a target value k. Now the array is rotated at some pivot point unknown to you. Return True if k is present and otherwise, return False.

- ### a