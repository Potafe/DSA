## Problem:

1. [Merge Sort](#ans-1)

## Solution:

#### Ans 1.

Steps:

1. Recursive sort left half
2. Recursive sort right half
3. Total solve both sorted halves.

![alt text](/Sorting/images/image.png)

```cpp
merge(l, r, mid, arr) {
    temp = {};
    left = l;
    right = mid + 1;

    while (left <= mid and right <= r) {
        if (arr[left] <= arr[right]) {
            temp.push(arr[left]);
            left++;
        }
        else {
            temp.push_back(arr[right]);
            right--;
        }
    } 

    while (left <= mid) {
        temp.push_back(arr[left]);
        left++;
    }

    while (right <= r) {
        temp.push_back(arr[right]);
        right++;
    }

    for (int i = low; i <= high; i++) {
        arr[i] = temp[i - low];
    }   
}

mergeSort(l, r, arr) {
    if (l >= r) return;
    mergeSort(l, mid, arr);
    mergeSort(mid + 1, r, arr);
    merge(l, r, mid, arr);
}
```
________________________________






