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

3. 3-sum

   First approach (initial - works but bad time complexity)
```
   class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        // Create hashmap with all the numbers (value - index)
        // use a forloop to iterate - find the complement needed to reach the target
        // for each value in the hashmap, look to see if the next complement exists
        // in the hashmap and has a diff value than the index

        HashMap<Integer, List<Integer>> numMap = new HashMap<>();
        HashSet<List<Integer>> result = new HashSet<>();

        int complement = 0;

        // Build the hashmap
        for (int i = 0; i < nums.length; i++) {
            List<Integer> indices = numMap.getOrDefault(nums[i], new ArrayList<>());
            indices.add(i);
            numMap.put(nums[i], indices);
        }

        // Iterate through pairs
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                complement = -(nums[i] + nums[j]);

                if (numMap.containsKey(complement)) {
                    List<Integer> complementIndices = numMap.get(complement);

                    final int final_i = i;
                    final int final_j = j;
                    boolean hasValidIndex = complementIndices.stream()
                            .anyMatch(k -> k != final_i && k != final_j);

                    if (hasValidIndex) {
                        List<Integer> triplet = Arrays.asList(nums[i], nums[j], complement);
                        Collections.sort(triplet); // sort to avoid duplicates
                        result.add(triplet);
                    }
                }
            }
        }
        
        return new ArrayList<>(result);
    }
}
```
**Better time complexity:** `O(n^2)`
```
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        // Start by sorting the nums to enable the two pointer approach
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);

        // subtract two from nums.length since we need at least 2 nums remaining
        // to form a triplet
        for (int left = 0; left < nums.length - 2; left++) {
            if (left > 0 && nums[left] == nums[left - 1]) continue; // skip duplicates
            
            int mid = left + 1, right = nums.length - 1;
            while (mid < right) {
                int sum = nums[left] + nums[mid] + nums[right];

                if (sum == 0) {
                    result.add(Arrays.asList(nums[left], nums[mid], nums[right]));
                    
                     // Skip duplicates
                    while (mid < right && nums[right] == nums[right - 1]) right--;
                    while (mid < right && nums[mid] == nums[mid + 1]) mid++;
                    mid++;
                    right--;
                } else if (sum < 0) {
                    mid++;
                } else {
                    right--;
                }
            }
        }

        return result;
    }
}
```
