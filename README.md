# InterviewPrep

## Heuristics when stuck on a problem

- Always consider hash tables (dictionaries) with their O(1)-ness (using a dictionary is the most common way to get from a brute force approach to something more clever. It should always be your first thought).
- If at all array-related, try sorting first.
- If search-related, consider binary search.
- Start with a brute force solution, look for repeat work in that solution, and modify it to only do that work once
- Space-time trade-off! That is, for better time complexity, try using auxiliary data structures. E.g, do something in a single pass over an array (O(N) time) by using a hash table (O(N) space) vs. doing something in multiple passes (O(N²) time) without using any extra space (O(1)). What information can I store to save time?
- Try a greedy solution: Iterate through the problem space taking the optimal solution "so far" until the end. (Optimal if the problem has "optimal substructure", which means stitching together optimal solutions to subproblems yields an optimal solution.)
- Remember that I can use two pointers (e.g., to get the midpoint by having one pointer go twice as fast, or in a sum problem by having the pointers work inward from either end, or to test if a string is a palindrome).
- If the problem involves parsing or tree/graph traversal (or reversal in some way), consider using a stack.
- Does solving the problem for size (N-1) make solving it for size N any easier? If so, try to solve recursively and/or with dynamic programming. (Using the max/min function can help a lot in recursive or dynamic programming problems.)
- A lot of problems can be treated as graph problems and/or use breadth-first or depth-first traversal
- If you have a lot of strings, try putting them in a  prefix tree/trie
- Any time you repeatedly have to take the min or max of a dynamic collection, think heaps. (If you don't need to insert random elements, prefer a sorted array.)

## Basics

[Data Structures](#data-structures) | [Algorithms](#algorithms) | [Concepts](#concepts)
--- | --- | ---
Linked List | [Breadth-First Search](#bfs) | Bit Manipulation
Trees, Tries, & Graphs | [Depth-First Search](#dfs) | Memory (Stack vs. Heap)
Stacks & Queues | [Binary Search](#binary-search) | [Recursion](#recursion-and-dynamic-programming)
Heaps | [Merge Sort](#merge-sort) | [Dynamic Programming](#recursion-and-dynamic-programming)
Vectors/ ArrayList | [Quick Sort](#quick-sort) | Big O Time & Space
Hash Tables |

## Data Structures
## Algorithms
### BFS

In BFS, we start at the root (or another arbitrarily selected node) and explore each neighbor before going on to any of their children. That is, we go wide (hence breadth-first search) before we go deep.

If we want to find the shortest path (or just any path) between two nodes, BFS is generally better.
If you are asked to implement BFS, the key thing to remember is the use of the queue.

```
graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F']
}
visited = set()
queue = []

def bfs(visited, graph, node):
    visited.add(node)
    queue.append(node)
    
    while queue:
        s = queue.pop(0)
        
        for neighbor in graph[s]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)


bfs(visited, graph, 'A')
```

### DFS

In DFS, we start at the root (or another arbitrarily selected node) and explore each branch completely before moving on to the next branch. That is, we go deep first (hence the name depth-first search) before we go wide.

DFS is often preferred if we want to visit every node in the graph.

```
graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F']
}
visited = set()

def dfs(visited, graph, node):
   if node not in visited:
       visited.add(node)
       for neighbor in graph[node]:
           dfs(visited, graph, neighbor)

dfs(visited, graph, 'A')
```

### Binary Search

In binary search, we look for an element x in a sorted array by first comparing x to the midpoint of the array. If x is less than the midpoint, then we search the left half of the array. If x is greater than the midpoint, then we search the right half of the array. We then repeat this process, treating the left and right halves as subarrays.We repeat this process until we either find x or the subarray has size 0.

```
def binary_search(a, x):
    low = 0
    high = len(a) - 1
    mid = (low + high) // 2
    
    while low <= high:
        mid = (low + high) // 2
        if a[mid] < x:
            low = mid + 1
        elif a[mid] > x:
            high = mid - 1
        else:
            return mid
    return -1 # Error

def binary_search_recursive(a, x, low, high):
    if low > high:
        return -1 # Error
    mid = (low + high) // 2
    if a[mid] < x:
        return binary_search_recursive(a, x, mid+1, high)
    elif a[mid] > x:
        return binary_search_recursive(a, x, low, mid-1)
    else:
        return mid
    
```

### Merge Sort
*Runtime: O(n log(n)) average and worst case. Memory: Depends.*

Merge sort divides the array in half, sorts each of those halves, and then merges them back together recursively. Eventually, you are merging just two single-element arrays.

```
def init_merge_sort(orig):
    merge_sort(orig, 0, len(orig))


def merge_sort(orig, low, high):
    if low<high:
        mid = (low+high)//2
        merge_sort(orig, low, mid)
        merge_sort(orig, mid+1, high)
        merge(orig, low, mid, high)

def merge(orig, low, mid, high):
    aux = orig[low:high].copy()
    
    left = low
    right = mid+1
    current = low
    
    while left <= mid and right <= high:
        if aux[left] < aux[right]:
           left+=1
           orig[current]=aux[left]
        else:
           right+=1
           orig[current]=aux[right]
        current+=1
    
    remaining = mid - left
    for i in range(remaining):
        orig[current + i] = aux[left + i]
```

### Quick Sort

*Runtime: O(n log (n)) average, O(n²) worst case. Memory: O(log(n))*

In quick sort, we pick a random element and partition the array, such that all numbers that are less than the partitioning element come before all elements that are greater than it. The partitioning can be performed efficiently through a series of swaps.

```
def quick_sort(arr, left, right):
    pi = partition(arr, left, right)
    if left < pi-1:
        quick_sort(arr, left, pi-1)
    if right > pi:
        quick_sort(arr, pi, right)

def partition(arr, left, right):
    pivot = arr[(left+right)//2]
    while left <= right:
        while arr[left] < pivot:
            left += 1
        while arr[right] > pivot:
            right -= 1
        if left <= right:
            swap(arr, left, right)
            left += 1
            right -= 1
    return left
```
## Concepts
### Recursion and Dynamic Programming

A good way to approach dynamic programming is to implement it as a normal recursive solution, and then add the caching part.

#### Example Problem
**Triple Step:** A child is running up a staircase with n steps and can hop either 1 step, 2 steps or 3 steps at a time. Implement a method to count how many possible ways the child can run up the stairs.

#### Brute Force Solution

```
def count_ways(n):
    if n == 0:
        return 1
    elif n < 0:
        return 0
    else:
        return count_ways(n-1) + count_ways(n-2) + count_ways(n-3)
```

#### Dynamic Programming Solution

```
def init_count_ways(n):
    mem = [-1]*n
    count_ways(n,mem)

def count_ways(n,mem):
    if n == 0:
        return 1
    elif n < 0:
        return 0
    elif mem[n] > -1:
        return mem[n]
    else:
        mem[n] = return count_ways(n-1,mem) + count_ways(n-2,mem) + count_ways(n-3, mem)
        return mem[n]
```