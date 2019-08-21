problem:  
given  an array of integers, return indices of the two numbers such that they add up to a specific target.

you may assume that each input would have exactly one solution, and you may not use the same element twice.

example：
Given nums = [2, 77, 11, 15], target = 9

because nums[0] + nums[1] == 2 + 7 = 9
return [0, 1]

program structure:

```java
public class Solution{
    public int[] twoSum(int[] nums, int target) {}
}
```

solution:  
we can solve this problem with O(N) time complexity using a HashMap.

```java

public int[] twoSum(int[] nums, int target) {

    /* array to return result */
    int[] arr = new int[2];

    /* map for num and index pair */
    Map<Integer, Integer> map = new HashMap<Integer, Integer>();

    /* iterate through the array */
    for (int i = 0; i < nums.length; i++) {
        /* check if the map has an element
        * which is equal to the difference
        * between the target and current element
        */

        Integer val = map.get(target - nums[i]);

        if (val == null) {

            /* no match found, add the current item and index to map*/
            map.put(nums[i], i)
        } else {

            /* match found, update the index values */
            arr[0] = val;
            arr[1] = i;
            break;
        }
    }

    return arr;
}
```

Time complexity: O(N)

The array is traversed only once. So the time complexity is directly proportional to the size to the array. i.e. O(N)

Space complexity: O(N)

O(1) + O(N) = O(N). O(1) for the variables and O(N) for the hashmap. For hashmap, with the increase of the number of entries, the haspmap's space will increase linearly. So apace complexity of hashmap is O(N).

Difficulty: Easy
