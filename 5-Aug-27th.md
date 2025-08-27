# Aug 27th Practice Problems
## Notes
- Stringbuilder..

---
## Problems
1. Reverse linked list

Don't forget to set head.next to null first - think about base case!!
**Time complexity:** `O(n)`

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) return head;

        ListNode prev = null;   // start with null, since new tail will point to null
        ListNode curr = head;

        while (curr != null) {
            ListNode nextNode = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextNode;
        }
        return prev;
    }
}
```

2. Is palindrome (incomplete)

**Time complexity:** ``

```
class Solution {
    public boolean isPalindrome(String s) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            char curr = s.charAt(i);
            if (Character.isLetterOrDigit(curr)) {
                sb.append(Character.toLowerCase(curr));
            }
        }

        String copy = sb.toString();
        if (copy.length() == 0) return true;

        if (copy.length() % 2 == 0) {
            int leftPointer = 0;
            int rightPointer = copy.length() - 1;

            while (rightPointer < leftPointer) {
                if (copy.charAt(rightPointer) != copy.charAt(leftPointer))
                    return false; // not palindrome
                leftPointer++;
                rightPointer--;
            }
        } else {
            int center = copy.length() / 2;
            int counter = 0;
            while ((center - counter > 0) && (center + counter < copy.length() - 1)) {
                if (copy.charAt(center - counter) != copy.charAt(center + counter))
                    return false;
                counter++;
            }
        }

        return true;
    }
}
```

3. 

**Time complexity:** ``

```

```
