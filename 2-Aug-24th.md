# Aug 24th Practice Problems
## Notes

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
