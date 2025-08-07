## Problems:
	
1. [Introduction to Priority Queues using Binary Heaps](#ans-1)
2. [Min Heap and Max Heap Implementation](#ans-2)
3. [Check if an array represents a min-heap or not](#ans-3)
4. [Convert min Heap to max Heap](#ans-4)

## Solutions:

#### Ans 1. 

#### Binary Heap

A **Binary Heap** is a complete binary tree that stores data efficiently, allowing quick access to the maximum or minimum element, depending on the type of heap. It can be either a **Min Heap** or a **Max Heap**.

#### Types of Heaps

* **Min Heap**: The key at the root must be the smallest among all the keys in the heap, and this property must hold recursively for all nodes.

* **Max Heap**: The key at the root must be the largest among all the keys in the heap, and this property must hold recursively for all nodes.

#### Properties

1. **Complete Binary Tree**: A Binary Heap is a complete binary tree. This means all levels are fully filled, except possibly the last level, which is filled from left to right.

2. **Array Representation**: A Binary Heap is typically represented as an array. The root element is at `arr[0]`.

#### Index Relationships

In the array representation of a Binary Heap, the following relationships hold for the `i-th` node:

| Parent         | Left Child     | Right Child    |
| -------------- | -------------- | -------------- |
| `arr[(i-1)/2]` | `arr[(2*i)+1]` | `arr[(2*i)+2]` |

#### Traversal Method

The traversal method used to achieve array representation is **Level Order Traversal**.

#### Applications of Binary Heap

#### Heap Sort

Heap Sort uses a Binary Heap to sort an array in **O(n log n)** time.

#### Priority Queue

Priority Queues can be efficiently implemented using Binary Heaps because they support the following operations in **O(log N)** time:

* `insert()`
* `delete()`
* `extractmax()`
* `decreaseKey()`

#### Variations of Binary Heap

* **Binomial Heap** and **Fibonacci Heap** are variations of Binary Heaps. These variations perform **union** operations efficiently.

#### Graph Algorithms

Priority Queues are especially useful in Graph Algorithms like:

* **Dijkstra's Shortest Path**
* **Prim's Minimum Spanning Tree**

________________________________

#### Ans 2. 
________________________________
#### Ans 3. 
________________________________
#### Ans 4. 
________________________________

