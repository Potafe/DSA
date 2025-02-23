## Problems:

1. [Count Inversions](#ans-1)
2. [Repeat and Missing Number](#ans-2)
3. [Rotate Matrix](#ans-3)
4. [Merge Overlapping Subintervals](#ans-4)
5. [Merge two sorted arrays without extra space](#ans-5)
6. [Find the duplicate in an array of N+1 integers](#ans-6)
7. [Rotate Image](#ans-3)

## Solutions:

#### Ans 1.

1. Use mergesort.

```cpp

long long getInversions(long long *arr, int n){
    return mergeSort(0, n - 1, *arr);
}

long long mergeSort(int low, int high, long long *arr) {
    long long count = 0;
    if (low >= hi) return count;
    int mid = (lo + hi) / 2;
    count += mergeSort(low, mid, arr);
    count += mergeSort(mid + 1, high, arr);
    count += merge(low, mid, high, arr);
    return count;
}

long long merge(int low, int mid, int high, long long *arr) {
    int left = low;
    int right = mid + 1;
    temp = {};

    long long count = 0;

    while(left <= mid && right <= high) {
        if (arr[left] < arr[right]) {
            temp.push_back(arr[left]);
            left++;
        }
        else if (arr[left] >= arr[right]) {
            temp.push_back(arr[right]);
            count++;
            right++;
        }
    }

    while (left <= mid) {
        temp.push_back(arr[left]);
        left++;
    }

    while (right <= high) {
        temp.push_back(arr[right]);
        right++;
    }

    for (int i = low; i <= high; i++) {
        arr[i] = temp[i - low];
    }   

    return count;
}

```
________________________________

#### Ans 2.

```cpp

vector<int> missingAndDouble() {
    int arr[] = {3,1,2,5,3};
    int n = sizeof(arr)/sizeof(arr[0]);
    int xr = 0;
    
    // XOR OF ARR[i] and (1 -> N); 
    for(int i = 0; i < n; i++) {
        xr ^= (arr[i] ^ (i+1));
    }
    
    //FIND BIT WHICH IS 1;
    int number = (xr & ~(xr - 1));
    
    //ONE IS MISSING OTHER IS DOUBLE;
    int zero = 0;
    int one = 0;
    
    for (int i = 0; i < n; i++) {
        if ((arr[i] & number) != 0) {
            one = one ^ arr[i];
        }
        else {
            zero = zero ^ arr[i];
        }
    }
    
    for (int i = 1; i <= n; i++) {
        if ((i & number) != 0) {
            one = one ^ i;
        }
        else {
            zero = zero ^ i;
        }
    }
    
    //CHECK DOUBLE ELEMENT;
    int cnt = 0;
    for (int i = 0; i < n; i++) {
        if (arr[i] == zero) cnt++;
    }

    cout << one << " " << zero;

}
```
________________________________
