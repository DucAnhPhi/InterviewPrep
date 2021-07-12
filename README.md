# InterviewPrep

## Basics

[Data Structures](#data-structures) | [Algorithms](#algorithms) | [Concepts](#concepts)
--- | --- | ---
Linked List | Breadth-First Search | Bit Manipulation
Trees, Tries, & Graphs | Depth-First Search | Memory (Stack vs. Heap)
Stacks & Queues | Binary Search | Recursion
Heaps | Merge Sort | Dynamic Programming
Vectors/ ArrayList | Quick Sort | Big O Time & Space
Hash Tables

## Data Structures
## Algorithms
### Breadth-First Search (BFS)

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

### Depth-First Search (DFS)

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
