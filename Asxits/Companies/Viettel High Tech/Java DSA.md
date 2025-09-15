## Table of Contents

1. [Time & Space Complexity](https://claude.ai/chat/a8c0e592-e977-47dd-b25d-e46856976d74#time--space-complexity)
2. [Arrays](https://claude.ai/chat/a8c0e592-e977-47dd-b25d-e46856976d74#arrays)
3. [Strings](https://claude.ai/chat/a8c0e592-e977-47dd-b25d-e46856976d74#strings)
4. [Linked Lists](https://claude.ai/chat/a8c0e592-e977-47dd-b25d-e46856976d74#linked-lists)
5. [Stacks & Queues](https://claude.ai/chat/a8c0e592-e977-47dd-b25d-e46856976d74#stacks--queues)
6. [Trees](https://claude.ai/chat/a8c0e592-e977-47dd-b25d-e46856976d74#trees)
7. [Graphs](https://claude.ai/chat/a8c0e592-e977-47dd-b25d-e46856976d74#graphs)
8. [Hash Tables](https://claude.ai/chat/a8c0e592-e977-47dd-b25d-e46856976d74#hash-tables)
9. [Sorting Algorithms](https://claude.ai/chat/a8c0e592-e977-47dd-b25d-e46856976d74#sorting-algorithms)
10. [Searching Algorithms](https://claude.ai/chat/a8c0e592-e977-47dd-b25d-e46856976d74#searching-algorithms)
11. [Dynamic Programming](https://claude.ai/chat/a8c0e592-e977-47dd-b25d-e46856976d74#dynamic-programming)
12. [Common Patterns](https://claude.ai/chat/a8c0e592-e977-47dd-b25d-e46856976d74#common-patterns)

---

## Time & Space Complexity

### Big O Notation

- **O(1)** - Constant time
- **O(log n)** - Logarithmic time
- **O(n)** - Linear time
- **O(n log n)** - Linearithmic time
- **O(n²)** - Quadratic time
- **O(2^n)** - Exponential time

### Quick Reference

```java
// O(1) - Array access
int val = arr[0];

// O(n) - Linear search
for (int i = 0; i < n; i++) { /* */ }

// O(log n) - Binary search
while (left <= right) {
    int mid = left + (right - left) / 2;
    // ...
}

// O(n²) - Nested loops
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) { /* */ }
}
```

---

## Arrays

### Key Operations

```java
// Declaration and initialization
int[] arr = new int[5];
int[] arr2 = {1, 2, 3, 4, 5};

// Common operations
arr[0] = 10;           // O(1) - Access/Update
int len = arr.length;   // O(1) - Length

// ArrayList (Dynamic Array)
List<Integer> list = new ArrayList<>();
list.add(1);           // O(1) amortized
list.get(0);           // O(1)
list.remove(0);        // O(n)
list.size();           // O(1)
```

### Common Array Problems

```java
// Two Sum
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        map.put(nums[i], i);
    }
    return new int[]{};
}

// Maximum Subarray (Kadane's Algorithm)
public int maxSubArray(int[] nums) {
    int maxSoFar = nums[0];
    int maxEndingHere = nums[0];
    
    for (int i = 1; i < nums.length; i++) {
        maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    return maxSoFar;
}
```

---

## Strings

### Key Operations

```java
String str = "Hello";
str.length();              // O(1)
str.charAt(0);             // O(1)
str.substring(0, 3);       // O(n)
str.equals("Hello");       // O(n)
str.toCharArray();         // O(n)

// StringBuilder for efficient string building
StringBuilder sb = new StringBuilder();
sb.append("Hello");        // O(1) amortized
sb.toString();             // O(n)
```

### Common String Problems

```java
// Valid Palindrome
public boolean isPalindrome(String s) {
    int left = 0, right = s.length() - 1;
    while (left < right) {
        while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
            left++;
        }
        while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
            right--;
        }
        if (Character.toLowerCase(s.charAt(left)) != 
            Character.toLowerCase(s.charAt(right))) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}

// Longest Substring Without Repeating Characters
public int lengthOfLongestSubstring(String s) {
    Set<Character> set = new HashSet<>();
    int maxLength = 0;
    int left = 0;
    
    for (int right = 0; right < s.length(); right++) {
        while (set.contains(s.charAt(right))) {
            set.remove(s.charAt(left));
            left++;
        }
        set.add(s.charAt(right));
        maxLength = Math.max(maxLength, right - left + 1);
    }
    return maxLength;
}
```

---

## Linked Lists

### Implementation

```java
class ListNode {
    int val;
    ListNode next;
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}

class LinkedList {
    ListNode head;
    
    // Insert at beginning - O(1)
    void insertFirst(int val) {
        ListNode newNode = new ListNode(val);
        newNode.next = head;
        head = newNode;
    }
    
    // Delete node - O(n)
    void delete(int val) {
        if (head == null) return;
        
        if (head.val == val) {
            head = head.next;
            return;
        }
        
        ListNode current = head;
        while (current.next != null && current.next.val != val) {
            current = current.next;
        }
        
        if (current.next != null) {
            current.next = current.next.next;
        }
    }
}
```

### Common LinkedList Problems

```java
// Reverse Linked List
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode current = head;
    
    while (current != null) {
        ListNode next = current.next;
        current.next = prev;
        prev = current;
        current = next;
    }
    return prev;
}

// Detect Cycle (Floyd's Algorithm)
public boolean hasCycle(ListNode head) {
    if (head == null || head.next == null) return false;
    
    ListNode slow = head;
    ListNode fast = head.next;
    
    while (slow != fast) {
        if (fast == null || fast.next == null) return false;
        slow = slow.next;
        fast = fast.next.next;
    }
    return true;
}
```

---

## Stacks & Queues

### Stack Implementation

```java
// Using ArrayList
class Stack<T> {
    private List<T> stack = new ArrayList<>();
    
    void push(T item) {
        stack.add(item);
    }
    
    T pop() {
        if (isEmpty()) throw new EmptyStackException();
        return stack.remove(stack.size() - 1);
    }
    
    T peek() {
        if (isEmpty()) throw new EmptyStackException();
        return stack.get(stack.size() - 1);
    }
    
    boolean isEmpty() {
        return stack.isEmpty();
    }
}

// Java built-in
Stack<Integer> stack = new Stack<>();
stack.push(1);
stack.pop();
stack.peek();
```

### Queue Implementation

```java
// Using LinkedList
Queue<Integer> queue = new LinkedList<>();
queue.offer(1);    // enqueue
queue.poll();      // dequeue
queue.peek();      // front element

// ArrayDeque (preferred)
Deque<Integer> deque = new ArrayDeque<>();
deque.offerLast(1);   // add to rear
deque.pollFirst();    // remove from front
```

### Common Problems

```java
// Valid Parentheses
public boolean isValid(String s) {
    Stack<Character> stack = new Stack<>();
    Map<Character, Character> map = new HashMap<>();
    map.put(')', '(');
    map.put('}', '{');
    map.put(']', '[');
    
    for (char c : s.toCharArray()) {
        if (map.containsKey(c)) {
            if (stack.isEmpty() || stack.pop() != map.get(c)) {
                return false;
            }
        } else {
            stack.push(c);
        }
    }
    return stack.isEmpty();
}
```

---

## Trees

### Binary Tree Implementation

```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

### Tree Traversals

```java
// Inorder (Left, Root, Right)
public void inorder(TreeNode root) {
    if (root == null) return;
    inorder(root.left);
    System.out.print(root.val + " ");
    inorder(root.right);
}

// Preorder (Root, Left, Right)
public void preorder(TreeNode root) {
    if (root == null) return;
    System.out.print(root.val + " ");
    preorder(root.left);
    preorder(root.right);
}

// Postorder (Left, Right, Root)
public void postorder(TreeNode root) {
    if (root == null) return;
    postorder(root.left);
    postorder(root.right);
    System.out.print(root.val + " ");
}

// Level Order (BFS)
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;
    
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    
    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        List<Integer> currentLevel = new ArrayList<>();
        
        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            currentLevel.add(node.val);
            
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        result.add(currentLevel);
    }
    return result;
}
```

### Binary Search Tree

```java
// Search in BST
public TreeNode searchBST(TreeNode root, int val) {
    if (root == null || root.val == val) return root;
    return val < root.val ? searchBST(root.left, val) : searchBST(root.right, val);
}

// Insert into BST
public TreeNode insertIntoBST(TreeNode root, int val) {
    if (root == null) return new TreeNode(val);
    
    if (val < root.val) {
        root.left = insertIntoBST(root.left, val);
    } else {
        root.right = insertIntoBST(root.right, val);
    }
    return root;
}
```

---

## Graphs

### Graph Representation

```java
// Adjacency List
Map<Integer, List<Integer>> graph = new HashMap<>();

// Add edge
graph.computeIfAbsent(u, k -> new ArrayList<>()).add(v);

// Adjacency Matrix
int[][] matrix = new int[n][n];
matrix[u][v] = 1; // for directed graph
matrix[v][u] = 1; // for undirected graph
```

### Graph Traversals

```java
// DFS (Depth-First Search)
public void dfs(Map<Integer, List<Integer>> graph, int start, Set<Integer> visited) {
    visited.add(start);
    System.out.print(start + " ");
    
    for (int neighbor : graph.getOrDefault(start, new ArrayList<>())) {
        if (!visited.contains(neighbor)) {
            dfs(graph, neighbor, visited);
        }
    }
}

// BFS (Breadth-First Search)
public void bfs(Map<Integer, List<Integer>> graph, int start) {
    Queue<Integer> queue = new LinkedList<>();
    Set<Integer> visited = new HashSet<>();
    
    queue.offer(start);
    visited.add(start);
    
    while (!queue.isEmpty()) {
        int node = queue.poll();
        System.out.print(node + " ");
        
        for (int neighbor : graph.getOrDefault(node, new ArrayList<>())) {
            if (!visited.contains(neighbor)) {
                visited.add(neighbor);
                queue.offer(neighbor);
            }
        }
    }
}
```

---

## Hash Tables

### HashMap Operations

```java
Map<String, Integer> map = new HashMap<>();
map.put("key", 1);        // O(1) average
map.get("key");           // O(1) average
map.containsKey("key");   // O(1) average
map.remove("key");        // O(1) average

// Iteration
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    Integer value = entry.getValue();
}
```

### HashSet Operations

```java
Set<Integer> set = new HashSet<>();
set.add(1);              // O(1) average
set.contains(1);         // O(1) average
set.remove(1);           // O(1) average
```

---

## Sorting Algorithms

### Quick Sort

```java
public void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

private int partition(int[] arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr, i, j);
        }
    }
    swap(arr, i + 1, high);
    return i + 1;
}
```

### Merge Sort

```java
public void mergeSort(int[] arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}

private void merge(int[] arr, int left, int mid, int right) {
    int[] temp = new int[right - left + 1];
    int i = left, j = mid + 1, k = 0;
    
    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) {
            temp[k++] = arr[i++];
        } else {
            temp[k++] = arr[j++];
        }
    }
    
    while (i <= mid) temp[k++] = arr[i++];
    while (j <= right) temp[k++] = arr[j++];
    
    for (i = left; i <= right; i++) {
        arr[i] = temp[i - left];
    }
}
```

---

## Searching Algorithms

### Binary Search

```java
public int binarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;
}
```

---

## Dynamic Programming

### Key Concepts

- **Overlapping Subproblems**: Same subproblems solved multiple times
- **Optimal Substructure**: Optimal solution contains optimal solutions to subproblems
- **Memoization**: Top-down approach with caching
- **Tabulation**: Bottom-up approach with table

### Common DP Problems

```java
// Fibonacci
public int fibonacci(int n) {
    if (n <= 1) return n;
    
    int[] dp = new int[n + 1];
    dp[0] = 0;
    dp[1] = 1;
    
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
}

// Climbing Stairs
public int climbStairs(int n) {
    if (n <= 2) return n;
    
    int prev2 = 1, prev1 = 2;
    for (int i = 3; i <= n; i++) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}

// 0/1 Knapsack
public int knapsack(int[] weights, int[] values, int capacity) {
    int n = weights.length;
    int[][] dp = new int[n + 1][capacity + 1];
    
    for (int i = 1; i <= n; i++) {
        for (int w = 1; w <= capacity; w++) {
            if (weights[i - 1] <= w) {
                dp[i][w] = Math.max(
                    dp[i - 1][w],
                    dp[i - 1][w - weights[i - 1]] + values[i - 1]
                );
            } else {
                dp[i][w] = dp[i - 1][w];
            }
        }
    }
    return dp[n][capacity];
}
```

---

## Common Patterns

### Two Pointers

```java
// Two Sum in Sorted Array
public int[] twoSum(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    
    while (left < right) {
        int sum = arr[left] + arr[right];
        if (sum == target) {
            return new int[]{left, right};
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    return new int[]{-1, -1};
}
```

### Sliding Window

```java
// Maximum Sum Subarray of Size K
public int maxSum(int[] arr, int k) {
    int windowSum = 0, maxSum = 0;
    
    // Calculate sum of first window
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    maxSum = windowSum;
    
    // Slide the window
    for (int i = k; i < arr.length; i++) {
        windowSum = windowSum - arr[i - k] + arr[i];
        maxSum = Math.max(maxSum, windowSum);
    }
    return maxSum;
}
```

### Fast & Slow Pointers

```java
// Find Middle of Linked List
public ListNode findMiddle(ListNode head) {
    ListNode slow = head, fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}
```

---

## Key Tips for Interviews

1. **Always clarify the problem** - Ask about edge cases, constraints, input/output format
2. **Think out loud** - Explain your approach before coding
3. **Start with brute force** - Then optimize
4. **Test with examples** - Walk through your solution with sample inputs
5. **Consider edge cases** - Empty inputs, single elements, duplicates
6. **Optimize space/time complexity** - Look for ways to reduce complexity
7. **Clean code** - Use meaningful variable names, proper indentation

### Common Time Complexities to Remember

- **Sorting**: O(n log n) - Merge Sort, Quick Sort
- **Binary Search**: O(log n)
- **Hash Operations**: O(1) average
- **Tree Operations**: O(log n) for balanced trees
- **Graph Traversal**: O(V + E) where V = vertices, E = edges

This crash course covers the essential DSA concepts you'll encounter in coding interviews. Practice these patterns and understand when to apply each data structure and algorithm!