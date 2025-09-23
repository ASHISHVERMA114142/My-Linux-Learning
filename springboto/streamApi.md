# Java Stream API: `stream(arr).boxed()` and `stream(arr).sequential()`

## 1. `stream(arr).boxed()`

- **Purpose**: Converts a primitive stream (like `IntStream`, `LongStream`, or `DoubleStream`) into a stream of their corresponding wrapper classes (`Stream<Integer>`, `Stream<Long>`, or `Stream<Double>`).

- **Usage**: Useful when you need to work with a stream of primitive types but want to use methods that require objects.

- **Example**:
    ```java
    int[] arr = {1, 2, 3, 4, 5};
    Stream<Integer> boxedStream = Arrays.stream(arr).boxed();
    boxedStream.forEach(System.out::println); // Outputs: 1 2 3 4 5
    ```

## 2. `stream(arr).sequential()`

- **Purpose**: Ensures that the stream is processed in a sequential manner. This is important when you want to maintain the order of operations.

- **Usage**: Useful for operations that rely on the order of elements.

- **Example**:
    ```java
    List<String> list = Arrays.asList("a", "b", "c", "d");
    Stream<String> sequentialStream = list.stream().sequential();
    sequentialStream.forEach(System.out::println); // Outputs: a b c d
    ```

## Summary

- **`stream(arr).boxed()`**: Converts a primitive stream to a stream of wrapper objects, allowing the use of object-based operations.
  
- **`stream(arr).sequential()`**: Ensures that the stream is processed in a sequential manner, maintaining order in operations.


#----------------------------------------------------------------------------------------------------------------------------------------------
# Converting `int[]` to `List<Integer>`

In Java, you cannot directly convert an `int[]` (an array of primitive integers) to a `List<Integer>` using `Arrays.asList(intArray)`. This is because `Arrays.asList()` treats the entire primitive array as a single object, resulting in a list containing one element, which is the array itself.

## Example of Incorrect Usage

If you try to do this:

