## Heap 
There are two types of head min-heap and max-heap 
1. min-heap - smallest element will be on the top
2. max-heap - largest element will be on the top

## cpp and jave 
``` cpp
priority_queue<int> q; - max heap
priority_queue<int,vector<int>,greater<int>> q - min heap
```
``` java
PriorityQueue<Integer> q =new PriorityQueue<>(); -> min heap
PriorityQueue<Integer> q = new PriorityQueue<>(Collections.reverseOrder()); -> max heap
PriorityQueue<Integer> q = new PriorityQueue<>( ( a, b) -> a-b ); -> PriorityQueue with compartor 
```
## concepts 
1. kth largest element - use min heap here
2. kth smalltest element - use max heap here
3. kth greater element - use min heap
4. kth smallest element - use max heap
5. kth sortest array -
Question - Given an array arr[], where each element is at most k away from its target position, you need to sort the array optimally.
solution - in this case we use min-heap as we have sorted postion that is just k away from the current location so we have to create min-heap of size "k" once the size will become "k+1" then we will pop the element so that the top most element will be the first postion as it is the smalltest and every element in the array is just "k" distance from its position .
6. 
