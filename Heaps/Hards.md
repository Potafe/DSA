## Problems:

1. [Design Twitter](#ans-1)
2. [Connect n Ropes with Minimal Cost](#ans-2)
3. [Kth Largest Element in a Stream of Running Integers](#ans-3)
4. [Maximum Sum Combination](#ans-4)
5. [Find Median from Data Stream](#ans-5)
6. [K Most Frequent Elements](#ans-6)


## Solutions:

### Ans 1

1. We keep track of tweets according to userId, time and tweetId
2. We also keep track of each followers of a userId
3. In the heap we push according to time, tweetId
4. Now we get the recent tweets

```cpp
class Twitter {
    int time;
    
    unordered_map<int, unordered_set<int>> relation; 

    unordered_map<int, vector<pair<int, int>>> tweets; // Stores tweets for each user (time, tweetId)

public:
    Twitter() {
        time = 0;
    }
    
    void postTweet(int userId, int tweetId) {
        tweets[userId].push_back({time, tweetId});
        time++;
    }
    
    vector<int> getNewsFeed(int userId) {
        vector<int> ans;
        priority_queue<pair<int, int>> maxHeap;

        // Add user's own tweets to the heap
        if (tweets.find(userId) != tweets.end()) {
            for (auto& tweet : tweets[userId]) {
                maxHeap.push(tweet);
            }
        }

        // Add tweets of followees to the heap
        if (relation.find(userId) != relation.end()) {
            for (int followeeId : relation[userId]) {
                if (tweets.find(followeeId) != tweets.end()) {
                    for (auto& tweet : tweets[followeeId]) {
                        maxHeap.push(tweet);
                    }
                }
            }
        }

        // Extract the top 10 tweets
        int count = 0;
        while (!maxHeap.empty() && count < 10) {
            ans.push_back(maxHeap.top().second);
            maxHeap.pop();
            count++;
        }

        return ans;
    }
    
    void follow(int followerId, int followeeId) {
        if (followerId != followeeId) {
            relation[followerId].insert(followeeId);
        }
    }
    
    void unfollow(int followerId, int followeeId) {
        if (relation.find(followerId) != relation.end()) {
            relation[followerId].erase(followeeId);
        }
    }
};
```
________________________________

### Ans 2

________________________________

### Ans 3

1. We keep the size of the pq only k.

```cpp
class KthLargest {
    priority_queue<int, vector<int>, greater<int>> pq;
    int kth;
public:
    KthLargest(int k, vector<int>& nums) {
        kth = k;

        for (int num : nums) {
            add(num);
        } 
    } 
    
    int add(int val) {
        pq.push(val);
        
        if (pq.size() > kth) {
            pq.pop();
        }
        
        return pq.top();
    }
};
```
________________________________

### Ans 4

________________________________

### Ans 5

________________________________

### Ans 6

1. We store the numbers in the pq according to their frequency.

```cpp
vector<int> topKFrequent(vector<int>& nums, int k) {
    vector<int> ans;
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;

    unordered_map<int, int> mp;
    for (int num : nums) {
        mp[num]++;
    }

    for (auto [num, fr] : mp) {
        pq.push({fr, num});
        if (pq.size() > k) {
            pq.pop();
        }
    }


    while (!pq.empty()) {
        ans.push_back(pq.top().second);
        pq.pop();
    }

    return ans;
}
```
________________________________



