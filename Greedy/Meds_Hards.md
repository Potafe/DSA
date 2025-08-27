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
________________________________________________

#### Ans 4:
________________________________________________

#### Ans 5:
________________________________________________

#### Ans 6:
________________________________________________

#### Ans 7:
________________________________________________

#### Ans 8:
________________________________________________

#### Ans 9:
________________________________________________

#### Ans 10:
________________________________________________

#### Ans 11:
________________________________________________