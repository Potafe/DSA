## Problems:
	
1. [Introduction to Priority Queues using Binary Heaps](#ans-1)
2. [Min Heap and Max Heap Implementation](#ans-2)
3. [Check if an array represents a min-heap or not](#ans-3)
4. [Convert min Heap to max Heap](#ans-4)

## Solutions:

### Ans 1

#### Binary Heap

A **Binary Heap** is a complete binary tree that stores data efficiently, allowing quick access to the maximum or minimum element, depending on the type of heap. It can be either a **Min Heap** or a **Max Heap**.

________________________________

#### 1.1 Types of Heaps

* **Min Heap**: The key at the root must be the smallest among all the keys in the heap, and this property must hold recursively for all nodes.

* **Max Heap**: The key at the root must be the largest among all the keys in the heap, and this property must hold recursively for all nodes.

________________________________

#### 1.2 Properties

1. **Complete Binary Tree**: A Binary Heap is a complete binary tree. This means all levels are fully filled, except possibly the last level, which is filled from left to right.

2. **Array Representation**: A Binary Heap is typically represented as an array. The root element is at `arr[0]`.

________________________________

#### 1.3 Index Relationships

In the array representation of a Binary Heap, the following relationships hold for the `i-th` node:

| Parent         | Left Child     | Right Child    |
| -------------- | -------------- | -------------- |
| `arr[(i-1)/2]` | `arr[(2*i)+1]` | `arr[(2*i)+2]` |

________________________________

#### 1.4 Traversal Method

The traversal method used to achieve array representation is **Level Order Traversal**.

________________________________

#### 1.5 Applications of Binary Heap

________________________________

##### 1.5.1 Heap Sort

Heap Sort uses a Binary Heap to sort an array in **O(n log n)** time.

________________________________

##### 1.5.2 Priority Queue

Priority Queues can be efficiently implemented using Binary Heaps because they support the following operations in **O(log N)** time:

* `insert()`
* `delete()`
* `extractmax()`
* `decreaseKey()`

________________________________

##### 1.5.3 Variations of Binary Heap

* **Binomial Heap** and **Fibonacci Heap** are variations of Binary Heaps. These variations perform **union** operations efficiently.

________________________________

##### 1.5.4 Graph Algorithms

Priority Queues are especially useful in Graph Algorithms like:

* **Dijkstra's Shortest Path**
* **Prim's Minimum Spanning Tree**

##### 1.5.5 Differencr from other structures

| Feature        | Heap (Min/Max)                    | Binary Search Tree (BST)    | Sorted Array |
| -------------- | --------------------------------- | --------------------------- | ------------ |
| Tree Shape     | Complete Binary Tree              | Binary Tree (not complete)  | Flat         |
| Order          | Heap Property only (parent-child) | All left < root < all right | Fully sorted |
| Fast Access To | Root only (min or max)            | Any element (via search)    | Any element  |
| Insertion Time | O(log n)                          | O(log n)                    | O(n)         |
| Find Max/Min   | O(1)                              | O(log n)                    | O(1)         |

##### 1.5.6 Common Heap Operations

| Operation                       | Time Complexity | Description                                 |
| ------------------------------- | --------------- | ------------------------------------------- |
| `insert()`                      | O(log n)        | Add a new element and restore heap property |
| `extractMax()` / `extractMin()` | O(log n)        | Remove root and reheapify                   |
| `heapify()`                     | O(log n)        | Rebuild heap from an index                  |
| `buildHeap()`                   | O(n)            | Heapify an entire array (bottom-up)         |

________________________________

### Ans 2. 

- MinHeap Implementation:
- For a Min Heap: arr[i] ≤ arr[2*i + 1] and arr[i] ≤ arr[2*i + 2]

```cpp
class MinHeap {
    vector<int> heap;

    int parent(int i) {
        return (i - 1) / 2;
    }

    int left(int i) {
        return 2 * i + 1;
    }

    int right(int i) {
        return 2 * i + 2;
    }

    void heapifyDown(int i) {
        int l = left(i);
        int r = right(i);
        int smallest = i;

        if (l < heap.size() && heap[l] < heap[smallest]) {
            smallest = l;
        }

        if (r < heap.size() && heap[r] < heap[smallest]) {
            smallest = r;
        }

        if (smallest != i) {
            swap(heap[i], heap[smallest]);
            heapifyDown(smallest);
        }
    }

    void heapifyUp(int i) {
        while (i != 0 && heap[parent(i)] > heap[i]) {
            swap(heap[i], heap[parent(i)]);
            i = parent(i);
        }
    }


    public:

        MinHeap() {}

        void initHeap() {
            for (int i = heap.size() / 2; i >= 0; i--) {
                heapifyDown(i);
            }
        }

        void insert(int key) {
            heap.push_back(key);
            heapifyUp(heap.size() - 1);
        }

        int getMin() {
            if (heap.empty()) {
                throw underflow_error("Heap is empty");
            }
            return heap[0];
        }

        bool isEmpty() {
            return heap.empty();
        }

        void extractMin() {
            heap[0] = heap.back();
            heap.pop_back();
            
            if (!heap.empty()) {
                heapifyDown(0);
            }
        }

        void changeKey(int index, int newKey) {
            int old_key = heap[index];
            heap[index] = newKey;

            if (newKey > oldKey) {
                heapifyDown(index);
            }

            else {
                heapifyUp(index);
            }
        }
}
```

- MaxHeap Implementation:
- For a Max Heap: arr[i] ≥ arr[2*i + 1] and arr[i] ≥ arr[2*i + 2]

```cpp
class MaxHeap {
    vector<int> heap;

    int parent(int i) {
        return (i - 1) / 2;
    }

    int left(int i) {
        return 2 * i + 1;
    }

    int right(int i) {
        return 2 * i + 2;
    }

    void heapifyDown(int i) {
        int l = left(i);
        int r = right(i);
        int largest = i;

        if (l < heap.size() && heap[l] > heap[largest]) {
            largest = l;
        }

        if (r < heap.size() && heap[r] > heap[largest]) {
            largest = r;
        }

        if (largest != i) {
            swap(heap[i], heap[largest]);
            heapifyDown(largest);
        }
    }

    void heapifyUp(int i) {
        while (i != 0 && heap[parent(i)] < heap[i]) {
            swap(heap[i], heap[parent(i)]);
            i = parent(i);
        }
    }


    public:

        MaxHeap() {}

        void initHeap() {
            for (int i = heap.size() / 2; i >= 0; i--) {
                heapifyDown(i);
            }
        }

        void insert(int key) {
            heap.push_back(key);
            heapifyUp(heap.size() - 1);
        }

        int getMax() {
            if (heap.empty()) {
                throw underflow_error("Heap is empty");
            }
            return heap[0];
        }

        bool isEmpty() {
            return heap.empty();
        }

        void extractMax() {
            heap[0] = heap.back();
            heap.pop_back();
            
            if (!heap.empty()) {
                heapifyDown(0);
            }
        }

        void changeKey(int index, int newKey) {
            int old_key = heap[index];
            heap[index] = newKey;

            if (newKey < oldKey) {
                heapifyDown(index);
            }

            else {
                heapifyUp(index);
            }
        }
}
```
________________________________

### Ans 3. 

- MinHeap Implementation:
- For a Min Heap: arr[i] ≤ arr[2*i + 1] and arr[i] ≤ arr[2*i + 2]

```cpp
bool isMinHeap (vector<int> arr) {
    int n = arr.size();

    for (int i = n / 2 - 1; i >= 0; i--) {
        int left = 2 * i + 1;
        int right = 2 * i +  2;

        if (left < n && arr[i] > arr[left]) return false;
        if (right < n && arr[i] > arr[right]) return false;
    }

    return true;
}
```

- MaxHeap Implementation:
- For a Max Heap: arr[i] ≥ arr[2*i + 1] and arr[i] ≥ arr[2*i + 2]

```cpp
bool isMaxHeap (vector<int> arr) {
    int n = arr.size();

    for (int i = n / 2 - 1; i >= 0; i--) {
        int left = 2 * i + 1;
        int right = 2 * i +  2;

        if (left < n && arr[i] < arr[left]) return false;
        if (right < n && arr[i] < arr[right]) return false;
    }

    return true;
}
```
________________________________

### Ans 4. 
________________________________

