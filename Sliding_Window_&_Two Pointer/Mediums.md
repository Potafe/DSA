## Problems:

1. [Longest Substring Without Repeating Characters](#ans-1)
2. [Max Consecutive Ones III](#ans-2)
3. [Fruit Into Baskets](#ans-3)
4. [Longest Repeating Character Replacement](#ans-4)
5. [Binary subarray with sum](#ans-5)
6. [Count number of nice subarrays](#ans-6)
7. [Number of substring containing all three characters](#ans-7)
8. [Maximum point you can obtain from cards](#ans-8)


## Solutions:

#### Ans 1.

    0. Have 2 pointers:
        -> left and right.
    1. Just insert string[right] into a set.
    2. If not found -> length = right - left + 1.
    3. If found: 
        -> remove element s[left]
        -> update the left pointer (left++)

```cpp
int lengthOfLongestSubstring(string s) {
        set<char> t;
        int left = 0;
        int maxi = 0;

        for (int right = 0; right < s.size(); right++) {
            if (t.count(s[right]) == 0) {
                t.insert(s[right]);
                maxi = max(right - left + 1, maxi);
            }

            else {
                while (t.count(s[right]) > 0) {
                    t.erase(s[left]);
                    left++;
                }
            }

            t.insert(s[right]);
        }    

        return maxi;
    }
```
________________________________

#### Ans 2.
    1. We maintain 2 pointers left and right.
    2. We also have a zeroes counter.
    3. If zeroes > limit -> we increment left
    4. Update the max length accordingly.

```cpp
int longestOnes(vector<int>& nums, int k) {
    int n = nums.size();
    int maxi = 0;

    int i = 0;
    int no_of_zeroes = 0;

    for (int j = 0; j < nums.size(); j++) {
        if (nums[j] == 0) no_of_zeroes++;

        while (no_of_zeroes > k) {
            if (nums[i] == 0) no_of_zeroes--;
            i++;
        }

        maxi = max(maxi, j - i + 1);
    }   

    return maxi;
}
```
________________________________

#### Ans 3.

________________________________

#### Ans 4.

________________________________

#### Ans 5.

________________________________

#### Ans 6.

________________________________

#### Ans 7.

________________________________

#### Ans 8.

________________________________