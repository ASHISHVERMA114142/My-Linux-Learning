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
