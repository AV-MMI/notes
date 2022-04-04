# Divide and Conquer Algorithm.

 The Divide and Conquer algorithm is an algorithm that help us solve problems decomposing them into sub-problems, so we can solve each sub-problem and then merge them all to solve the main problem.
 
 The DaC approach consist of three steps:
 
**Divide:**

 Divide the main problem into sub-problems.
 
**Conquer:**

 Solve each sub-problem.
 
**Combine**
 Put together all solved sub-problems to solve the main problem.
 
 An example of an algorithm using the Divide and Conquer approach is the *merge sort*. This algorithm works dividing the input array recursively into two halves until we get individual elements.
 
 ![merge-sort](../img/divide-and-conquer-2.webp)
 
 Then we proceed to sort each element in an incremental way before we put them all together. In this case, the **conquer** and **combine** steps go side by side.
 
 ![merge-sort result](../img/divide-and-conquer-3.webp)
 
 There is another approach that we can use to solve this type of problems. This approach is the **Dynamic approach**. It works like the divide and conquer approach, it divides a problem into various sub-problems, then it solve those sub-problems and merge them all to get the solution for the main problem, until this point it is not different of DAC, but the thing that make it different to the DAC is that the solution of each sub-problem is stored in the Dynamic approach. So if in the future a sub-problem that we have already solved is passed into our function, instead of computing the solution we just have to retrieve it from the place where we stored it.
 
 We also should keep in mind that the Dynamic approach is not always recursive, usually, loops are preferred when using dynamic programming.
 
 ![Difference between DAC and Dynamic](../img/difference-dac-and-dynamic.svg)