# Sept 1st Practice Problems
## Notes
- Read questions properly and see what I'm allowed to use!!

---
## Problems
1. Product of array except itself (WITH division)

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

2. Product of array except itself (WITHOUT division)

**Time complexity:** `O(n)`

```
class Solution {
    public int[] productExceptSelf(int[] nums) {
        if (nums.length == 0) return nums; // empty array

        int[] prefix = new int[nums.length];
        int[] postfix = new int[nums.length];

        // Create prefix array
        prefix[0] = nums[0];
        for (int i = 1; i < nums.length; i++) {
            prefix[i] = prefix[i - 1] * nums[i];
        }

        // Create postfix array
        postfix[nums.length - 1] = nums[nums.length - 1];
        for (int i = nums.length - 2; i >= 0; i--) {
            postfix[i] = postfix[i + 1] * nums[i];
        }

        // Compute product
        for (int i = 0; i < nums.length; i++) {
            int product = 1;
            if (i - 1 >= 0) {
                product *= prefix[i - 1];
            }
            if (i + 1 < nums.length) {
                product *= postfix[i + 1];
            }

            nums[i] = product;
        }

        return nums;
    }
}
```

3. 

**Time complexity:** ``

```

```
