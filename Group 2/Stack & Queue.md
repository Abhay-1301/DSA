# Part 1 - Learning

First there is implementation problem.

- Implement Stack using Arrays
- Implement Queue using Arrays
- Implement Stack using Queue
- Implement Queue using Stack
- Implement Stack using LinkedList
- Implement Queue using LinkedList
- Balanced Paranthesis
- Implement Min Stack

These problems I think I can do. I will most probably be stuck in coding part because I have not coded in a while without AI. So lets see the last question because I do not know what it is. If I get time to code we will code all other questions later.

So the last question is just simple that we have to Implement stack but I have to write a function getMin() which retrieves the minimum element in the stack. It has brute force as well as optimal approach. I can think of brute force solution by using O(N) extra space by maintaining another stack which in filled with the old stack.top and we track min.

But no here the brute force implementaion is to store min element along with value as pair in stack while inserting.

The optimal approach has some formula 2*min-x and 2*value-min so read this. This is new. 

At the end code all question for confidence in coding.

# Part 2 - Prefix, Infix, Postfix conversion

- Infix to Postfix
- Infix to Prefix
- Prefix to Infix
- Prefix to Postfix
- Postfix to Prefix
- Postfix to Infix

# Part 3 - Monotonic Stack Queue Problem

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

# Part 4 - Implementation Problem

- Sliding Window Maximum: Given arr[] and a value k. For a sliding window of size k, find maximum in all subarray, make an array of it and return it. But what is optimal way of solving this ? 

- Stock Span Problem: 

- Celebrity Problem:

- LRU Cache:

- LFU Cache: 