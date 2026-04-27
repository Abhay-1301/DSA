# Part 1: Learning. Introduction to Priority Queues using Binary Heaps

- Heaps or Priority Queue Theory:
    - Heaps are also called priority queue.
    - Special queue where elements are processed according to priority.
    - Useful in task scheduling, path finding algorithm and real time systems.
- Binary Heap:
    - Binary tree which is complete (all level completely filled except last level and last level is such that all keys are as left as possible) and should satisfy heap property (if min heap, smallest root node and similar for max).
    - A binary heap is represented as an array.
        - Root value at arr[0]
        - Node index = i ; Left = 2i+1 ; Right = 2i+2 ; Parent = (i-1) / 2
- Opertion on Min Heap
    - Insert: 



# Heaps

# Part 1: Learning


Heaps Theory
Operations Associated with Min Heap (log N everything except get min)
Insert
First insert at last position
Check with its parent if its smaller. If not swap. Check again at parent node.
Thumb rule is to check the complete binary tree property and the min heap property.
Heapify
Fix the violation at node I to restore min heap.
Violation means min heap property violation.
Assumption is subtree of this node are valid binary heap.
getMin
Return root node or arr[0]
ExtractMin
Removes minimun element from heap.
Copy the last element to root node, decrease size and call heapify at root node.
Delete
Delete value at given index
Update the value with INTMIN
Call decrease key which will force this to go at top
Now call extract min
DecreaseKey
Update value at a given node.
Min heap property may get violated. If new val is less than previously existing value, min heap property is not violated on subtree of this node. But it may get violated in ancestors so swap the nodes and recursively check again.
Convert min heap to max heap





# Part 2: Medium Problems



MY THINKING: 
JUDGING MY THINKING: 
BRUTE FORCE: 
OPTIMAL: 
NOTES:




Kth largest/smallest element in an array


My thinking: The brute force solution is to sort the array and directly find the kth largest or smallest in n log N time. But this can be solved using min or max heap. Core idea is to maintain a min or max heap of size k. Insert all elements and then return the root node. This will take klogn time.
Judging My thinking: This is completely wrong approach. 
Actual Brute Force Solution:
For largest use Minheap
Push first K element
For remaining element if it’s greater than root/top element, pop and push.
Return top
TC: nlogk, SC: k
Actual Optimal Solution without using heaps in O(N)
<SOLUTION USES TWO POINTERS. SEE IT LATER.>
Any notes:
You have priority queue data structure in C++. You do not have to think in terms of implementation of max-min heap insertions deletion updation. Just think in terms of insert pop etc.
Actual Brute Force Solution made sense while dry running on notebook but not convinced how it’s working so I might forget. (I think the heap is storing Kth largest to largest till the given index in which the Kth largest is minimum after whole iteration of array.)
Min heap initialization: priority_queue <int, vector<int>, greater<int>> pq; Max heap initialization: priority_queue<int> maxHeap


SORT K SORTED ARRAY


MY THINKING: Problem was that element is at I-k to I+k position from its sorted position so brute force is just to do sort. Optimal will be using some heap shit, but I was not able to think which leads to notes.
JUDGING MY THINKING: Brute force is correct. Optimal couldn’t think so read notes why and read optimal for solution.
BRUTE FORCE: Equal to my thinking.
OPTIMAL:
Thing is we have to leverage this K algorithmically.
So think like this, the smallest element will be in k+1 window. Find it using Minheap and place it at first. Then move to next window from 1 to k+2 and so on.
priority_queue<int, vector<int>, greater<int>> minHeap; vector<int> result; 
for (int i = 0; i <= k && i < arr.size(); i++) minHeap.push(arr[i]);
for (int i = k + 1; i < arr.size(); i++) result.push_back(minHeap.top()); minHeap.pop(); minHeap.push(arr[i]);
while (!minHeap.empty()) result.push_back(minHeap.top()); minHeap.pop();
return result;
NOTES:
Whenever this K shit comes, think of heap. It does not change things much but just makes complexity from nlogn to nlogk.


Replace elements by their rank


MY THINKING: Brute force starts from sorting the array. After that easily doable. What’s optimal if learning min or max heap?
JUDGING MY THINKING: No any solution using heap. My thought is optimal. Brute force was n^2 by checking for each element how many others are smaller.
BRUTE FORCE: NA
OPTIMAL: NA
NOTES: Just see some function used to brush up syntax.
sort(sortedArr.begin(), sortedArr.end());
unordered_map<int, int> rankMap;
for (int num : sortedArr)
if (rankMap.find(num) == rankMap.end())


Task scheduler


MY THINKING: 
JUDGING MY THINKING: 
BRUTE FORCE: 
OPTIMAL: 
NOTES:
While solving this question I got a thought that somehow queue must be used and then I was like no it’s a question of heap. Then it striked my mind that heap is priority queue.
Think or have a mental model also in terms of priority queue where it is queue and it is processed based on some priority and not heaps. 


Hand of straights
Merge K sorted list



# Part 3: Hard Problems


Design Twitter
Minimum cost to connect sticks
Kth largest element in a stream of running integers
Maximum sum combination
Find median from data stream
Top K Frequent Elements

