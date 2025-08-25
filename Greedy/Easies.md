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
________________________________________________

#### Ans 3.
________________________________________________

#### Ans 4.
________________________________________________

#### Ans 5.
________________________________________________