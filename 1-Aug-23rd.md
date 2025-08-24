# Aug 23rd Practice Problems
## Notes:
- To calculate length of substring, use `right - left + 1`
- Be careful with instantiating list (abstract) vs. arraylist

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

4. Group anagrams
**Time complexity**: `O(n * klog(k))` - where n = num of elements in list, k = average length of string
(sorting cost)

```
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        // Start by sorting each string (turn into a char array)
        // Add to hashmap as key - the value is the original string 

        HashMap<String, List<String>> map = new HashMap<>();
        List<List<String>> result = new ArrayList<List<String>>();
        for (String str : strs) {
            char[] sortedStr = str.toCharArray();
            Arrays.sort(sortedStr);
            String sortedStrKey = new String(sortedStr);

            List<String> anagrams = map.getOrDefault(sortedStrKey, new ArrayList<>());
            anagrams.add(str);
            map.put(sortedStrKey, anagrams);
        }

        for (Map.Entry<String, List<String>> entry : map.entrySet()) {
            result.add(entry.getValue());
        }

        return result;
    }
}
```

5. Container with most water
**Time complexity**: `O(n^2)` (too slow)
```
   class Solution {
    public int maxArea(int[] height) {
        // Algorithm:
        // Search the entire array (using pointer) for the first value (closest to the edge)
        // that's greater than or equal to current value (bottleneck) OR just the bigggest
        // value if no such value exists
        // Then, calculate what the volume of the container is at that point
        // 
        // Iterate from all the values from the right <- going inwards to see if
        // it creates a bigger container
        // If not, the one identified is the biggest container
        int maxVolume = 0;
        for (int i = 0; i < height.length - 1; i++) {
            int[] maxInfo = findMaxNumber(i, height);
            int maxValue = maxInfo[0];
            int maxIndex = maxInfo[1];
            maxVolume = Math.max(maxVolume, calculateVolume(height[i], height[maxIndex], i, maxIndex));
            int right = height.length - 1;
            while (right > maxIndex) {
                int volume = calculateVolume(height[i], height[right], i, right);
                if (volume > maxVolume) {
                    maxVolume = volume;
                } 
                right--;
            }
        }

        return maxVolume;
    }

    private int[] findMaxNumber(int start, int[] list) {
        int maxIndex = list.length - 1;
        int maxValue = -1;
        for (int i = list.length - 1; i > start; i--) {
            if (list[i] > maxValue) {
                maxValue = list[i];
                maxIndex = i;
            }
        }
        return new int[] {maxValue, maxIndex};
    }

    private int calculateVolume(int height_1, int height_2, int index_1, int index_2) {
        return Math.min(height_1, height_2) * (index_2 - index_1);
    }
}
```

Better time complexity (using two pointer approach): `O(n)`
```
class Solution {
    public int maxArea(int[] height) {
        int maxVolume = 0;
        int left = 0, right = height.length - 1;

        while (left < right) {
            maxVolume = Math.max(maxVolume, calculateVolume(height[left], height[right], left, right));
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }

        return maxVolume;
    }

    private int calculateVolume(int height_1, int height_2, int index_1, int index_2) {
        return Math.min(height_1, height_2) * (index_2 - index_1);
    }
}
```

6. Copy graph
**Time complexity:**: `O(v + e)`
- uses DFS
```
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

class Solution {
    HashMap<Integer, Node> copiedNodes = new HashMap<>();

    public Node cloneGraph(Node node) {
        if (node == null) return null;
        if (copiedNodes.containsKey(node.val)) return copiedNodes.get(node.val); // return existing copy

        Node copiedNode = new Node(node.val);
        copiedNodes.put(node.val, copiedNode);

        for (Node neighbor : node.neighbors) {
            copiedNode.neighbors.add(cloneGraph(neighbor));
        }

        return copiedNode;
    }
}
```

7. Is graph bipartite?
**Time complexity:** `O(v + e`
```
class Solution {
    int[][] _graph;
    int[] colours;

    public boolean isBipartite(int[][] graph) {
        _graph = graph;
        colours = new int[_graph.length];

        for (int i = 0; i < _graph.length; i++) {
            if (colours[i] == 0 && !isValidColour(i, 1)) {
                return false;
            }
        }

        return true;
    }

    private boolean isValidColour(int node, int colour) {
        if (colours[node] != 0) return colours[node] == colour;

        colours[node] = colour;
        for (int neighbour : _graph[node]) {
            if (!isValidColour(neighbour, -colour))
                return false;
        }

        return true;
    }
}
```
