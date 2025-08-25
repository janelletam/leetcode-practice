# Aug 24th Practice Problems
## Notes
- Syntax: HashSet add is `set.add(element)` vs. HashMap add is `map.put(key, value)`
- When calculate mid-index based on start/end, don't forget to ADD the start value to whatever the mid-index calculation is
```int mid = START + (end - start) / 2;```
- If problem requires O(logn) time, usually involves halving the solution in some way to minimize the size of the problem

---
## Problems
1. Search rotated sorted array

**Time complexity:** `O(logn)`

```
class Solution {
    private int[] _nums;
    private int _target;
    public int search(int[] nums, int target) {
        _nums = nums;
        _target = target;
        
        return findWhereSorted(0, _nums.length - 1);
    }

    private int findWhereSorted(int startIndex, int endIndex) {
        int midIndex = startIndex + (endIndex - startIndex) / 2;
        int nextStartIndex;
        int nextEndIndex;

        if (startIndex > endIndex) {
            return -1; // base case - not found
        }

        // special case: only 2 elements
        if (endIndex - startIndex <= 1) {
            if (_nums[startIndex] == _target) return startIndex;
            if (_nums[endIndex] == _target) return endIndex;
            return -1;
        }

        // Found target
        if (_nums[midIndex] == _target) {
            return midIndex;
        }
        // Either everything before the mid index is sorted)
        else if (_nums[startIndex] < _nums[midIndex]) {
            if (_target >= _nums[startIndex] && _target < _nums[midIndex]) {
                return binarySearch(startIndex, midIndex - 1);
            } else {
                return findWhereSorted(midIndex + 1, endIndex);
            }
        // Or everything after is sorted
        } else {
            if (_target > _nums[midIndex] && _target <= _nums[endIndex]) {
                return binarySearch(midIndex + 1, endIndex);
            } else {
                return findWhereSorted(startIndex, midIndex - 1);
            }
        }
    }

    private int binarySearch(int startIndex, int endIndex) {
        if (startIndex > endIndex) {
            return -1; // base case - not found
        }
        
        int midIndex = startIndex + (endIndex - startIndex) / 2; // will round down 

        if (_nums[midIndex] == _target) {
            return midIndex;
        } else if (_nums[midIndex] < _target) {
            return binarySearch(midIndex + 1, endIndex);
        } else {
            return binarySearch(startIndex, midIndex - 1);
        }
    }
}
```

2. Longest consecutive sequence
   
**Time complexity**: Because of the `continue` line, we visit each element in the set at most once

```
class Solution {
    public int longestConsecutive(int[] nums) {
        // convert to hashset
        // for each number, check to see if its n -1 value is in the set -> if so, skip
        // if not, check to see if there exists a sequence
        HashSet<Integer> set = new HashSet<>();
        int maxSequenceLength = 0;

        for (int num : nums) {
            set.add(num);
        }

        for (int num : set) {
            if (set.contains(num - 1)) continue;
            int currSequenceLength = 1;
            int curr = num;

            while (set.contains(curr + 1)) {
                currSequenceLength++;
                curr++;
            }

            maxSequenceLength = Math.max(maxSequenceLength, currSequenceLength);
        }

        return maxSequenceLength;
    }
}
```

3. Find peak element

**Time complexity:** O(logn)
```
class Solution {
    public int findPeakElement(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        int peak = -1;
       
        while (left <= right) {
            // base checks
            if (right == left) return right; // only 1 element left 

            if (right - left == 1) {
                if (nums[right] > nums[left]) return right;
                else return left;
            }

            int mid = left + (right - left) / 2;

            // check around the mid point
            if (nums[mid - 1] < nums[mid] && nums[mid] > nums[mid + 1]) {
                return mid;
            }

            // at least 1 peak mnust exist to the left
            if (nums[mid - 1] > nums[mid]) {
                right = mid - 1;
            // at least 1 peak must exist to the right
            } else {
                left = mid + 1;
            }
        }

        return peak;
    }
}
```

4. Roman to int

First solution: not optimal.. uses substring
```
class Solution {
    public int romanToInt(String s) {
        int value = 0;

        HashMap<String, Integer> map = createRomanToIntMap();

        for (int i = 0; i < s.length(); i++) {
            if (i + 1 < s.length()) {
                String twoChars = s.substring(i, i + 2);
                if (map.containsKey(twoChars)) {
                    value += map.get(twoChars);
                    i++; // skip over next char
                    continue;
                }
            }

            value += map.get(String.valueOf(s.charAt(i)));
        }

        return value;
    }

    private HashMap<String, Integer> createRomanToIntMap() {
        HashMap<String, Integer> map = new HashMap<>();
        map.put("I", 1);
        map.put("V", 5);
        map.put("X", 10);
        map.put("L", 50);
        map.put("C", 100);
        map.put("D", 500);
        map.put("M", 1000);
        map.put("IV", 4);
        map.put("IX", 9);
        map.put("XL", 40);
        map.put("XC", 90);
        map.put("CD", 400);
        map.put("CM", 900);

        return map;
    }
}
```

**Better time complexity:** `O(n)`

```class Solution {
    public int romanToInt(String s) {
        int value = 0;

        for (int i = 0; i < s.length(); i++) {
            int curr = convertCharToInt(s.charAt(i));
            if (i < s.length() - 1) {
                int next = convertCharToInt(s.charAt(i + 1));
                if (curr < next) {
                    value += next - curr;
                    i++; // skip next value as well
                    continue;
                }
            }

            value += curr;
        }

        return value;
    }

    private int convertCharToInt(char c) {
        switch (c) {
            case 'I': return 1;
            case 'V': return 5;
            case 'X': return 10;
            case 'L': return 50;
            case 'C': return 100;
            case 'D': return 500;
            case 'M': return 1000;
            default: return -1; // invalid
        }
    }
}
```
