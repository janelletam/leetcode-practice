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

**Time complexity:** `O(n)`

```
class Solution {
    public boolean isPalindrome(String s) {
        String copy = createAlphaNum(s);

        if (copy.length() == 0) return true;

        // Even palindrome
        if (copy.length() % 2 == 0) {
            int leftPointer = 0;
            int rightPointer = copy.length() - 1;

            while (leftPointer < rightPointer) {
                if (copy.charAt(rightPointer) != copy.charAt(leftPointer)) return false; // not palindrome
                leftPointer++;
                rightPointer--;
            }
        } 
        // Odd palindrome
        else {
            int center = copy.length() / 2;
            int counter = 1;
            
            while ((center - counter >= 0) && (center + counter <= copy.length() - 1)) {
                if (copy.charAt(center - counter) != copy.charAt(center + counter)) return false;
                counter++;
            }
        }

        return true;
    }

    private String createAlphaNum(string s) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            char curr = s.charAt(i);
            if (Character.isLetterOrDigit(curr)) {
                sb.append(Character.toLowerCase(curr));
            }
        }

        return sb.toString()
    }
}
```

More elegant solution (just use two pointers, same approach for even and odd):
```
class Solution {
    public boolean isPalindrome(String s) {
        s = createAlphaNum(s);
        
        int leftPointer = 0;
        int rightPointer = s.length() - 1;

        while (leftPointer < rightPointer) {
            if (s.charAt(leftPointer) != s.charAt(rightPointer)) return false;
            leftPointer++;
            rightPointer--;
        }

        return true;
    }

    private String createAlphaNum(String s) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            char curr = s.charAt(i);
            if (Character.isLetterOrDigit(curr)) {
                sb.append(Character.toLowerCase(curr));
            }
        }

        return sb.toString();
    }
}
```

3. Max path through binary tree

Uses recursion, I needed help for this... But basically, think: what do I need for recursion to work? What result am I actually checking? 

**Intuition behind solution:** In this case, in order for recursion to work, I have to return the max value when NOT SPLITTING but including the root parameter in dfs(root). But the actual result that I'm finding - I do that by checking to see if splitting at the root node in question gives the biggest result.

**Time complexity:** `O(n)`

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int result;

    public int maxPathSum(TreeNode root) {
        // use a dfs based on the root
        // dfs: returns what is the max value of the subtrees
        // (path includes whatever the root is)
        // update result based on: what's the max value WITH spliting
        result = root.val;
        dfs(root);
        return result;
    }

    // return max path WITHOUT split
    private int dfs(TreeNode root) {
        if (root == null) return 0; // empty

        int leftMax = Math.max(0, dfs(root.left)); // don't include if subtree is negative
        int rightMax = Math.max(0, dfs(root.right));

        // compute WITH split
        result = Math.max(result, root.val + leftMax + rightMax);

        // intuition: can only split once, so therefore in order
        // for recursion to work, I have to return the actual max without splitting
        return root.val + Math.max(leftMax, rightMax);
    }
}
```
