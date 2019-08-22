Problem:

Given an array and a value, remove all instances of that value in place and return the new length.

Do not allocate extra space for another array, you must do this in palce with constant memory.

The order of elements can be changed. It doesn't matter what you leaved beyond the new length.

Example:

Given input array nums = [3, 2, 2, 3], val = 3

Your function should return length = 2, with the first two elements of nums being 2.

Program structure:

```java
public class Solution{
    public int removeElement(int[] nums, int val) {

    }
}
```

Solution:

We need to have two indices(fast and slow) and iterate through the arrar elements to check if the elements pointed by the fast index is as same as the given value. If different, replace the slow index value with the fast index value. If the values are the same, skip it and move to the fast index to tne next element. When the fast element reaches the end, return the slow value as the nnumber of elements in the resulting array.

```java
public class Solution{

    public int removeElement(int[] A, int num)
    {

        /* handle empty list */
        if ( A.length == 0 ) return 0;

        int slow = 0;

        for ( int fast = 0; fast < A.length; fast++ )
        {
            if(A[fast] != num)
                A[slow++] = A[fast];
        }

        return slow;
    }
}
```

Time complexity: O(N)

This solution run in O(N) time complexity where N is the size of the array.

Space Complexity: O(1)

This algorithm uses a constant extra space.

Difficulty: Easy