## Problems:

1. [Kth Largest Element in an Array (using Priority Queue)](#ans-1)
2. [Kth Smallest Element in an Array (using Priority Queue)](#ans-2)
3. [Sort K Sorted Array](#ans-3)
4. [Merge M Sorted Lists](#ans-4)
5. [Replace Each Array Element by its Corresponding Rank](#ans-5)
6. [Task Scheduler](#ans-6)
7. [Hands of Straights](#ans-7)


## Solutions:

### Ans 1

NOT EVEN GOING TO SOLVE THIS 
________________________________

### Ans 2

NOT EVEN GOING TO SOLVE THIS 
________________________________

### Ans 3

The approach is basic:

1. We store elements in the priority_queue for size k + 1 only.
2. Now we replace the elements in the array, while also entering the current element in the pq also.

```cpp
void nearlySorted(vector<int>& arr, int k) {
    priority_queue<int, vector<int>, greater<int>> pq;

    for (int i = 0; i <= k; i++) {
        pq.push(arr[i]);
    }
    
    int index = 0;
    for (int i = k + 1; i < arr.size(); i++) {
        arr[index++] = pq.top();
        pq.pop();
        pq.push(arr[i]);
    }
    
    while (pq.size()) {
        arr[index++] = pq.top();
        pq.pop();
    }
}
```

________________________________

### Ans 4

The approach is simple:

1. We push all the elements in the pq.
2. Reconstruct a new LL using the pq.

```cpp
ListNode* mergeKLists(vector<ListNode*>& lists) {
    priority_queue<int, vector<int>, greater<int>> q;

    for (int i = 0; i < lists.size(); i++) {
        ListNode* it = lists[i];
        while (it != nullptr) {
            q.push(it->val);
            it = it->next;
        }
    }

    ListNode* ans = new ListNode();
    ListNode* cur = ans;

    while (!q.empty()) {
        cur->next = new ListNode(q.top());
        q.pop();
        cur = cur->next;
    }

    return ans->next;    
}
```
________________________________

### Ans 5

________________________________

### Ans 6

- Using priority queue

1. First count the number of occurrences of each task and store that in a map/vector.

2. Then push the count into a priority queue, so that the maximum frequency task can be accessed and completed first.

3. Then until all tasks are completed (i.e the priority queue is not empty):
intialise the cycle length as n+1. n is the cooldown period so the cycle will be of n+1 length.

> Example: for ['A','A','A','B','B'] and n=2,
the tasks can occur in the following manner:
[A B idle]->[A B idle]->[A]. See here each cycle is n+1 length long, only then A can repeat itself.

4. for all elements in the priority queue, until the cycle length is exhausted, pop the elements out of the queue and if the task is occurring more than once then add it to the remaining array (which stores the remaining tasks).

5. This means that we are completing that task once in this cycle.So keep counting the time.

6. Then, add the occurrence of tasks back to the priority queue.

7. Add the idle time into the time count. Idle time is the time that was needed in the cycle because no task was available. It is the remaining cycle length in our algorithm. Idle time should be only added if the priority queue is empty (i.e all tasks are completed).

SMALL DRY RUN IF NEEDED:

> tasks = ["A","A","A","B","B","B"], n = 2

```
ITERATION 1

pq = [3, 3]
cycle = 3

pq = [3]
max_fr = 3
remain.push(2)
time = 1
cycle = 2

pq = []
max_fr = 3
remain.push(2)
time = 2
cycle = 1

pq = [2, 2]
time = 3

ITERATION 2

pq = [2, 2]
cycle = 3

pq = [2]
max_fr = 2
remain.push(1)
time = 4
cycle = 2

pq = []
max_fr = 2
remain.push(1)
time = 5
cycle = 1

pq = [1, 1]
time = 6

ITERATION 3

pq = [1, 1]
cycle = 3

pq = [1]
max_fr = 1
time = 7
cycle = 2

pq = []
max_fr = 1
time = 8
cycle = 1

time = 8
```

```cpp
int leastInterval(vector<char>& tasks, int n) {
    priority_queue<int> pq; 

    vector<int> mp(26);

    for (char i: tasks) {
        mp[i - 'A']++;
    }    

    for (int i = 0; i < mp.size(); i++) {
        if (mp[i]) pq.push(mp[i]);
    }

    int time = 0;

    while (pq.size()) {
        int cycles = n + 1;
        vector<int> remain;
        while (cycles && pq.size()) {
            int max_fr = pq.top();
            if (max_fr > 1) remain.push_back(max_fr - 1);
            time++;
            cycles--;
            pq.pop();
        }

        for (int max_frs: remain) pq.push(max_frs);
        if (pq.empty()) break;
        time += cycles;
    }

    return time;
}
```
________________________________

### Ans 7

#### This is more of a greedy problem:
1. We init a map (not an unordered_map) and count the frequency of each element.
2. Now we start a for loop (till groupsize) and find the next consecutive element (while also reducing their frequency).
3. We do this for every num in the map.

```cpp
bool isNStraightHand(vector<int>& hand, int groupSize) {
    if (hand.size() % groupSize) return false;

    map<int, int> mp;

    for (int i = 0; i < hand.size(); i++) {
        mp[hand[i]]++;
    }

    for (auto [num, count]: mp) {
        if (count == 0) continue;

        int need = count;
        for (int i = 0; i < groupSize; i++) {
            if (mp[num + i] < need) return false;
            mp[num + i] -= need;
        }
    }

    return true;
}   
```
________________________________