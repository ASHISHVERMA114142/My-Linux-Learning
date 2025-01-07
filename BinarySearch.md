# Binary Search 
There are many features of the binary search algorithm . 

### 1. If array is sorted . 
Ex : below code will find exactly target element is inside array or not .    
LT - 704  
LT - 33  
```java
    public int binarySearch(int[] nums, int target) {
        int left=0,right=nums.length-1;
        while(left<=right){
            int mid=left+(right-left)/2;
            if(nums[mid]==target) return mid;
            else if(nums[mid]<target) left=mid+1;
            else right=mid-1;
        }
        return -1;
    }
```
### 2. for finding next greater element or just less element inside array . 
let say we have sorted array that are arranged in increasing order 
nums= 1,3,5,9  
target = 2  
In this case returning "left" at the end will give us either target or just greater element that is present on the array .  
Returning "right" will give us either target or just smaller element that is present on the array .  
LT - 35  
```java
    public int searchInsert(int[] nums, int target) {
        int left=0,right=nums.length-1;
        while(left<=right){
            int mid=left+(right-left)/2;
            if(nums[mid]==target) return mid;
            else if(nums[mid]<target) left=mid+1;
            else right=mid-1;
        }
        return right;
    }
```

