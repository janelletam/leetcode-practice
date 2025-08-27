# [Date] Practice Problems
## Notes
- 

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

2. 

**Time complexity:** ``

```

```

3. 

**Time complexity:** ``

```

```
