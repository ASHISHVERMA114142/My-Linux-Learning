# Lombok `@Getter` and `@Setter` Note

When annotating a field:

- `@Getter` generates a default getter:
  - Named `getFieldName()` or `isFieldName()` if the field is `boolean`.
  - Returns the current field value.  
- `@Setter` generates a default setter:
  - Named `setFieldName(...)`.
  - Takes one parameter matching the field type and assigns it.

Both methods are generated as `public` by default, unless overridden with `AccessLevel`.

Allowed `AccessLevel` values:
- `PUBLIC`
- `PROTECTED`
- `PACKAGE` (package-private)
- `PRIVATE`
- `NONE` — disables generation entirely for that field.  
:contentReference[oaicite:0]{index=0}

You can also annotate the entire class, applying getter/setter generation to **all non-static fields**. Field-level annotations override class-level defaults.  
:contentReference[oaicite:1]{index=1}

### Example

```java
@Getter
@Setter
public class Author {
    private int id;
    private String name;

    @Setter(AccessLevel.PROTECTED)
    private String surname;
}
```

# Lombok Constructor Annotations

Lombok simplifies constructor creation with three main annotations: `@NoArgsConstructor`, `@RequiredArgsConstructor`, and `@AllArgsConstructor`.

---

## 1. `@NoArgsConstructor`

Generates a **no-argument constructor**. If there are uninitialized `final` fields, compilation will fail—unless you use `force = true`, which initializes them to default values (`null` / `0` / `false`) :contentReference[oaicite:0]{index=0}.

Options:
- `access`: Sets constructor visibility (`PUBLIC`, `PROTECTED`, `PACKAGE`, `PRIVATE`)
# Lombok `@NoArgsConstructor` Option Examples

## 1. `access`
Set the visibility of the generated constructor using `AccessLevel` (`PUBLIC`, `PROTECTED`, `PACKAGE`, `PRIVATE`).

```java
import lombok.NoArgsConstructor;
import lombok.AccessLevel;

@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Singer {
    private Long id;
    private String firstName;
    private String lastName;
}
```

- `force`: `true` enables generation even with `final` fields, assigning defaults :( if you have not useded @RequiredArgsConstructor ) then it will assign default values to the ( false , o , null ) those final field .   
- `staticName`: When provided, constructor becomes private and a static factory method is generated : it will generate the static factory method that will return the contructor of that class .   

**When to use**: Useful for frameworks like Hibernate that require a default constructor   

---

## 2. `@AllArgsConstructor`

Generates a constructor that takes **one parameter for every field** in the class (excluding static ones). Defaults are applied only if fields were initialized at declaration  

**Use case**: Ideal when you want to initialize all fields in one go  

---

## 3. `@RequiredArgsConstructor`

Generates a constructor that includes parameters for:
- All uninitialized `final` fields.
- All fields annotated with `@NonNull` (and not initialized where declared)   

Null checks are inserted for `@NonNull` fields in the generated constructor   

**When to use**: Great for mandatory dependencies—commonly used in service classes to ensure required fields are initialized at creation   

---

## 4. Summary Table

| Annotation               | Constructor Parameters               | Null Checks for `@NonNull` | Use Case                          |
|--------------------------|--------------------------------------|-----------------------------|-----------------------------------|
| `@NoArgsConstructor`      | None                                 | No                          | Required by frameworks (e.g., Hibernate) |
| `@AllArgsConstructor`     | All fields                           | Yes (for `@NonNull`)        | Initialize all fields at once     |
| `@RequiredArgsConstructor`| Uninitialized `final` + `@NonNull`   |


# Lombok `@ToString` Annotation — Quick Notes

## Overview
- Annotating a **class** with `@ToString` generates a sensible `toString()`:
  - Format: `ClassName(field1=value1, field2=value2, ...)` 
  - By default, **all non-static fields** are included 
---

## Key Configuration Options

- `includeFieldNames` (default: `true`)
  - When `true`, prints `fieldName=value`; otherwise, only values appear 
- **Exclude fields** from output:
  - **Field-level**: Annotate with `@ToString.Exclude` 
  - **Class-level only**: Use `@ToString(onlyExplicitlyIncluded = true)` + annotate each desired field with `@ToString.Include`
- `callSuper` (default: `false`)
  - Include `super.toString()` in output (when extending another class)

