# Java collections 

## Priority Queue 
Java’s java.util package includes the PriorityQueue class, implementing a priority queue via a binary heap. Elements within this priority queue are ordered based on their natural ordering or a provided comparator.
It is easy to create "min heap " in java ,
Example : 
  PriorityQueue<Integer> q = new PriorityQueue<>();

But in order to create Max Heap you have to create Comparator there are few ways to do that . 
1. By extending comparator class - We can simple extend Comparator class that is present inside java.util.Comparator
   Ex-
   ''' java
   import java.util.Comparator;
   public class MyComparator extends Comparator<Integer> {
     public int compare(Integer x, Integer y){
     return y-x;
     }
   }
   
   To call this comparator we have to provide initial size of the PriorityQueue that will extend in future with Object of your comparator .

   PriorityQueue < Integer > q = new PriorityQueue<>( size , new MyComparator() ) ;

3. Insted of creating our custom comparator we can use Collections.reverseOrder() funtion to get the Max Heap .
   Ex-

   PriorityQueue <Integer> q =new PriorityQueue<>(size, Collections.reverseOrder());

4. Using lamda expression on the PriorityQueue instead of creating any kind of Comparator .
   Ex -

   PriorityQueue < Integer > q = new PriorityQueue<> ( size , ( o1 , o2 ) -> o2 - o1 );


## some common methods are 
| Method Name            | Use                                                                                       |
|------------------------|-------------------------------------------------------------------------------------------|
| `add(E e)`              | Inserts the specified element into the priority queue. If the element is `null`, it throws `NullPointerException`. |
| `offer(E e)`            | Inserts the specified element into the priority queue, returning `false` if the element cannot be added. |
| `peek()`                | Retrieves, but does not remove, the highest-priority element in the queue. Returns `null` if the queue is empty. |
| `poll()`                | Retrieves and removes the highest-priority element in the queue. Returns `null` if the queue is empty. |
| `remove()`              | Removes a specific element from the priority queue. Returns `true` if the element was removed, `false` otherwise. |
| `clear()`               | Removes all elements from the priority queue. |
| `size()`                | Returns the number of elements in the priority queue. |
| `isEmpty()`             | Returns `true` if the priority queue is empty, otherwise returns `false`. |
| `contains(Object o)`    | Returns `true` if the specified element is present in the queue, otherwise `false`. |
| `toArray()`             | Returns an array containing all the elements in the priority queue. |
| `comparator()`          | Returns the comparator used to order the elements in the queue, or `null` if the elements are ordered by their natural ordering. |
| `forEach(Consumer<? super E> action)` | Performs the given action for each element of the priority queue. |

# How comparator works 
Comparate works in simple logic 
we can see the inside the compare function will return int value that could be +ve , -ve or 0 
if it returns +ve then the swap will not happen 
if it return -ve the the swap will happen
if it return 0 then no changes 

1. Ascending Order: return num1 - num2;
   If num1 is smaller than num2, the result is negative (order them as num1 < num2).
2. Descending Order: return num2 - num1;
   If num1 is smaller than num2, the result is positive (order them as num2 < num1).

After java 8+ we can use lamba expression to directly use comparator 
like this -  
Comparator<Integer> ascendingComparator = (num1, num2) -> num1 - num2;  
                       or   
Comparator<Integer> descendingComparator = (num1, num2) -> num2 - num1;  

# what is difference between comparable and comparator ? 
