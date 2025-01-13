### Strongly Typed vs. Weakly Typed Languages
In programming, strongly typed and weakly typed refer to how strictly a programming language enforces type rules

1. Strongly Typed Languages  
A strongly typed language enforces strict type rules. In such a language, every variable has a specific type, and it is not automatically converted (coerced) to a different type unless explicitly told to do so. This means that mismatched types will typically result in a compile-time error or a runtime exception.

Characteristics of Strongly Typed Languages:  
Type errors are caught early, either at compile time or runtime.  
Type safety is ensured, preventing operations between incompatible types unless explicitly handled.  
Type conversion must be done explicitly by the programmer.  

2. Weakly Typed Languages  
A weakly typed language (sometimes referred to as a loosely typed language) is more flexible with types. In such languages, the language performs implicit type conversions or type coercion to make sure operations can proceed even if the types do not match perfectly. This often happens automatically, which can lead to unexpected behavior or errors if not properly managed.

Characteristics of Weakly Typed Languages:  
Implicit type conversion: The language automatically converts between types when necessary.  
More flexibility: You can perform operations with different data types, and the language tries to make it work, sometimes leading to unexpected behavior.  
Potential for runtime errors: Since the conversion is automatic, it might lead to unintended results or difficult-to-diagnose errors.  

## Strongly Typed vs Weakly Typed: Key Differences

| Feature                        | Strongly Typed Languages                     | Weakly Typed Languages                   |
|--------------------------------|----------------------------------------------|------------------------------------------|
| **Type Enforcement**           | Strict type checks at compile-time or runtime | Implicit type conversion/coercion        |
| **Type Errors**                | Type errors are caught early (compile-time or runtime) | Type errors are often avoided through implicit conversion |
| **Type Safety**                | High type safety, less prone to type-related bugs | Lower type safety, more prone to unexpected behaviors |
| **Type Conversion**            | Requires explicit conversion (casting)       | Implicit type conversion (often automatic) |
| **Examples**                   | Java, C#, Python (strongly typed but dynamic) | JavaScript, PHP, Perl                   |



## Static Typed vs Dynamic Typed: Key Differences

| Feature                        | Static Typed Languages                          | Dynamic Typed Languages                    |
|---------------------------------|-------------------------------------------------|--------------------------------------------|
| **Type Checking**               | Types are checked at **compile-time**.          | Types are checked at **runtime**.          |
| **Type Declaration**            | Variables must have their type explicitly declared. | Variables do not need type declarations; types are inferred at runtime. |
| **Type Errors**                 | Type errors are caught early, during compilation. | Type errors are caught during execution (runtime errors). |
| **Performance**                 | Typically faster, since types are resolved at compile-time. | May be slower due to runtime type checking. |
| **Flexibility**                 | Less flexible, as type constraints must be respected. | More flexible, since types can change at runtime. |
| **Code Readability**            | More explicit, types are visible and enforced, improving readability. | Less explicit, as types are inferred, potentially making the code harder to understand. |
| **Development Speed**           | Slower development due to the need for explicit type declaration and compilation. | Faster development since there’s no need for type declaration and compilation. |
| **Error Detection**             | Errors related to types are detected before running the program (compile-time). | Errors are detected when the program is running, potentially causing runtime failures. |
| **Type Safety**                 | High type safety. Misusing types results in compilation errors. | Lower type safety. Type-related issues may only surface at runtime. |
| **Examples**                    | Java, C, C++, Rust, Swift, Kotlin              | Python, JavaScript, Ruby, PHP, Perl         |
| **Type Coercion**               | Little to no type coercion; the types are fixed. | Type coercion may occur, which can lead to unexpected behavior. |
| **Code Refactoring**            | Easier to refactor, as type constraints are known at compile-time. | Refactoring may be more challenging, as types are not explicitly known. |
| **Memory Management**           | More predictable memory management due to fixed types. | Memory management is more dynamic, which can lead to inefficiencies. |
