## Problems:

1. [Implement Stack using Arrays](#ans-1)

2. [Implement Queue using Arrays](#ans-2)

3. [Implement Stack using Queue](#ans-3)

4. [Implement Queue using Stack](#ans-4)

5. [Implement stack using Linkedlist](#ans-5)

6. [Implement queue using Linkedlist](#ans-6)

7. [Check for balanced paranthesis](#ans-7)

8. [Implement Min Stack](#ans-8)


## Solutions:

#### Ans 1.
    Just have a top variable and an array:
        1. For push -> top++;
        2. For pop -> top--;

```cpp
    void MyStack ::push(int x) {
    // Your Code
    top++;
    arr[top] = x;
    }

    int MyStack ::pop() {
        if (top == -1) return -1;
        return arr[top--];
    }
```
________________________________

#### Ans 2.
    Have a front and rear variable.
```cpp
    void MyQueue ::push(int x) {
        arr[rear] = x;
        rear++;
    }

    int MyQueue ::pop() {
        if (front == rear) return -1;
        return arr[front++];
    }
```
________________________________
#### Ans 3.
    
```cpp
    queue<int> q1;
    queue<int> q2;

    MyStack() {
        
    }
    
    void push(int x) {
        q2.push(x);

        while (!q1.empty()) {
            q2.push(q1.front());
            q1.pop();
        }

        swap(q1, q2);
    }
    
    int pop() {
        int popped = q1.front();
        q1.pop();
        return popped;
    }
    
    int top() {
        return q1.front();    
    }
    
    bool empty() {
        if (q1.size() == 0) return true;
        return false;    
    }
```
________________________________
#### Ans 4.
    -> Basically use two stacks:
        1. Push all elements in that are already in stack 1 --> stack 2
        2. Then push new element to stack 1
        3. Then empty stack 2 --> stack 1
```cpp
    stack<int> stk;
    stack<int> stk2;
    
    void push(int x) {
        while (stk.size()) {
            stk2.push(stk.top());
            stk.pop();
        }   

        stk.push(x);

        while (stk2.size()) {
            stk.push(stk2.top());
            stk2.pop();   
        }
    } 
    
    int pop() {
        int front = stk.top();
        stk.pop();

        return front;
    }
    
    int peek() {
        return stk.top();
    }
    
    bool empty() {
        return stk.size() == 0;
    }

```
________________________________
#### Ans 5.

________________________________
#### Ans 6.

________________________________
#### Ans 7.

________________________________
#### Ans 8.
    Solution 1 -> maintain pair<int, int> stack
```python
    class MinStack(object):

    def __init__(self):
        self.stack = []    

    def push(self, val):
        """
        :type val: int
        :rtype: None
        """
        mini = self.getMin()
        print(mini)
        if mini == None or mini > val:
            mini = val
        
        self.stack.append([val, mini])
        

    def pop(self):
        """
        :rtype: None
        """
        self.stack.pop()
        

    def top(self):
        """
        :rtype: int
        """
        return self.stack[-1][0] if self.stack else None
        

    def getMin(self):
        """
        :rtype: int
        """
        return self.stack[-1][-1] if self.stack else None
        
```

    Solution 2 -> is unintuitve
```cpp
    stack<long long> stk;
    long long min = INT_MIN;
    
    MinStack() {
        
    }
    
    void push(int val) {
        if (stk.empty()) {
            stk.push(val);
            min = val;
        }    

        else {
            if(val > min) stk.push(val);
            else {
                stk.push((val - min+val));
                min = val;
            }
        }
    }
    
    void pop() {
        long long x = stk.top();
        stk.pop();   

        if (min > x) min = 2*min - x; 
    }
    
    int top() {
        long long x = stk.top();
        if (x < min) return min;
        
        else return x;
    }
    
    int getMin() {
        return min;
    }
```

________________________________

