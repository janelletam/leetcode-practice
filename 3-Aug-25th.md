# Aug 25th Practice Problems
## Notes
- 

---
## Problems
1. Reverse string in place (easy)

**Time complexity:** `O(n)`
**Space complexity:** `O(1)` - I just use a few extra vars

```
class Solution {
    public void reverseString(char[] s) {
        int left = 0;
        int right = s.length - 1;
        char temp;

        while (left < right) {
            temp = s[left];
            s[left] = s[right];
            s[right] = temp;

            left++;
            right--;
        }
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
