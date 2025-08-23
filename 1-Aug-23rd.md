# Aug 23rd Practice Problems


1. Longest substring (uses sliding window with two pointers and hashset)
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
