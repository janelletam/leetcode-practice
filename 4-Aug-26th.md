# Aug 26th Practice Problems
## Notes
- Set timer for problems!!
- Use debugging statements generously - they save a lot of time in the long run especially for tricky bugs
- Put debugging statements BEFORE I change the variables so I can see the current state
- Consider base cases (e.g. inputs are null) and do early return
- Convert list to HashSet by passing it in constructor ->  ```HashSet<String> set = new HashSet<>(list);```

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

2. Does linked list cycle exist?

I originally solved using HashSet (O(n)) - but better space complexity would involve using Floyd's algorithm and the two-pointer approach.

**Time complexity:** `O(n)`

Original:
```
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        // store each value in a hashmap
        HashSet<ListNode> seen = new HashSet<>();

        ListNode curr = head;
        while (curr != null && curr.next != null && !seen.contains(curr)) {
            seen.add(curr);
            curr = curr.next;
        }

        if (curr == null || curr.next == null) return false; // does not have cycle, reached end
        else return true; // does have cycle, we've already seen the next node
    }
}

```

Better:
```
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        // Better approach: tortoise and hare
        if (head == null) return false; // base case

        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next; // travels double the speed of slow

            if (fast == slow) return true; // pointers overlap
        }

        return false;
    }
}
```

3. Word break

I'm using greedy rn which fails... should use DP instead & backtracking. To do for tomorrow
**Time complexity:** `O(n)`

```
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        HashSet<String> wordSet = new HashSet<>(wordDict);
        int startPointer = 0;
        int endPointer = 0;

        while (endPointer < s.length()) {
            if (wordSet.contains(s.substring(startPointer, endPointer + 1))) {
                // found matched word
                System.out.println("Matched");
                System.out.println("Start: " + startPointer);
                System.out.println("End: " + endPointer);
                startPointer = endPointer + 1;
            }
            endPointer++; // for cases where the word is a single letter
        }

        // Matched all chars
        System.out.println("Final start: " + startPointer);
        return startPointer == s.length();
    }
}
```
