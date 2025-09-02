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

3. House robber

**Time complexity:** `O(n)`

```
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) return nums[0];
        if (nums.length == 2) return Math.max(nums[0], nums[1]); // base cases

        return Math.max(robLinear(nums, 1, nums.length - 1), robLinear(nums, 0, nums.length - 2));        
    }

    private int robLinear(int[] nums, int start, int end) {
        // use dynamic programming where
        // dp[i][0] = the max amount you can rob at this point excluding house i
        // dp[i][1] = the max amount you can rob at this point including house i
        // need to account for circular list

        int[][] dp = new int[nums.length][2];
        int maxResult = 0;

        for (int i = start; i <= end; i++) {
            // exclude house i
            if (i - 1 >= 0) {
                dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1]);
            }
            maxResult = Math.max(maxResult, dp[i][0]);
            
            // include house i
            int includeResult = 0;
            if (i - 2 >= 0) {
                includeResult = Math.max(dp[i - 2][0], dp[i - 2][1]);
            }
            dp[i][1] = includeResult + nums[i];
            maxResult = Math.max(maxResult, dp[i][1]);
        }

        return maxResult;
    }
}
```
4. Num decodings

Time complexity: `O(n)`
```
class Solution {
    public int numDecodings(String s) {
        char start = '0';

        // base cases
        if (s == null || s.isEmpty() || s.charAt(0) == start) return 0;

        // dp[i] stores number of possible encodings at that point
        int[] dp = new int[s.length() + 1];
        dp[0] = 1; // empty string
        dp[1] = 1; // string with 1 char

        for (int i = 2; i <= s.length(); i++) {
            int oneDigit = s.charAt(i - 1) - '0';
            int twoDigits = Integer.parseInt(s.substring(i - 2, i));

            if (oneDigit != 0) dp[i] += dp[i - 1];
            if (twoDigits >= 10 && twoDigits <= 26) dp[i] += dp[i - 2];
        }

        return dp[s.length()];
    }
}
```
