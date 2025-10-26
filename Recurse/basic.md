## Problems:

1. [Recursive Implementation of Atoi()](#ans-1)
2. [Implementation Pow(x, n)](#ans-2)
3. [Count Good Numbers](#ans-3)
4. [Sort a stack using recursion](#ans-4)
5. [Reverse a stack using recursion](#ans-5)

## Answers:

Not really a recursion only question at first, as we can solve it using
iterative method also (which looks more intuitive)

But to solve it using recursion we can do the following way:

1. First traverse index till there is no space.
2. Next traverse index till there is a number (notice the sign of the number mentioned also)
3. Now call our recursive method to calculate the number.

#### Ans 1.

```cpp
int atoi(string &s) {
    int index = 0;

    while (index < n && s[index] == ' ') index++;
    if (s[index] == "-") sign = -1, index++;
    else sign = +1, index++;

    recursiveAtoi(index, sign, s, 0)
}

int recursiveAtoi(int index, int sign, string s, int ans) {
    if (index > n || s[index] < '0' || s[index] > '0') return sign * ans;

    ans = recursiveAtoi(index + 1, sign, s, (ans * 10 + (s[index] - '0')));

    return ans;
}

```

---

#### Ans 2.

We just need to notice this simple pattern:

1. x ^ n can be written as:
   x ^ n = x _ (x ^ n - 1) # if n is not a power of 2.
   OR
   x ^ n = (x _ x) ^ (n / 2) # if n is a power of 2.

With the above idea we can simply write a basic recursion with a base case
of n == 0 -> we return 1.

NOTE: If power is negative, number becomes reciprocal:
x ^ -1 = 1 / x

```cpp

int pow(int x, int n) {
    if (n == 0) return 1;

    else if (n < 0) return 1 / power(x, -n);

    else if (n % 2 == 0) {
        return power(x * x, n / 2);
    }

    else return x * power(x * x, ((n - 1 / 2)));
}

int ourPow(int x, int n) {
    return pow(x, n);
}

```

---

#### Ans 3.

A digit string is called good if: 1. At even indices it is even 2. At odd indices it is prime

What we have to do is: 1. Given a number n -> return total good numbers of length n

Brute Force solution: 1. We loop till 10 ^ n. 2. We check for the above conditions (if they hold true) 3. If hold true -> cnt += 1

Optimal solution:

-> Even indices: digit must be even: {0,2,4,6,8} -> 5 choices
-> Odd indices: digit must be odd: {2,3,5,7} -> 4 choices

-> Now for a given "n": 1. We have `((n + 1) / 2)` number of even indices. 2. We have `(n / 2)` number of odd indices.

    Example:
        -> n = 5
        -> indices = 0, 1, 2, 3, 4 -> even = 3, odd = 2

-> Thus total combinations available is:
-> `Total = ((5 ^ even_indices_count) * (4 ^ odd_indices_count))`

-> Now if n == even:
-> since even_indices_count == odd_indices_count
-> `Total = (20 ^ (even_indices_count / 2)) % MOD`

-> Else if n == odd:
-> `Total = (5 * (20 ^ (even_indices_count / 2))) % MOD`

```cpp
int count_good_numbers(long long n) {
    long long ans = pow(x, n / 2);

    if (n & 1) ? (ans * 5) % MOD : ans;
}

int pow(int x, int n) {
    if (n == 0) return 1;

    else if (n % 2 == 0) return (power(x*x, (n / 2)) % MOD);

    else return (x * power((x * x), ((n - 1) / 2)) % MOD);
}

```

---

#### Ans 4.

To sort a stack using recursion we do the following:

1. Define a sort method -> that pops the element from the stack, recursively sorts the remaining stack and calls the insert_stack method to place the popped element in order.
2. Now we define the insert_stack method that recursively pops the element from the stack and puts them back into their right position.

As we can see above the time complexity of the above is O(n^2) as the popped element might need to traverse the entire stack to get inserted into the correct position.

```cpp

int sort_stack(stack<int> &s) {
    if (s.empty()) return;

    int top_el = s.top();
    s.pop();

    sort_stack(s);

    insert_stack(s, top_el);
}

void insert_stack(stack<int> &s, int el) {
    if (s.empty() || el >= s.top()) {
        s.push(el);
        return;
    }

    int temp_el = s.top();
    s.pop();

    insert_stack(s, el);

    s.push(temp_el);
}
```

---

#### Ans 5.

```

```

---
