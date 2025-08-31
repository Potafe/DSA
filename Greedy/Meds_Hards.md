## Problems:

1. [N meetings in one room](#ans-1)
2. [Jump Game](#ans-2)
3. [Jump Game 2](#ans-3)
4. [Minimum number of platforms required for a railway](#ans-4)
5. [Job sequencing Problem](#ans-5)
6. [Candy](#ans-6)
7. [Program for Shortest Job First (or SJF) CPU Scheduling](#ans-7)
8. [Program for Least Recently Used (LRU) Page Replacement Algorithm](#ans-8)
9. [Insert Interval](#ans-9)
10. [Merge Intervals](#ans-10)
11. [Non-overlapping Intervals](#ans-11)


## Answers:

#### Ans 1:
    
    1. We store the meeting start and end times in a array
    2. Sort array by end time
    3. Check if start time > end time:
        -> If yes -> increase number of meetings held

```cpp
int maxMeetings(vector<int>& start, vector<int>& end) {    
    vector<vector<int>> k;
    
    for (int i = 0; i < start.size(); i++) {
        k.push_back({end[i], start[i]});
    }
    
    sort(k.begin(), k.end());
    
    int m = 0;
    int l = -1;
    
    for (int i = 0; i < k.size(); i++) {
        if (k[i][1] > l) {
            m++;
            l = k[i][0];
        }
    }
    
    return m;
}
```
________________________________________________

#### Ans 2:

    Exactly the reason Greedy Sucks (bcoz I never thought of this :)):
        1. We keep a maxIndex
        2. We update this maxIndex according to the max 
           we can reach from a index (so current_index + jump[current_index]) 
        3. If current_index > maxIndex
            -> we return false as we can never reach that index using the jumps

```cpp
bool canJump(vector<int>& nums) {
    int maxIndex = 0;

    for(int i = 0; i < nums.size(); i++){
        // If the current index is greater
        // than the maximum reachable index
        // it means we cannot move forward
        // and should return false
        if (i > maxIndex){
            return false;
        }

        // Update the maximum index
        // that can be reached by comparing
        // the current maxIndex with the sum of
        // the current index and the
        // maximum jump from that index
        maxIndex = max(maxIndex, i + nums[i]);
    }
    
    // If we complete the loop,
    //it means we can reach the
    // last index
    return true;    
}
```
________________________________________________

#### Ans 3:

    farthest: Tracks the farthest index we can reach by the current position.

    current_end: Marks the farthest index reachable with the current number of jumps.

    Loop: We iterate over the array, updating farthest at each step. 
          If we reach current_end, we increment the jump counter and update 
          current_end to the farthest position reached.

    Exit Condition: The loop stops before the last index since once we reach it, 
                    we don't need to make any more jumps.

```cpp
int jump(vector<int>& nums) {
    int jumps = 0;
    int current_end = 0;
    int farthest = 0;

    for (int i = 0; i < nums.size() - 1; i++) {
        farthest = max(farthest, i + nums[i]);
    
        // When we reach the end of the current range, we make a jump
        if (i == current_end) {
            jumps++;
            current_end = farthest;
        }
    }  

    return jumps;
}
```
________________________________________________

#### Ans 4:
    1. In this problem we have an arrival and depart array.
    2. Each array represents arrive and depart of independent trains.
    3. To count the number of platforms needed:
        -> sort the arrays
        -> Just keep two pointers i and j one for each array
        -> Traverse both the arrays:
            -> If departure time [j] > arrival time [i]:
                -> Increment platforms needed
                -> Increment i till we reach a higher arrival time

```cpp
int findPlatform(vector<int>& arr, vector<int>& dep) {
    sort(arr.begin(), arr.end());
    sort(dep.begin(), dep.end());
    
    int i = 0; 
    int j = 0;
    int p = 0;
    
    while (i < arr.size() && j < dep.size()) {
        if (dep[j] >= arr[i] && j < dep.size()) {
            p++;
            i++;
        }
        
        else {
            i++;
            j++;
        }
    }
    
    return p;
}
```
________________________________________________

#### Ans 5:

##### INTUITIVE WAY:
    1. Sort the array by profit.
    2. Now init a slot array (with size = max deadline).
    3. Assign the slot if it's == -1.
    4. Count the jobs by checking if slot[i] != -1.
```cpp  
vector<int> jobSequencing(vector<int> &deadline, vector<int> &profit) {
    vector<vector<int>> s;

    int t = -1;
    
    for (int i = 0; i < profit.size(); i++) {
        if (deadline[i] > t) t = deadline[i];
        s.push_back({profit[i], deadline[i]});
    }
    
    sort(s.begin(), s.end(), [](auto &a, auto &b) {
        return a[0] > b[0];
    });
    
    vector<int> slot(t + 1, -1);
    
    for (int i = 0; i < s.size(); i++) {
        int d = s[i][1];
        int p = s[i][0];
    
        for (int j = d; j > 0; j--) {
            if (slot[j] == -1) {
                slot[j] = p;
                break;
            }
        }
    }
    
    int countJobs = 0, totalProfit = 0;
    for (int i = 1; i <= t; i++) {
        if (slot[i] != -1) {
            countJobs++;
            totalProfit += slot[i];
        }
    }
    
    return {countJobs, totalProfit};
}       
```

##### DISJOINT SET WAY:
    1. Sort jobs by profit descending.
    2. Use DSU where find(x) returns the latest available free slot ≤ x.
    3. If find(d) gives 0, no slot is available.
    4. Otherwise, schedule the job there and update parent using union.
##### DRY RUN:

    Input:
    deadline = [1, 2, 2, 3]
    profit   = [10, 20, 30, 50]

    jobs = [(10,1), (20,2), (30,2), (50,3)]
    jobs = [(50,3), (30,2), (20,2), (10,1)]


    maxDeadline = 3
    DSU parent = \[0,1,2,3]

    Meaning:

    * slot 1 is free → parent\[1] = 1
    * slot 2 is free → parent\[2] = 2
    * slot 3 is free → parent\[3] = 3

    Process each job

    Job (50,3)
        Check latest available slot ≤ 3
        → find(3) = 3 (slot 3 is free)
        Assign job → profit = 50, count = 1
        Merge → mark slot 3 as filled, link it to 2
        → merge(3, 2) → parent\[3] = 2
    parent now: [0,1,2,2]

    ...

    Job (10,1)
        Check slot ≤ 1
        → find(1) → parent[1] = 0 → slot unavailable
        Skip
    parent now: [0,0,1,2]

```cpp
class DSU {
public:
    vector<int> parent;

    DSU(int n) {
        parent.resize(n + 1);
        for (int i = 0; i <= n; i++) parent[i] = i;
    }

    // Find latest available slot
    int find(int x) {
        if (x == parent[x]) return x;
        return parent[x] = find(parent[x]); // Path compression
    }

    // Union: mark slot x as filled → link it to (x-1)
    void merge(int u, int v) {
        parent[u] = v;
    }
};

class Solution {
  public:
    vector<int> jobSequencing(vector<int> &deadline, vector<int> &profit) {
        int n = profit.size();
        vector<pair<int,int>> jobs; // {profit, deadline}
        int maxDeadline = 0;
    
        for (int i = 0; i < n; i++) {
            jobs.push_back({profit[i], deadline[i]});
            maxDeadline = max(maxDeadline, deadline[i]);
        }
    
        sort(jobs.begin(), jobs.end(), [](auto &a, auto &b) {
            return a.first > b.first; // sort by profit desc
        });
    
        DSU dsu(maxDeadline);
    
        int countJobs = 0, totalProfit = 0;
    
        for (auto &job : jobs) {
            int d = job.second;
            int p = job.first;
    
            int available = dsu.find(d);
            if (available > 0) { // slot available
                countJobs++;
                totalProfit += p;
                dsu.merge(available, dsu.find(available - 1));
            }
        }
    
        return {countJobs, totalProfit};
    }
};
```
________________________________________________

#### Ans 6:

    For this we just do a 2 pass solution:

    1. We mantain a candies array with default value = 1
    2. We now update the candies by checking the ratings of the prev index
    3. Return the sum of candies array

```cpp
int candy(vector<int>& ratings) {
    int n = ratings.size();
    vector<int> candies(n, 1);

    for (int i = 1; i < n; i++) {
        if (ratings[i] > ratings[i - 1]) {
            candies[i] = candies[i - 1] + 1;
        }
    }  

    for (int i = n - 1; i > 0; i--) {
        if (ratings[i - 1] > ratings[i]) {
            candies[i - 1] = max(candies[i] + 1, candies[i - 1]);
        }
    }   

    int c = 0;

    for (int num: candies) {
        c += num;
    }

    return c;
}
```
________________________________________________

#### Ans 7:
    BURST TIME = How long a process takes to run.
    WAITING TIME = For a process, it’s the time spent waiting in the ready queue before it gets CPU time.
    WAITING TIME OF A PROCESS = START TIME − ARRIVAL TIME


```cpp
long long solve(vector<int>& bt) {
    sort(bt.begin(), bt.end());
    vector<int> w;
    
    int ans = 0;
    w.push_back(ans);
    
    for (int i = 0; i < bt.size() - 1; i++) {
        ans += bt[i];
        w.push_back(ans);        
    }
    
    // cout << "size: " << w.size() << endl;
    
    ans = 0;
    
    for (int n: w) {
        ans += n;    
    }
    
    return floor(ans/bt.size());
}
```

##### OPTIMIZED VERSION:

    WE JUST CALCULATE ON THE GO, INSTEAD OF STORING IN AN ARRAY

```cpp
long long solve(vector<int>& bt) {
    sort(bt.begin(), bt.end());
    
    long long c = 0;
    long long ans = 0;

    for (int i = 0; i < bt.size() - 1; i++) {
        c += bt[i];
        ans += c;
    }
    
    // cout << ans << endl;
    
    return floor(ans/bt.size());
}
```
________________________________________________

#### Ans 8:
NOT DONE YET
________________________________________________

#### Ans 9:
1. **Initialization**

   * Extract `start` and `end` from `newInterval`.
   * We’ll try to place this interval inside the sorted `intervals` vector.

2. **Iterating through intervals (using iterator `it`)**
   For each interval in `intervals`:

   * **Case 1: New interval ends before current interval begins**

     ```cpp
     if (end < (*it)[0])
     ```

     * Example: new interval `[2,3]`, current interval `[5,6]`.
     * Since it doesn’t overlap and comes **before**, insert `{start, end}` at this position.
     * Return result immediately (no need to check further).

   * **Case 2: New interval starts after current interval ends**

     ```cpp
     else if (start > (*it)[1])
     ```

     * Example: new interval `[7,8]`, current interval `[5,6]`.
     * Since new interval is **completely after**, just move forward (`++it`).

   * **Case 3: Overlap case**

     ```cpp
     else
     ```

     * Example: new interval `[2,6]`, current interval `[4,5]`.
     * They overlap, so we **merge**:

       * `start = min(start, (*it)[0])`
       * `end = max(end, (*it)[1])`
     * Remove the current interval (`erase(it)`) because it is absorbed into the merged interval.

3. **After loop ends**

   * If we didn’t return inside the loop, it means either:

     * `newInterval` comes **after all intervals**
     * Or it was merged into something that extends till the end.
   * In both cases, push the merged `{start, end}` to the back.
	
```cpp
vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
	int n = intervals.size();

	int start = newInterval[0];
	int end = newInterval[1];

	for (auto it = intervals.begin(); it != intervals.end();) {
		if (end < (*it)[0]) {
			intervals.insert(it, {start, end});
			return intervals;
		} 
		else if (start > (*it)[1]) {
			++it;
		} 
		else {
			start = min(start, (*it)[0]);
			end = max(end, (*it)[1]);
			it = intervals.erase(it);
		}
	}

	intervals.push_back({start, end});
	return intervals;
}
```
________________________________________________

#### Ans 10:

    Simple Logic:

    If end of array 'i' is greater than array 'i + 1': 
	 -> these array will be merged to one = [arr[i][0], arr[i + 1][1]]

    Example:
        intervals = [[1,3],[2,6]]
	
        array[0][1] = 3 
        array[1][0] = 2
        -> merged array [arr[i][0], arr[i + 1][1]] = [1, 6]

```cpp
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end());
    
    int j = 0;
    for (int i = 0; i < intervals.size() - 1l) {
        if (intervals[i][1] > intervals[i + 1][0]) {
            intervals[i][1] = max(intervals[i][1], intervals[i + 1][1]);
            intervals.erase(intervals.begin() + i + 1);
        }

        else {
            i++;
        }
    } 
}
```

________________________________________________

#### Ans 11:
	1. Sort by end time
	2. If we find a start time < prev end time -> count++
	3. Else new prev end = current end time
	
```cpp
 int eraseOverlapIntervals(vector<vector<int>>& it) {
    sort(it.begin(), it.end(), [](auto &a, auto &b) {
        return a[1] < b[1];
    });

    int count = 0;  
    int prevEnd = it[0][1];

    for (int i = 1; i < it.size(); i++) {
        if (it[i][0] < prevEnd) {
            // overlap → remove this one
            count++;
        } else {
            // no overlap → update prevEnd
            prevEnd = it[i][1];
        }
    }
    
    return count;
}
```
________________________________________________