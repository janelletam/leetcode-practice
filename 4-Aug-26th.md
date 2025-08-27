# Aug 26th Practice Problems
## Notes
- Set timer for problems!!
- Use debugging statements generously - they save a lot of time in the long run especially for tricky bugs

---
## Problems
1. Num of palindromic substrings - solved by identifying palindromes treating each char as the center

Solved in ~17 mins.
**Time complexity:** `O(n^2)`

```
class Solution {
    public int countSubstrings(String s) {
        // do pointer approach where for each char in s, count the
        // number of palindromic substrings that can be formed by it
        int numSubstrings = 0;

        for (int center = 0; center < s.length(); center++) {
            
            int counter = 0;
            // Count odd palindromes (with i = center)
            while ((center - counter) >= 0 && (center + counter) < s.length() && s.charAt(center - counter) == s.charAt(center + counter)) {
                numSubstrings++;
                counter++;
            }

            // Count even ppalindromes (start with left pointer = center)
            int leftPointer = center;
            int rightPointer = center + 1;
            while (leftPointer >= 0 && rightPointer < s.length() && s.charAt(leftPointer) == s.charAt(rightPointer)) {
                numSubstrings++;
                leftPointer--;
                rightPointer++;
            }
        }

        return numSubstrings;
    }
}
```

2. 

**Time complexity:** ``

```

```

3. 

**Time complexity:** ``

```

```
