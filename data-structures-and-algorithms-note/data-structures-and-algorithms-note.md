# Data Structures and Algorithms Note

## Data Structures

### Array

If arrays are fixed length, when an array is full, a typical implementation is to double in size. Each doubling takes O(n) time. Amortized insertion time is still O(1).

```java
  ArrayList<String> merge(String[] words, String[] more) {
    ArrayList<String> sentence = new ArrayList<String>();
    for (String w : words) {
      sentence.add(w);
    }
    for (String w : more) {
      sentence.add(w);
    }
    return sentence;
  }
```

[Find the index of an element in an array in Java.](https://www.techiedelight.com/find-index-element-array-java/)

---

### StringBuilder

When you concatenate a listing of strings, if you use String type to store the final string, complexity is O(n^2). However, you can use StringBuilder to create a resizable array.

```java
String joinWords(String[] words) {
    StringBuilder sentence = new StringBuilder();
    for (String w : words) {
        sentence.append(w);
    }
    return sentence.toString();
}
```

---

### Linked List

A list of data items connected with links.

Types of linked list:

- Singly Linked List
- Doubly Linked List
- Circular Linked List

When to use a linked list:

- You do not need random access to any specific elements.
- You need to do constant insertions and deletions.
- You are not sure how many items will be in the list.

#### Singly Linked List

![singly-linked-list.jpg](img/singly-linked-list.jpg)

- Advantages: insertion and deletion can be very quick.
  - insertion (prepend) - **O(1)**
  - insertion (append) - **O(n)**
- Disadvantages
  - Slow to access an element. **O(n)**
  - Memory is a concern. Require data and pointers.

#### Doubly Linked List

![doubly-linked-list.jpg](img/doubly-linked-list.jpg)

#### Circular Linked List

##### Singly Circular Linked List

![singly-circular-linked-list.jpg](img/singly-circular-linked-list.jpg)

##### Doubly Circular Linked List

![doubly-circular-linked-list.jpg](img/doubly-circular-linked-list.jpg)

---

### Stack

An Abstract Data Type (ADT) used to store and retrieve values in Last In First Out method.

flexible size

time factor - O(n)

![stack.jpg](img/stack.jpg)

When to use stack:

- recursive function calls
- expression parsing
- depth first traversal of graphs

Basic operations:

- push():
  - Firstly check if the stack is full.
- pop():
  - Firstly check if the stack is empty.
  - Return the top value.
- peek(): get the top data element of the stack, without removing it.
  - `return stack[top]`
- isFull(): check if stack is full.
  - ```
    // array implementation
    if (top == MAXSIZE - 1)
        return true;
    else
        return false;
    ```
- isEmpty(): check if stack is empty.
  - ```
    // array implementation
    if (top == -1)
        return true;
    else
        return false;
    ```

---

### Queue

An Abstract Data Type (ADT) used to store and retrieve values in First In First Out method.

flexible size

![queue.jpg](img/queue.jpg)

When to use queue:

- priority queues
- breadth first traversal of graphs

Basic operations:

- enqueue():
  - Firstly check if the queue is full.

- dequeue():
  - Firstly check if the queue is empty.
  - Return the front value.

- peek(): get the element at the front of the queue without removing it.
  - `return queue[front]`

- isFull(): check if queue is full.
  - ```
    // array implementation
    if (rear == MAXSIZE - 1)
        return true;
    else
        return false;
    ```

- isEmpty(): check if queue is empty.
  - ```
    // array implementation
    if (front < 0 || front > rear)
      return true;
    else
      return false;
    ```

O(1) time for both array-based and linkedList-based implementations. 
 
Linked lists have a large number of primitive operations per call. **In practice, the linkedList method is more expensive than the array-based method.**

#### Circular Queue

Adapt the CircularlyLinkedList implementation.

Use cases:

- multiplayer, turn-based games 
- round-robin scheduling of computing processes

#### Deque (Double-Ended Queue)

A doubly linked list is most appropriate for implementing all operations efficiently.

---

### Hash Table

![hash-table.png](img/hash-table.png)

![hash-table-complexity.png](img/hash-table-complexity.png)

Conceptualize hash table as a bucket array.

![bucket-array.png](img/bucket-array.png)

Advantages: 

- Insertion and search operations are very fast.
- Do not need to search through the array when finding a value.

Disadvantages: 

- Hard to order.

#### Hashing

Use hashing technique to generate an index where an element is to be inserted or is to be located from.

Hashing is a technique to convert a key into an index of an array. Steps:

1. The key is passed into **hash code function**, and get hash code. 
2. The hash code is passed into **compression function** and get the index of the bucket array. 
    - Maybe using `index = hashCode % N`, where `N` is commonly chosen as size of the bucket array.
    - If `sizeOfBucketArray` is a prime number, then the compression function
is good. If not, greater risk of causing collisions.
    
![hash-function.jpg](img/hash-function.jpg)

A good hash function: 

- sufficiently minimize collisions
- fast and easy to compute
- The probability any two different keys collide is 1/sizeOfBucketArray. 

#### Collision

1. There is key-value pair data.
2. Use a hash function with key as input parameter to get the the index of the array where to store data. Make sure the index is within bound of size of the array.
3. The index may be repeated, which is called collision. 

Ways to solve collision:

- Separate chaining 
   - Chaining with linked lists: store data with the same hash code in linked lists. So, the whole data structure will be array of linked lists.
     - **the most common** approach
     - worst lookup time O(n)
     - If the number of collisions is small, it is efficient.
     - :-1: The linked list may be very long. It will take time to find data with the same index.
     - :-1: If much data is stored in the linked list instead of bucket array, the remaining of the array will be wasted.
     - (When the number of entries in the linked list is 75% of bucket array size, double size the bucket array. Then re-allocate entries, which is costly.)
   - Chaining with balanced binary search trees: store collisions in a binary search tree.
     - rare approach
     - worst lookup time O(log n)
     - for extremely non-uniform distribution
     - 👍 Potentially using less space, since no longer allocate a large array.
     - 👍 Can iterate through the keys in order.
- Open addressing   
   - Linear probing: search the next empty location in the bucket array to store the data. Probe distance is 1 or another fixed value.
     - If the number of collisions is small, it is efficient.
     - 👎 Size of bucket array is limited.
     - 👎 Data may be stored one next to one another, which causes clustering.  
   - Quadratic probing: increase the probe distance quadratically.
     - Complicates removal operation.
     - Causes secondary clustering.
     - Not guaranteed to find an empty slot once the table becomes at least half full, or if N is not a prime number. 
   - Double hashing: use a second hash function to determine the probe distance.
     - N should be a prime.
     - Can avoid clustering.

Disadvantage of separate chaining: 

- Requires the use of an auxiliary data structure to hold entries.
- Not good if space is at a premium (e.g. writing a program for a small hand-
held device).

#### Load Factors

Load factor (λ) = n / N

n: number of data inserted into the bucket array. 

N: commonly use size of the bucket array. 

You should maintain λ < 0.9 for hash tables with separate chaining. < 0.5 for linear probing, and a bit higher for other open addressing schemes.

By default, Java’s implementation uses separate chaining with λ < 0.75.

Lower load factor indicates lower possibility of collision. 

#### Resize

Rule of thumb: once load factor >= 0.7, resize the bucket array to double size.

The new bucket array’s size is good to be a prime number approximately double the previous size.


#### Applications

- Phone directory: Name is the key and phone number is the value.
- Domain Name System (DNS): Domain name is the key and IP address is the value.
- Vote: Check if voters have voted already.
- Cache on web server. Cache web pages. URL is the key and web page is the value.

---

### Skip List

![skip-list.png](img/skip-list.png)

- Used to store key-value pair.
- Randomly decide to store duplicate elements on each level using `java.util.Random.nextBoolean()`, 1/2 possibility.
- Time complexity of search, insertion, removal: O(log n)

Optimization:

- For the horizontal axes, it is possible to keep the list singly linked.
- Do not need to store references to values at the levels of the skip list above the bottom level.
- Faster than AVL trees and other balanced
search trees.

---

### Graph

A pictorial representation of a set of objects where some pairs of objects are connected by links. Objects are represented by points termed as vertices, and the links that connect the vertices are called edges.

A graph is a pair of sets (V, E).

Vertices - represent them using an array.

Edges - represent them using a two-dimensional array.

#### Depth First Search (DFS)  

Traverse a graph in a depthward motion. Use stack to remember to get the next vertex to start a search, when a dead end occurs in any iteration.

1. Visit the adjacent unvisited vertex. Mark it as visited. Display it. Push it in a stack.
2. If no adjacent vertex is found, pop up a vertex from the stack. (It will pop up all the vertices from the stack, which do not have adjacent vertices.)

3. Repeat Rule 1 and Rule 2 until the stack is empty.

![graph-depth-first-search.png](img/graph-depth-first-search.png)

Applications:

- Produce the minimum spanning tree and all pair shortest path tree for an unweighted graph.
- Cycle detection in a graph (both undirected and directed).
- Path finding.
- Solving puzzles with only one solution, such as mazes. DFS can be adapted to find all solutions to a maze by only including nodes on the current path in the visited set.

#### Breadth First Search (BFS)  

Traverse a graph in a breadthward motion. Use a queue to remember to get the next vertex to start a search, when a dead end occurs in any iteration.

1. Visit the adjacent unvisited vertex. Mark it as visited. Display it. Insert it in a queue.
2. If no adjacent vertex is found, remove the first vertex from the queue.

3. Repeat Rule 1 and Rule 2 until the queue is empty.

![graph-breadth-first-search.png](img/graph-breadth-first-search.png)

Applications:

- Peer to Peer networks: In Peer to Peer networks like BitTorrent, BFS is used to find all neighbor nodes.
- Crawlers in search engines: Crawlers build index using BFS. The idea is to start from source page and follow all links from source and keep doing same. DFS can also be used for crawlers, but the advantage with BFS is, depth or levels of the built tree can be limited.
- Social networking websites: In social networks, we can find people within a given distance "k" from a person using Breadth First Search till "k" levels.
- GPS navigation systems: BFS is used to find all neighboring locations.
- Broadcasting in network: In networks, a broadcasted packet follows BFS to reach all nodes.
- Garbage collection: BFS is used in copying garbage collection using Cheney’s algorithm. BFS is preferred over DFS because of better locality of reference.
- Cycle detection in undirected graph.
- Path finding.

### Tree

A tree is a minimally connected graph having no loops and circuits.

![binary-tree.jpg](img/binary-tree.jpg)

Three types of traversal:

- **In-order Traversal**: the left subtree is visited first, then the root and later the right sub-tree. **Prefer this one**, as nodes can be printed in order for binary search tree.

![tree-in-order-traversal.jpg](img/tree-in-order-traversal.jpg)

- **Pre-order Traversal**: the root node is visited first, then the left subtree and finally the right subtree.

![tree-pre-order-traversal.jpg](img/tree-pre-order-traversal.jpg)

- **Post-order Traversal**: first we traverse the left subtree, then the right subtree and finally the root node.

![tree-post-order-traversal.jpg](img/tree-post-order-traversal.jpg)

How to remember："pre", "in", "post" represent the sequence of visiting root.

- pre: root -> left -> right
- in: left -> root -> right
- post: left -> right -> root

#### Binary Tree

A binary tree has a special condition that each node can have two children at maximum.

#### Full Binary Tree

If the height is N, the number of nodes is 2^N -1.

![full-binary-tree.png](img/full-binary-tree.png)

#### Binary Search Tree (BST)

- A node's left child must have a value less than or equal to its parent's value.
- A node's right child must have a value greater than its parent's value.
- Each node has a key and an associated value.
- Binary search tree is a special binary tree.

![img/binary-search-tree.jpg](img/binary-search-tree.jpg)

#### AVL Tree (Self-Balancing  Binary Search Tree)

- Height balancing binary search tree.

- Check the height of the left and the right sub-trees and assures that the difference (balance factor) is not more than 1. Otherwise, the tree is unbalanced.

Rotation techniques to balance the tree:

- Left rotation
- Right rotation
- Left-Right rotation
- Right-Left rotation

Why do we want trees to be balanced?

![balanced-vs-unbalanced-tree.png](img/balanced-vs-unbalanced-tree.png)

#### Red-Black Tree

[漫画：什么是红黑树？](https://zhuanlan.zhihu.com/p/31805309)

#### Spanning Tree

A spanning tree is a subset of Graph G, which has all the vertices covered with minimum possible number of edges.

A complete undirected graph can have maximum n^(n-2) number of spanning trees, where n is the number of nodes.  

- Removing one edge from the spanning tree will make the graph disconnected, i.e. the spanning tree is **minimally connected**.
- Adding one edge to the spanning tree will create a circuit or loop, i.e. the spanning tree is **maximally acyclic**.
- Used to find a minimum path to connect all nodes in a graph.

Common application of spanning trees:

- Civil Network Planning
- Computer Network Routing Protocol
- Cluster Analysis

##### Minimum Spanning Tree (MST)

In a weighted graph, a minimum spanning tree is a spanning tree that has minimum weight than all other spanning trees of the same graph. In real-world situations, this weight can be measured as distance, congestion, traffic load or any arbitrary value denoted to the edges.

Minimum Spanning Tree algorithms:

- Kruskal's Algorithm
- Prim's Algorithm

#### Heap

Heap is a special case of balanced binary tree data structure where root-node key is compared with its children are arranged accordingly.

##### Max Heap

The value of the parent node is greater than or equal to either of its children.

Max Heap Construction:

![max-heap-constructure.gif](img/max-heap-constructure.gif)

Max Heap Deletion:

![max-heap-deletion.gif](img/max-heap-deletion.gif)

##### Min Heap

The value of the parent node is less than or equal to either of its children.

---

## Algorithms

### Basic Concepts

#### Algorithm Complexity

Two main factors decide the efficiency of algorithm:
- Time Factor: counting the number of key operations.
- Space Factor: counting the maximum memory space required by the algorithm.

#### Asymptotic Analysis

Refers to computing the running time of any operation in mathematical units of computation.

The time required by an algorithm falls under three types:

- Best Case - minimum time required for program execution.
- Average Case - average time required for program execution.
- Worst Case - maximum time required for program execution.

Asymptotic Notations:

- Omega Notation **Ω(n)** - measures the **best case** time complexity or the best amount of time.
- Big O Notation **Ο(n)** - measures the **worst case** time complexity or the longest amount of time.
- Theta Notation **θ(n)** - **average case**, express both the lower bound and the upper bound of an algorithm's running time.

#### Big O Complexity Chart

![big-o-complexity-chart.png](img/big-o-complexity-chart.png)

#### Big O Cheat Sheet

![big-o-cheat-sheet.png](img/big-o-cheat-sheet.png)

#### Approaches to Develop Algorithms

##### Greedy Algorithms

localized optimum solution

Most networking algorithms use the greedy approach:

- Travelling Salesman Problem
- Prim's Minimal Spanning Tree Algorithm
- Kruskal's Minimal Spanning Tree Algorithm
- Dijkstra's Minimal Spanning Tree Algorithm
- Graph - Map Coloring
- Graph - Vertex Cover
- Knapsack Problem
- Job Scheduling Problem

##### Divide and Conquer

The algorithms based on divide-and-conquer programming approach:

- Merge Sort
- Quick Sort
- Binary Search
- Strassen's Matrix Multiplication
- Closest pair (points)

##### Dynamic Programming

overall optimization

For problems which can be divided into **similar** sub-problems, so that their results can be re-used.

Use the output of a smaller sub-problem and then try to optimize a bigger sub-problem.

Use Memorization to remember the output of already solved sub-problems.

Problems solved using dynamic programming approach:

- Fibonacci number series
- Knapsack problem
- Tower of Hanoi
- All pair shortest path by Floyd-Warshall
- Shortest path by Dijkstra
- Project scheduling

---

### Bit Operations 

`<<`: left shift

`>>`: signed right shift

`>>>`: unsigned right shift, pad 0 for both positive and negative numbers.

Example: 

`00001010 << 2 = 00101000`

For signed right shift, 

- if the number is unsigned, pad 0 at the most left side. E.g., `00001010 >> 2 = 00000010`.
- if the number is signed and negative, pad 1 at the most left side. E.g., `10001010 >> 3 = 11110001`.

How to figure out whether the most right side is 1 or not: do xor between this number and 1. If the result is 1, yes; if 0, no.

For positive numbers, mathematically, `>> 1` is the same as `/ 2` and `<< 1` is the same as `* 2`, but with better performance. **Recommend** using this one in practice.

`(<number> & 1) == 1` can be used to determine whether a number is odd or not. (instead of using `% 2 == 1`)

---

### Expression Parsing

- Infix Notation - human readable
- Prefix (Polish) Notation - operator is ahead
- Postfix (Reverse-Polish) Notation - operator is after

![expression-parsing.png](img/expression-parsing.png)

---

### Searching Algorithms

Options: 

- linear search
- binary search
- hash table search
- binary search tree search


#### Linear Search  

Find an item in a sequentially arranged data type. Compare expected data item with each of data items in list or array.

average - Ο(n)

worst - O(n^2)

Work better finding all matched values in an array.

#### Binary Search

Select the middle which splits the entire list into two parts. Compare the target value to the mid of the list.

```java
// commonly used, but bad
// because if the values low and high are big, may overflow
int mid = (low + high) / 2;

// better way
int mid = (high - low) / 2 + low;

// mathematically, >> 1 is the same as / 2, but better performance
// better way
int mid = ((high - low) >> 1) + low;
```

Time complexity: Ο(log n)

Work better finding one match if there is no duplicates in the array.

**The array must be sorted in advance.**

#### Inverted Index

[什么是倒排索引？](https://www.cnblogs.com/zlslch/p/6440114.html)

---

### Sorting Algorithms

In-place Sorting vs. Not-in-place Sorting

Stable vs. Not Stable Sorting

- Stable sorting sort repeated elements in the same order that they appear in the input.

Adaptive vs. Non-Adaptive Sorting

- Adaptive: if the source list has some element already sorted, it will try not to re-order them.
- Non-Adaptive Sorting: force every single element to be re-ordered, not taking into account the elements which are already sorted.

[排序算法总结](https://www.runoob.com/w3cnote/sort-algorithm-summary.html)

[十种常见排序算法的思想、应用场景](https://blog.csdn.net/hairy_monsters/article/details/80154391)

#### Bubble Sort

A comparison-based algorithm in which each pair of adjacent elements is compared and elements are swapped if they are not in order.

N elements need N-1 loops.

- time - O(n^2) - not suitable for large set of data. :-1:
- space - O(1)

#### Insertion Sort

Divide the list into two sub-list: sorted and unsorted. Then it takes one element at a time and finds an appropriate location in sorted sub-list and insert it.

worst - O(n^2)

stable

1. a[2]跟a[1]比，if a[2] < a[1]，a[1]的值赋给a[2]。
2. a[2]跟a[0]比，if a[2] < a[0]，a[0]的值赋给a[1]。a[2]的值赋给a[0]。

把小的值向后覆盖掉大的值。最终把最小值赋给第一个值。

#### Selection Sort

Divide the list into two sub-lists: sorted and unsorted. Then it selects the minimum element from unsorted sub-list and places it into the sorted list. This iterates unless all the elements from unsorted sub-list are placed into sorted sub-list.

worst - O(n^2)

not stable

1. Mark a[0] as min。
2. a[0]跟a[1]比，if a[0] > a[1]，这时不要交换a[0]和a[1]。而是mark a[1] as min。
3. a[1]跟a[2]比，if a[1] > a[2]，mark a[2] as min。
4. 一趟loop完后，拿到min。这时将min和a[0]交换。再从a[1]开始重复这个过程。

找min，一趟loop完后再做交换。

More efficient than bubble sort. Less swap.

#### Merge Sort

Divide and conquer approach. Keep on dividing the list into smaller sub-list until all sub-list has only one element. Then it merges them in a sorted way until all sub-lists are sorted.

O(n log n)

space - O(n)

#### Shell Sort

A variant of insertion sort. Divide the list into smaller sub-list based on some gap variables. Sort each sub-list using insertion sort.

best - O(n log n)

1. Initialize the value of interval h. (h = h * 3 + 1) (O(n^(3/2)) by Knuth, 1973: 1, 4, 13, 40, 121, ...)
2. Divide the list into smaller sub-list of equal interval h.
3. Sort these sub-lists using insertion sort.
4. Reduce h, divide sub-lists into smaller one. Sort.
5. Repeat step 4 until h is 1. Final insertion sort.

![img/shell-sort.jpg](img/shell-sort.jpg)

![img/shell-sort-2.jpg](img/shell-sort-2.jpg)

#### Quick Sort

Divide and conquer approach. Divide the list in smaller "partitions" using "pivot". The values which are smaller than the pivot are arranged in the left partition and greater values are arranged in the right partition. Each partition is recursively sorted using quick sort.

1. Randomly choose a value in the array as pivot.
2. Compare the a[0] and pivot, a[n] and pivot. If a[0] > pivot, a[n] < pivot, swap a[0] and a[n].
3. Compare a[1] and pivot, a[n-1] and pivot. If a[1] < pivot, a[n-1] < pivot, compare a[2] and pivot. If a[2] > pivot, swap a[2] and a[n-1].
4. When all elements <= pivot are at the left and all elements >= pivot are at the right (这里只是将array以pivot为基准分成大的小的两组，不一定是pivot左边的都小于pivot，右边的都大于pivot), split them as two sub-lists. Then do the above steps recursively until the sub-list cannot be split again.  

Pivot selection:

- If pick the last or first element as the pivot, susceptible to the worst case O(n^2), most notably when the original sequence is already sorted, reverse sorted, or nearly sorted.
- In practice, **"median-of-three"** technique, use median of the front, middle, and tail of the sequence.

Hybrid approach:

- Quick sort has **good performance on large data sets**, but **high overhead on small data sets**. 
- When the size of a subsequence falls below some threshold (perhaps 50 elements), use insertion sort. 


[Good explanation](https://www.youtube.com/watch?v=SLauY6PpjW4&list=PLX6IKgS15Ue02WDPRCmYKuZicQHit9kFt&t=0s&index=20)

---

#### Sorting Algorithms Complexity Table

![array-sorting-algorithms-complexity.png](img/array-sorting-algorithms-complexity.png)

![array-sorting-algorithms-complexity-2.png](img/array-sorting-algorithms-complexity-2.png)

---

### Recursion

If something is repeated, you should think about applying recursion algorithm to it.

A recursive function is one which calls itself, directly or calls a function that in turn calls it.

Properties:

- Base criteria: where function stops calling itself recursively.
- Progressive approach: where function tries to meet the base criteria in each iteration.

Many programming languages implement recursion by means of stacks.

![recursion.jpg](img/recursion.jpg)

**Advantages**:

- It makes a program more readable.
- More efficient than iterations because of latest enhanced CPU systems.

**Disadvantages**:

- Space complexity of recursive function may go higher than that of a function with iteration, because the system needs to store activation record each time a recursive call is made.
- Time and space consumption: Every time the function is invoked, it needs to create space on stack to store parameters, returned address and temp variables; pushing and popping data also needs time.
- If time of recursion is big, performance will be low. There can also be stack overflow problem because stack size is limited.

#### Tower of Hanoi

![tower-of-hanoi.jpg](img/tower-of-hanoi.jpg)

Tower of Hanoi puzzle with n disks can be solved in minimum
$$
2^n-1
$$
steps.

Algorithm ideas:

- Divide the stack of disks in two parts:
  - n^th disk
  - all other n-1 disks
- The ultimate aim is to move disk n^th from source to destination and then put all other n-1 disks onto it.

Steps:

1. Move n-1 disks from source to aux.
2. Move n^th disk from source to destination.
3. Move n-1 disks from aux to destination.

#### Fibonacci Series

1,1,2,3,5,8,13,21 ...

f(n) = f(n-1) + f(n-2)

```c
// this code is just used to demonstrate recursion algorithm
// should not be used in projects due to low performance
int fibbonacci(int n) {
   if(n == 0) {
      return 0;
   } else if(n == 1) {
      return 1;
   } else {
      return (fibonacci(n-1) + fibonacci(n-2));
   }
}
```

#### Factorial (n!)

```c
int factorial(int n) {
   //base case
   if(n == 0) {
      return 1;
   } else {
      return n * factorial(n-1);
   }
}
```
