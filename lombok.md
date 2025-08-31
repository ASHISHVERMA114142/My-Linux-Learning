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
- `access`: Sets constructor visibility (`PUBLIC`, `PROTECTED`, `PACKAGE`, `PRIVATE`) :contentReference[oaicite:1]{index=1}.
- `force`: `true` enables generation even with `final` fields, assigning defaults :contentReference[oaicite:2]{index=2}.
- `staticName`: When provided, constructor becomes private and a static factory method is generated :contentReference[oaicite:3]{index=3}.

**When to use**: Useful for frameworks like Hibernate that require a default constructor :contentReference[oaicite:4]{index=4}.

---

## 2. `@AllArgsConstructor`

Generates a constructor that takes **one parameter for every field** in the class (excluding static ones). Defaults are applied only if fields were initialized at declaration :contentReference[oaicite:5]{index=5}.

**Use case**: Ideal when you want to initialize all fields in one go :contentReference[oaicite:6]{index=6}.

---

## 3. `@RequiredArgsConstructor`

Generates a constructor that includes parameters for:
- All uninitialized `final` fields.
- All fields annotated with `@NonNull` (and not initialized where declared) :contentReference[oaicite:7]{index=7}.

Null checks are inserted for `@NonNull` fields in the generated constructor :contentReference[oaicite:8]{index=8}.

**When to use**: Great for mandatory dependencies—commonly used in service classes to ensure required fields are initialized at creation :contentReference[oaicite:9]{index=9}.

---

## 4. Summary Table

| Annotation               | Constructor Parameters               | Null Checks for `@NonNull` | Use Case                          |
|--------------------------|--------------------------------------|-----------------------------|-----------------------------------|
| `@NoArgsConstructor`      | None                                 | No                          | Required by frameworks (e.g., Hibernate) |
| `@AllArgsConstructor`     | All fields                           | Yes (for `@NonNull`)        | Initialize all fields at once     |
| `@RequiredArgsConstructor`| Uninitialized `final` + `@NonNull`   |
