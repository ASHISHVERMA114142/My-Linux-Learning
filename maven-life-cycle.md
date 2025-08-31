# Maven Validate Phase

## ✅ Purpose

The **Validate** phase is the first phase in the Maven build lifecycle. Its main purpose is to ensure that the project is correctly configured and ready for the build process to proceed.

---

## 🔍 What It Checks

### 1. **`pom.xml` Validity**
- Is the XML well-formed?
- Are required elements like `<groupId>`, `<artifactId>`, and `<version>` present (or inherited)?
- Are required build plugins properly declared?
- Can all declared dependencies be resolved?

### 2. **Project Structure**
- Does the directory layout follow Maven conventions?
  - For example:
    - Source code in `src/main/java`
    - Test code in `src/test/java`
- Are essential files and folders present?

### 3. **Maven Settings/Environment**
- Can Maven access the configured repositories?
- Is the project aligned with the expected Maven version?

---

## ❌ What It Does **Not** Do

- ❌ Does **not compile** source code.
- ❌ Does **not run tests**.
- ❌ Does **not package or install** the project.
- ❌ Does **not validate** custom project logic beyond plugin and configuration checks.

---

## 📉 Example Failures

### ❗ Example 1: Missing `<groupId>` in `pom.xml`

```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <!-- Missing <groupId> -->
    <artifactId>my-app</artifactId>
    <version>1.0-SNAPSHOT</version>
</project>
