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
    MyStack() {
        
    }
    
    void push(int x) {
        
    }
    
    int pop() {
        
    }
    
    int top() {
        
    }
    
    bool empty() {
        
    }
```
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

