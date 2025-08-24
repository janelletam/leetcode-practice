# Aug 23rd Practice Problems
## Notes:
- To calculate length of substring, use `right - left + 1`

---
## Problems:
1. Longest substring (uses sliding window with two pointers and hashset, check to see if currChar is in hashset and update pointers accordingly)

**Time complexity:** `O(n)`
```
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int maxLength = 0, left = 0;
        HashSet<Character> charSet = new HashSet<>();

        for (int right = 0; right < s.length(); right++) {
            while (charSet.contains(s.charAt(right))) {
                charSet.remove(s.charAt(left++));
            }
            charSet.add(s.charAt(right));
            maxLength = Math.max(maxLength, right - left + 1);
        }
        return maxLength;
    }
}
```

2. Longest palindromic substring (calculate the odd and even palindromes using the current index as the center

**Time complexity:** `O(n^2)`
```
class Solution {
    public String longestPalindrome(String s) {
        int maxLength = 0;
        String result = "";

        for (int i = 0; i < s.length(); i++) {
            int left = i, right = i;
            
            // Odd palindromes
            while(left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
                if ((right - left + 1) > maxLength) {
                    maxLength = right - left + 1;
                    result = s.substring(left, right + 1);
                }
                
                left--;
                right++;
            }

            // Even palindromes
            left = i; right = i + 1;
            while(left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
                if ((right - left + 1) > maxLength) {
                    maxLength = right - left + 1;
                    result = s.substring(left, right + 1);
                }
                
                left--;
                right++;
            }
        }

        return result;
    }
}
```
