Problem:

Given an array S of integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triples in the array which gives the sum of zero.

Note:
The solution set must not contain duplicated triples.

For example:

given array S = [-1, 0, 1, 2, -1, -4]

A solution set is:
[
    [-1, 0, 1],
    [-1, -1, 2]
]

Program Structure:
```java
public class Solution {
    public List<List<Integer>> threeSum(int[] sums) {

    }
}
```

Solution:

The idea is to sort an input array and then run through all indices of a possible first element of a triplet. For each possible first element we make a standard bi-directional 2Sum sweep of the remaining part of the array. Also we want to skip equal emelemts to avoid duplicates in the answer.

```java
public class Solution {
    public List<List<Integer>> threeSum(int[] num) {

        Arrays.sort(num);

        List<List<Integer>> res = new LinkedList< List< Integer > > ();

        for(int i = 0; i < num.length -2; i++) {
            if ( i == 0 ||  num[i] != num[i - 1]) {
                int lo = i + 1, hi = num.length - 1, sum = 0 - num[i];

                while (lo < hi) {
                    if (num[lo] + num[hi] == sum) {
                        res.add( Arrays.asList(num[i], num[lo], num[hi]));
                        while(lo < hi && num[lo] == num[lo + 1]) lo++;
                        while(lo < hi && num[hi] == num[hi - 1]) hi--;
                        lo++;
                        hi--;
                    } else if (num[lo] + num[hi] < sum) lo++;
                    else hi--;
                }
            }
        }

        return res;
    }
}
```

Time complexity: O(N ^ 2)
All elements in the list are processed for every index.

Difficulty: Medium