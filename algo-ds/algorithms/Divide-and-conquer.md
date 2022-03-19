# Divide and Conquer.


## Concept

 This algorithm recursively breaks down a problem into two or more sub-problems of the same or related type, until these problems become simpler enough to be solved directly, then the solutions to the sub-problems are combined to solve the original problem.
 
## Examples

### Merge sorge algorithm.

![Merge sort](../img/merge-sort-diagram.png)

 Merge sort is one of the most popular sorting algorithms, and is based in the **Divide and Conquer algorithm**, so, basically we divide our problem into multiple sub-problems, then each sub-problem is solved individually and then all the sub-problems are combined to form the solution for our problem.

 
 Let's suppose we have an array `nums` to sort. A sub-problem would be to sort a sub-section of this array, starting at index `a`, and ending at index `c`, denoted as `ǹums[a...c]`.
 
#### Divide

 If `b` is the middle point between `a` and `c`, then we can split the sub-array`nums` array into two sub-arrays
`nums[a, b]`, and `nums[b+1, c]`.

#### Conquer

 Now we try to sort both sub-arrays `nums[a, b]` and `nums[b+1, c]` If we haven't yet reached the base case, we continue dividing both sub-arrays until we can sort them.
 
 
#### Combine

 When our conquer step is finalized (meaning that we have divided our array into two sub-arrays --or more--, and we already sorted them), we combine the results by creating a sorted array `nums[a, c]` from the two sorted sub-arrays `nums[a, b]` and `nums[b+1, c]`.
 
### Problem: Maximum Subarray Sum.
 