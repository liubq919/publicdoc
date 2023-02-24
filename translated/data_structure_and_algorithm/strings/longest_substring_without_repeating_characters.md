Problem:
Given a string, find the length of the longest substring without repeating characters.

Examples:

    Given "abcabcbb", the answer is "abc", which the length is 3.

    Given "bbbbb", the answer is "b", with the length of 1.

    Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.


Program Structure:
```java
public class Solution {
  public int lengthOfLongestSubstring(
      String s) {

  }
}
```

Solution:
The basic idea is, keep a hashmap which stores the characters in string as keys and their positions as values,
and keep two pointers which define the max substring.

Move the right pointer to scan through the string, and meanwhile update the hashmap.

If the character is already in the hashmap, then move the left pointer to the right of the same character last found.

Note that the two pointers can only move forward.

```java
    public static int lengthOfLongestSubstring(String s) {
        if (s.length() == 0)
            return 0;

        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        int max = 0;

        for (int i = 0, j = 0; i < s.length(); ++i) {

            if (map.containsKey(s.charAt(i))) {
                j = Math.max(j, map.get(s.charAt(i)) + 1);
            }
            map.put(s.charAt(i), i);
            max = Math.max(max, i - j + 1);
        }

        return max;
    }
```

Time Complexity: O(N)

This solution runs in O(N) time complexity where N is the size of the string.

Space Complexity: O(N)

This algorithm uses an extra space of size N.

Difficulty: Medium