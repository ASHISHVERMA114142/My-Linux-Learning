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
