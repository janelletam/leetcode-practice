# Sept 1st Practice Problems
## Notes
- 

---
## Problems
1. Product of array except itself

**Time complexity:** `O(n)`

```
class Solution {
    public int[] productExceptSelf(int[] nums) {
        // Solution: compute total product (don't add 0)
        // Divide what num[i] is
        // Edge case: if num[i] == 0, then everything else is equal to 0
        // num[i] == the total product

        int idxZero = -1; // stores if index has zero or not
        boolean hasMoreThanOneZero = false; // stores if there are multiple has zero indices
        int totalProduct = 1;

        int i = 0;

        while (i < nums.length) {
            if (nums[i] != 0) {
                totalProduct *= nums[i];
            } else if (idxZero == -1) {
                idxZero = i;
            } else {
                hasMoreThanOneZero = true;
                totalProduct = 0;
                break;
            }
            i++;
        }

        for (int j = 0; j < nums.length; j++) {
            // No zeroes, proceed normally
            if (idxZero == -1) {
                nums[j] = totalProduct / nums[j];
            }
            // Only one zero
            else if (!hasMoreThanOneZero) {
                if (j != idxZero) nums[j] = 0;
                else nums[j] = totalProduct;
            }
            // Multiple zeroes
            else {
                nums[j] = 0;
            }
        }
        
        return nums;
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
