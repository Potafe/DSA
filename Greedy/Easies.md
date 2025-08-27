## Problems:

1. [Assign Cookies](#ans-1)
2. [Fractional Knapsack Problem](#ans-2)
3. [Greedy algorithm to find minimum number of coins](#ans-3)
4. [Lemonade Change](#ans-4)
5. [Valid Paranthesis Checker](#ans-5)

## Answers:

#### Ans 1.
    We first sort both the cookies and greed arrays:
        1. We assign based on greed <= size of cookie:
            -> If yes increment the pointer of greed.
            -> Else increment the pointer of cookie size.
    
    Return greed pointer.

```cpp
int findContentChildren(vector<int>& g, vector<int>& s) {
    sort(g.begin(), g.end());
    sort(s.begin(), s.end());

    int r = 0;
    int l = 0;

    while (l < s.size() && r < g.size()) { 
        if (g[r] <= s[l]) {
            r++;
        }

        l++;
    } 

    return r;
}
```
________________________________________________

#### Ans 2.
    1. We sort the array according to value-weight.
    2. We then fill the ans if we can take the current capacity.
    3. If not we take the ratio of the val/weight * current capacity.
    4. Return the ans.

```cpp
bool comp(vector<int>& arr, vector<int>& brr) {
    double a1 = (1.0 * arr[0]) / arr[1];
    double b1 = (1.0 * brr[0]) / brr[1];
    return a1 > b1;
}

double fractionalKnapsack(vector<long long>& val, vector<long long>& wt, long long capacity) {
    int n = val.size();

    vector<vector<int>> items(n, vector<int>(2));

    for (int i = 0; i < n; i++) {
        item[i][0] = val[i];
        items[i][1] = wt[i];
    }

    sort(items.begin(), items.end(), comp);

    double ans = 0.0;
    int currentCapacity = capacity;
    
    for (int i = 0; i < n; i++) {
        if (items[i][1] <= currentCapacity) {
            ans += items[i][0];
            currentCapacity -= items[i][1];
        }

        else {
            ans += (1.0 * items[i][0] / items[i][1]) * currentCapacity;
            break;
        }
    }
    
    return ans;
}
```
________________________________________________

#### Ans 3.

##### THIS QUESTION IS ACTUALLY A DP QUESTION. IGNORE IT
________________________________________________

#### Ans 4.
    Really intuitive if you just understand the question,
    just keep track of 10 and 5 dollar bill.

```cpp
bool lemonadeChange(vector<int>& bills) {
    if (bills[0] > 5) return false;

    int lb = 0;
    int hb = 0;
    
    for (int i = 0; i < bills.size(); i++) {
        if (bills[i] == 5) lb++;
        else if (bills[i] == 10) {
            if (lb >= 1) {
                lb--;
                hb++;
            }
            else {
                // cout << "here";
                return false;
            }
        }
        else {
            if (lb >= 1 && hb >= 1) {
                lb--;
                hb--;
            }
            else if (lb >= 3) {
                lb -= 3;
            }
            else {
                // cout << i;
                return false;
            }
        }
    }    

    return true;
}
```
________________________________________________

#### Ans 5.
    What we basically do is:
    1. Init two stacks one for left brackets and one for stars
    2. Pop from stack 1 or stack 2 when we find a right bracket
    3. If both stacks are still non empty:
        a. Try to find if stack 2 top > stack 1 top:
            -> if yes pop from stack 1
            -> else break loop
    4. Return (stack 1 == empty)  

```cpp
bool checkValidString(string s) {
    stack<int> s1;
    stack<int> s2;

    for (int i = 0; i < s.size(); i++) {
        if (s[i] == '(') {
            s1.push(i);
        }

        else if (s[i] == '*') {
            s2.push(i);
        }

        else {
            if (!s1.empty()) {
                s1.pop();
            }

            else if (!s2.empty()) {
                s2.pop();
            }

            else return false;
        }
    }    

    while (!s1.empty() && !s2.empty()) {
        if (s1.top() < s2.top()) {
            s1.pop();
            s2.pop();
        }

        else {
            break;
        }
    }

    return s1.empty();

}
```
________________________________________________