```java
int[] intArray = {1, 2, 3, 4, 5};
List<Integer> newList = Arrays.asList(intArray); // This will not work as expected
```
# some question for the practice 
```java
        // TODO : Hard question for the stream
        // TODO : 8. Check if all elements are positive.
        List<Integer> input8=Arrays.asList(0,-1,-2,-3);
        Boolean output8=input8.stream().allMatch(x-> x>0);
        System.out.println(output8);
        // output: true

        // TODO : 9. Find top 3 highest numbers.
        List<Integer> input9= Arrays.asList(1,2,3,4,5,6);
        List<Integer> output9=input9.stream().distinct().sorted(Collections.reverseOrder()).limit(3).toList();
        System.out.println(output9);
        // output: [6,5,4]

        // TODO: 10. Sort a map by values using streams.
        Map<String, Integer> input10 = Map.of("A", 3, "B", 1, "C", 2);
        LinkedHashMap<String,Integer> sortedMap= input10.entrySet().stream()
                .sorted(Map.Entry.comparingByValue())
                .collect(Collectors.toMap(
                        Map.Entry::getKey,
                        Map.Entry::getValue
                        ,(e1,e2)-> e1,
                        LinkedHashMap::new
                ));
        System.out.println(sortedMap);
        // output: {B=1, C=2, A=3}

        // TODO: 11 Find duplicate elements in a list.
     
        System.out.println("**HARD**");

        // TODO : Medium question for the stream
        // TODO: 1. Group strings by their length.
        List<String> input1= Arrays.asList("a", "bb", "ccc", "dd", "e");
        Map<Integer,List<String>> output1 = input1.stream().collect(Collectors.groupingBy(String::length));
        System.out.println(output1);
        //output: {1=[a, e], 2=[bb, dd], 3=[ccc]}

        // TODO: 2. Join a list of strings with commas.
        List<String> input2= Arrays.asList("a", "bb", "ccc", "dd", "e");
        String output2= input2.stream().collect(Collectors.joining(", "));
        System.out.println(output2);
        //output: a, bb, ccc, dd, e

        // TODO: 3. Find sum of squares of all numbers.
        List<Integer> input3=Arrays.asList(1,2,3);
        int output3=input3.stream().mapToInt(x-> x*x).sum();
        System.out.println(output3);
        //output: 14

        // TODO: 4. Partition list into even and odd.
        List<Integer> input4= Arrays.asList(1,2,3,4,5,6);
        Map<Boolean,List<Integer>> output4=input4.stream().collect(Collectors.groupingBy(x-> x%2==0));
        System.out.println(output4);
        //output: {false=[1, 3, 5], true=[2, 4, 6]}

        // TODO: 5. Find frequency of characters in a string.
        String input5="banana";
        Map<Character,Long> output5=input5.chars().mapToObj(c-> (char)c).collect(Collectors.groupingBy(x-> x, Collectors.counting()));
        System.out.println(output5);
        // output: {a=3, b=1, n=2}

        // TODO: 6. Find the second highest number.
        List<Integer> input6=Arrays.asList(1,2,3,4,4,4,5);
        Optional<Integer> output6=input6.stream().distinct().sorted(Collections.reverseOrder()).skip(1).findFirst();
        System.out.println(output6.orElseThrow());
        // output: 4

        // TODO: 7 Flat map list of lists into single list.
        List<List<Integer>> input7=new ArrayList<>();
        input7.add(Arrays.asList(1,2,3));
        input7.add(Arrays.asList(4,5,6));
        input7.add(Arrays.asList(7,9));
        List<Integer> output7=input7.stream().flatMap(List::stream).toList();
        System.out.println(output7);
        // output: [1, 2, 3, 4, 5, 6, 7, 9]


        System.out.println("*EASY*");
        // TODO : Easy question for the stream
        // TODO: Find max number from list.
        List<Integer> nums5= Arrays.asList(1,1,1,1,2,3,5,2,3,4,5,6);
        Optional<Integer> ans5=nums5.stream().max(Integer::compare);
        System.out.println(ans5.orElseThrow());
        // output: 6

        // TODO: Sort a list of strings.
        List<String> names3 = Arrays.asList("Bob", "Alice", "Andrew", "Charlie");
        List<String> ans3= names3.stream().sorted().toList();
        System.out.println(ans3);
        //output : [Alice, Andrew, Bob, Charlie]

        // TODO: Remove duplicates from a list.
        List<Integer> nums4= Arrays.asList(1,1,1,1,2,3,5,2,3,4,5,6);
        List<Integer> ans4=nums4.stream().distinct().toList();
        System.out.println(ans4);
        // output : [1, 2, 3, 5, 4, 6]

        // TODO: Find the first element starting with 'A'.
        List<String> names1 = Arrays.asList("Bob", "Alice", "Andrew", "Charlie");
        Optional<String> ans2=names1.stream().filter(x-> x.startsWith("A")).findFirst();
        System.out.println(ans2.orElseThrow());
        // OUTPUT: Alice

        // TODO: onvert a list of strings to uppercase.
        List<String> names = Arrays.asList("alice", "bob", "charlie");
        List<String> ans=names.stream().map(String::toUpperCase).toList();
        System.out.println(ans);
        // Output: [ALICE, BOB, CHARLIE]

        // TODO: Filter even numbers from a list.
        List<Integer> nums = Arrays.asList(1,2,3,4,5,6);
        List<Integer> ans1=nums.stream().filter(x-> x%2==0).toList();
        System.out.println(ans1);
        //output: [2, 4, 6]

        // TODO: Count the number of elements greater than 10
        List<Integer> nums1 = Arrays.asList(5,12,18,7,3);
        long count=nums1.stream().filter(x-> x>10).count();
        System.out.println(count);
        // OUTPUT: 2
```