- Advanced: Methods can be included too
  - Annotate **no-arg instance methods** with `@ToString.Include` to have their return value show up 
---

##  Practical Examples

### Basic Usage
```java
@ToString
public class User {
  private int id;
  private String name;
  @ToString.Exclude
  private String password;
}
```

# Lombok `@EqualsAndHashCode` — Quick Notes

## Overview
- Annotate a **class** with `@EqualsAndHashCode` to automatically generate `equals()` and `hashCode()` methods.
- **Defaults**: Includes all **non-static**, **non-transient** fields by default. :contentReference[oaicite:0]{index=0}
- Lombok ensures the generated methods adhere to the Java contract between `equals()` and `hashCode()`—you maintain consistency without extra effort. :contentReference[oaicite:1]{index=1}

---

##  Field-Level Customization

- Exclude fields:
```java
  @EqualsAndHashCode.Exclude
  private String sensitiveData;
```

Include fields explicitly:
```java
@EqualsAndHashCode(onlyExplicitlyIncluded = true)
public class User {
    @EqualsAndHashCode.Include
    private String username;
    private int age;
}
```
Only fields annotated with @Include are used.  

Include method-return values:  
```java
@EqualsAndHashCode
public class User {
    private int value;
    @EqualsAndHashCode.Include
    public int doubled() {
        return value * 2;
    }
}
```

The doubled() return value is included in equality/hash computation.  

Dealing with Inheritance

Use callSuper = true when your class extends another and you want to incorporate superclass fields. Failing to do so may lead to incorrect equality when the superclass contains important state.  


Optional Configuration Parameters

From Lombok's API:

onlyExplicitlyIncluded: Only include fields/methods annotated with @Include. Default: false. 

callSuper: Whether to include superclass equals()/hashCode() logic. Default: false.   

doNotUseGetters: Use direct field access instead of getters. Default: false. 

Deprecated: of and exclude annotation parameters; prefer using Include/Exclude instead.  


@NonNull
You can annotate with @NonNull a record component, a parameter of a method, or an entire constructor. This way, Lombok will generate null-check statements for you accordingly.  

@Data
@Data is a shortcut annotation that combines the features of @ToString, @EqualsAndHashCode, @Getter @Setter, and @RequiredArgsConstructor together. So, @Data generates all the boilerplate involved in POJOs  

@Value
@Value is the immutable variant of @Data. This means that all fields are made private and final by Lombok by default. Plus, setters will not be generated, and the class itself will be marked as final. This way, the class will not be inheritable. Just like what happens with @Data, toString(), equals() and hashCode() implementations are also created.  


# Lombok `@Synchronized` — Quick Reference

## Overview
- The `@Synchronized` annotation provides thread-safe method locking—similar to Java's `synchronized` keyword—but avoids synchronizing on `this`.
- **Non-static methods** lock on a private field named `$lock`; **static methods** use a private static field named `$LOCK`. Lombok auto-generates these fields if absent. :contentReference[oaicite:0]{index=0}
- You can also specify your own lock field by name; Lombok will then **not** generate `$lock` / `$LOCK` and will require the field to exist. :contentReference[oaicite:1]{index=1}
- Only usable on **instance** and **static methods**, just like the `synchronized` keyword. :contentReference[oaicite:2]{index=2}

---

##  Example & Equivalent Plain Java

### Lombok-enhanced code
```java
import lombok.Synchronized;

public class SynchronizedDemo {
    private final Object customLock = new Object();

    @Synchronized
    public static void sayHello() {
        System.out.println("Hello!");
    }

    @Synchronized
    public int getOne() {
        return 1;
    }

    @Synchronized("customLock")
    public void printCustom() {
        System.out.println("Using custom lock");
    }
}
```

generated class 
```java
public class SynchronizedDemo {
    private static final Object $LOCK = new Object[0];
    private final Object $lock = new Object[0];
    private final Object customLock = new Object();

    public static void sayHello() {
        synchronized ($LOCK) {
            System.out.println("Hello!");
        }
    }

    public int getOne() {
        synchronized ($lock) {
            return 1;
        }
    }

    public void printCustom() {
        synchronized (customLock) {
            System.out.println("Using custom lock");
        }
    }
}
```
@Log
The majority of loggers require you to set up a logger instance in every class where you want to log. This definitely involves boilerplate code. By annotating a class with @Log, Lombok will automatically add a static final log field,  
