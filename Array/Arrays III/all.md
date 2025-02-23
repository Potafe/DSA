## Problems:

1. [Reverse Pairs](#ans-1)
2. [Search in a 2D matrix](#ans-2)
3. [Pow(x, n)](#ans-3)
4. [Majority Element (>n/2 times)](#ans-4)
5. [Majority Element (n/3 times)](#ans-5)
6. [Grid Unique Paths](#ans-6)

## Solutions:

#### Ans 1.

```cpp
    int reversePairs(vector<int>& nums) {
        return mergeSort(0, nums.size() - 1, nums);
    }

    int mergeSort(int lo, int hi, vector<int>& arr) {
        int count = 0;
        if (lo >= hi) return count;
        int mid = lo + (hi - lo) / 2;
        count += mergeSort(lo, mid, arr);
        count += mergeSort(mid + 1, hi, arr);
        count += countPairs(arr, lo, mid, hi);
        merge(lo, mid, hi, arr);
        return count;
    }

    int merge(int low, int mid, int high, vector<int>& arr) {
        int left = low;
        int right = mid + 1;
        vector<int>temp;

        int count = 0;

        while(left <= mid && right <= high) {
            if (arr[left] < arr[right]) {
                temp.push_back(arr[left]);
                left++;
            }
            else if (arr[left] >= arr[right]) {
                temp.push_back(arr[right]);
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

    int countPairs(vector<int>&arr, int low, int mid, int high){
        int count = 0;
        int right = mid+1;
        for(int i = low;i <= mid; i++){
            while (right <= high && (long long)arr[i] > 2LL * arr[right]) right++;
            count += (right - (mid+1));
        }
        return count;
    }
```
________________________________
#### Ans 2.

```cpp
int binJ(vector<int>& test, int target) {
    int left = 0;
    int right = test.size() - 1;
    
    while(left <= right) {
        int mid = (left + right) / 2;
        if (test[mid] == target) {
            return mid;
        }
        
        else if (target > test[mid]) {
            left = mid + 1;
        }
        
        else if (target < test[mid]) {
            right = mid - 1;
        }
    }
    
    return -1;
}

int binI(vector<vector<int>>& test, int target, int n) {
    int left = 0;
    int right = test.size() - 1;
    
    int midi = 0;
    while(left <= right) {
        int mid = (left + right) / 2;
        if (test[mid][0] <= target && test[mid][n-1] >= target) {
            return mid;
        }
        
        if (target > test[mid][0]) {
            left = mid + 1;
        }
        
        
        if (target < test[mid][0]) {
            right = mid - 1;
        }
    }
    return -1;
}


bool searchMatrix(vector<vector<int>>& matrix, int target) {
    //will have binary search;
    int i = binI(matrix, target, matrix[0].size());
    if (i == -1) return false;
    int j = binJ(matrix[i], target);
    if (j == -1) return false;
    return true; 
}
```
________________________________
#### Ans 3.

```cpp
double power(double x, long n) {
    if (n == 0) return 1;

    if (n < 0) return 1 / (power(x, -n));

    if (n % 2 == 0) return power(x * x, n / 2);

    return x * (power(x * x, (n - 1) / 2));    
}

double myPow(double x, int n) {
    return power(x, static_cast<long>(n));       
}
```
________________________________
#### Ans 6.

```cpp
int uniquePaths(int m, int n) {
    long long result = 1;

    for (int i = 1; i <= m - 1; ++i) {
        result *= (n + i - 1);
        result /= i;
    }

    return result;
}
```