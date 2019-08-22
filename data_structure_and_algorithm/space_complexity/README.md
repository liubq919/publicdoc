## Space Complexity

### Introduction
Space Complexity refers to the magnitude of auxiliary space your program takes to process the input, ignoring the space used by the input to the program.

There are broadly two kinds of algorithms we have to calculate the space complexity for:

#### 1. Iterative Algorithms
For iterative algorithms we have to consider the variables and data structures we declare in our program. To be more general, if we reserve any memory apart from the input, it counts towards the space complexity.

For example declaring an array of size n would add to the space complexity by a factor of O(n), a 2D array would add to the space complexity by a factor of O(n^2) and so on. Simple variables add a space complexity of O(1) since the size of a particular datatype is always constant.

**The more trickier cases are the user defined datatypes, however in most cases iterative algorithms are straightforward to analyze in terms of space complexity.**

#### 2.	Recursive Algorithms
These algorithms are trickier because in addition to taking care of the variables and data structures (memory reserved for operations in the heap), we also have to take care of the stack memory which is used for recursive calls.

For example, in the Fibonacci algorithm we have F(n) = F(n-1) + F(n-2), n>2. If we trace out the recursive calls we see that F(4) calls F(3) and F(2). F(3) calls F(2) and F(1) which return base cases. So at a given point of time we see that the program does not occupy more than n cells in the stack memory for a function call of F(n). This will be much easier to understand if you draw the function calls on a piece of paper and push each driver onto the stack before descending to a lower level of recursion.

Hence we can safely state that for this particular algorithm, we have a space complexity of O(n) (assuming that each cell in the stack reserves equal space).