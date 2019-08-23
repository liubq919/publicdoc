
Problem
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

Example:

    Input: "babad"

    Output: "bab"

    Note: "aba" is also a valid answer.
    Example:

    Input: "cbbd"

    Output: "bb"

Program Structure:
```java
public class Solution {
    public String longestPalindrome(String s) {

    }
}
```

Solution
Each character is considered as the mid of a palindrom string and the largest palindrom is checked.

```java
public class Solution {
    private int lo, maxLen;

    public String longestPalindrome(String s) {
        int len = s.length();
        if (len < 2) return s;

        for (int i = 0; i < len - 1; i++) {

            // assume odd length, try to extend Palindrome as possible
            extendPalindrome(s, i, i);

            // assume even length.
            extendPalindrome(s, i, i + 1);
        }
        return s.substring(lo, lo + maxLen);
    }

    private void extendPalindrome(String s, int j, int k) {

        while (j >= 0 && k < s.length() && s.charAt(j) == s.charAt(k)) {
            j--;
            k++;
        }

        if (maxLen < k - j - 1) {
            lo = j + 1;
            maxLen = k - j - 1;
        }
    }
}
```

Time Complexity: O(N^2)

Consider N as the length of the string, the extendPalindrome() method is called 2N times.

The complexity of extendPalindrome() itself is O(N) which makes the total time complexity as O(2N^2)=O(N^2).

Space Complexity: O(1)

This algorithm uses a constant extra space.

Difficulty: Medium

