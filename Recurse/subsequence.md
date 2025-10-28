### Problems

1. [Generate parenthesis](#ans-1)
2. [Generate all binary string](#ans-2)
3. [Print all subsequences](#ans-3)
4. [Check if there exists a subsequences with sum k](#ans-4)
5. [Count all subsequences with sum k](#ans-5)
6. [Combination sum](#ans-6)
7. [Combination sum - II](#ans-7)
8. [Subset sum](#ans-8)
9. [Subset sum - II](#ans-9) 10.[Combination sum - III](#ans-10) 11.[Letter combination of a phone number](#ans-11)

### Answers

#### Ans 1.

We first understand the recursive tree of this question:

The tree will have the following choices:

- pick open bracket
- pick closed bracket

What we will need is:

1. Two indexes -> one for open brackets and closed brackets
2. vector<int> ans -> to store all the strings generated
3. string x -> we will append or remove from this the brackets

That's it!

Now let us see how to proceed with the algo:

1. Define a base case:
   a. Base case being -> if open and closed brackets == n: push x to ans and return
2. Now we define 2 cases:
   a. If open < n -> we pick a open bracket
   b. If closed < n -> we pick a closed bracket

```cpp
vector<int> Generate_Parentheses(int n) {
    vector<int> ans;

    recursion(ans, "", n, 0, 0);

    return ans;
}

void recursion(vector<int> &ans, string x, int n, int open, int closed) {
    if (open == n && closed == n) {
        ans.push_back(x);
        return;
    }

    if (open < n) {
        recursion(ans, x + "(", n, open + 1, closed);
    }

    if (closed < n) {
        recursion(ans, x + ")", n, open, closed + 1);
    }
}

```

---

#### Ans 2.

If we want to generate all binary strings all we have to is use the above algo again in a different way now.

-> Here we will have to choose to pick either a '1' or '0' while keeping the size of string as n.

-> For the base case we check if the ones and zeros are total of size n.

```cpp

void recurse(vector<string> &ans, int ones, int zeros, int n, string x) {
    if (ones == n && zeros == n) {
        ans.push_back(x);
        return;
    }

    if (ones < n) {
        recurse(ans, ones + 1, zeros, n, x + '1');
    }

    if (zeros < n) {
        recurse(ans, ones, zeros + 1, n, x + '0');
    }
}

vector<string> binstr(int n) {
    vector<string> ans;
    recurse(ans, 0, 0, n, "");
    sort(ans.begin(), ans.end());
    return ans;
}

```

---

#### Ans3.

To generate all subsequences pattern of a given array, we have to understand the recursion tree:

1. We will always have 2 choices:
   -> pick a index.
   -> not pick a index and skip.

2. So now we define our requirements:
   -> A vector<vector<int>> ans -> we will push copy to ans.
   -> Index to keep track -> to keep track of our nums array.
   -> A vector<int> copy -> we will push/skip indexes into this array.

3. Now we define our cases:
   -> Base case: index > size(nums) -> we push copy to ans and return
   -> We push nums(index) to copy and move to next index.
   -> We pop the last index from copy and move to next index without picking any number.

```cpp
void recurse(vector<vector<int>> &ans, int index, vector<int> &copy, vector<int> nums) {
    if (index >= nums.size()) {
        ans.push_back(copy);
        return;
    }

    //pick number
    copy.push_back(nums[index]);
    recurse(ans, index + 1, copy, nums);
    copy.pop_back();

    // not pick number
    recurse(ans, index + 1, copy, nums);
}

vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> ans;
    vector<int> copy;

    recurse(ans, 0, copy, nums);

    return ans;
}
```

---

#### Ans 4.

We just have to check if there exists a subsequences with sum k:

-> The brute force recursive way is to generate all the subsequences with the sum == k

-> If they exist in our final vector<vector> we return true.

-> The more optimal solution is a DP one which we will discuss later.

```cpp
void recurse(vector<vector<int>> &ans, vector<int> &copy, vector<int> &nums, int index, int sum) {
    if (index >= ans.size() && sum == 0) {
        ans.push_back(copy);
        return;
    }

    copy.push_back(nums[index]);
    recurse(ans, copy, nums, index + 1, sum -= nums[index]);
    copy.pop_back();
    recurse(ans, copy, nums, index + 1, sum -= nums[index]);
}

bool checkSubsequenceSum(vector<int> nums, int k) {
    // we will define the needed variables and data structures (not writing them for brevity)
    recurse(ans, copy, nums, 0, k);
    if (ans.size() > 0) return true;
    return false;
}

```

---

#### Ans 5.

-> Not going to solve this one as this is a similar version of the above question.

-> In this case we just return the size of the ans array.

---

#### Ans 6.

We have to return subsequences that sum up to a particular target.

-> The logic remains the same -> we either pick a number or skip that number.

-> In this case we can pick the number more than once if needed (the needed case being -> if num < target)

-> So we will write it in this way:

-> Base Case: if index > n and target == 0 -> push copy to ans and return

-> Pick Case: while num < target -> we pick num and recurse on the same index

-> Not Pick Case: skip the current num and pick the num from next index

```cpp
void recurse(vector<vector<int>> &ans, vector<int> &copy, vector<int> nums, int index, int target) {
    if (index >= nums.size() && target == 0) {
        ans.push_back(copy);
        return;
    }

    if (nums[index] < target) {
        copy.push_back(nums[index]);
        recurse(ans, copy, nums, index, target -= nums[index]);
        copy.pop_back();
    }

    recurse(ans, copy, nums, index + 1, target);
}

vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    // we define all the needed data structures and variables
    recurse(and, copy, nums, 0, target);

    return ans;
}


```

---

#### Ans 7.

We basically have to now return the ans array from above question, but in this case the subsequences should be unique.

1. So we first sort the array.

2. Now we call our recursion function:
   - The base case of the recursion function is basically if target_sum == 0 -> we push copy to ans and return
   - Now we loop from each index and in this loop we call the recursion function again for the index after that:
     - In the loop we first check if the previous_index and the current_index values are not equal.
     - We also check if the current_value is not greater than the target_sum.
     - We then push current_value to the copy.
     - We now call the recursion function for the next index.

```cpp

void recursion(int index, vector<int> nums, int target, vector<vector<int>>& ans, vector<int>& copy) {
    if (target == 0) {
        ans.push_back(copy);
        return;
    }

    for (int i = index; i < n; i++) {
        if (i > index && nums[i] == nums[i - 1]) continue;

        if (nums[i] > target) break;

        copy.push_back(nums[i]);

        recursion(i + 1, nums, target, ans, copy);

        copy.pop_back();
    }
}

vector<vector<int>> combinationSum2(vector<int>& nums, int target) {
    sort(nums.begin(), nums.end());

    vector<vector<int>> ans;
    vector<int> copy;

    solve(0, nums, target, ans, copy);
    return ans;
}

```

---

#### Ans 8.

Simple recursion question:

1. We either pick a number and add it our sum.
2. Or we don't pick that number (skip it) and move on to our next index.

```cpp
void recursion(vector<int> &ans, vector<int>& arr, int index, int sum) {
    if (index >= arr.size()) {
        ans.push_back(sum);
        return;
    }

    //pick
    sum += arr[index];
    recursion(ans, arr, index + 1, sum);
    sum -= arr[index];

    //not pick
    recursion(ans, arr, index + 1, sum);
}

vector<int> subsetSums(vector<int>& arr) {
    vector<int> ans;

    recursion(ans, arr, 0, 0);

    return ans;
}

```

---

#### Ans 9.

Now we have to return all the "UNIQUE" subsets of the nums array.

1. We can basically do what we did in our combinationSum2 question:
   - We define our recursion which basically has a loop from the current_index to the end of the nums array:
     - This loop will work exactly like it did in the combinationSum2 question.
   - The base case is we push the copy to the ans array.

```cpp

void recursion(vector<vector<int>> &ans, vector<int> arr, int index, vector<int> &copy) {
    ans.push_back(copy);

    for (int i = index; i < arr.size(); i++) {
        if (i != index && arr[i] == arr[i - 1]) continue;

        copy.push_back(arr[i]);

        recursion(ans, arr, i + 1, copy);

        copy.pop_back();
    }
}

vector<vector<int>> subsetsWithDup(vector<int>& nums) {
    sort(nums.begin(), nums.end());

    vector<vector<int>> ans;
    vector<int> copy;

    recursion(ans, nums, 0, copy);


    return ans;
}

```

---

#### Ans 10.

---

#### Ans 11.

---
