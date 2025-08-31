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
```
```xml
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-validate-plugin:...
Missing required element 'groupId' in pom.xml
```
## ❗ Example 2: Malformed XML in pom.xml
```xml
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version
</project>
```
```xml
[ERROR] Error parsing the pom.xml: Element type "version" must be followed by either attribute specifications, ">" or "/>".
```
❗ Example 3: Invalid Project Structure

Source code placed outside src/main/java

Missing src folder

Possible Error:
```xml
[WARNING] No sources found at expected location: src/main/java
```


### 2. ** Complie phase **

Purpose:
Compiles the source code (typically from src/main/java) to .class files.
Basically it compiles all java code the is present on current dir using selected javac. 
What it does:
Runs javac on source files
Places output in target/classes/

### 3. ** Test Phase **

Purpose:
Runs unit tests in src/test/java using frameworks like JUnit/TestNG.
What it does:
Compiles test code
Runs tests
Fails the build if tests fail

### 4. ** Test Phase **

Purpose:
Runs unit tests in src/test/java using frameworks like JUnit/TestNG.
What it does:
Compiles test code
Runs tests
Fails the build if tests fail

### 5. ** Package phase **

Purpose:
Packages compiled code into an artifact (.jar, .war, .pom,ear , maven-plugin)
What it does:
Creates JAR/WAR using configurations in pom.xml
Includes compiled classes and resources
Fails when the <packaging>xyz</packaging> type is not match with the requrired types . 

### 6. **Verify Phase **


Purpose:
Runs additional checks (like integration tests or custom verifications).
What it does:
Runs extra plugins like maven-enforcer-plugin or integration tests
Example failure:
Using Enforcer plugin:

# Maven Verify Phase

## ✅ Purpose

The **Verify** phase is part of Maven's **default build lifecycle** and is executed **after testing** (unit and integration tests).  

> **Main Goal:**  
To run **additional checks** to verify the integrity, correctness, and quality of the project before it is packaged or installed.

---

## 📋 What It Checks

The `verify` phase does not compile or package the project — instead, it performs validations such as:

### 1. ✅ Integration Test Results
- Evaluates the outcome of integration tests (run by the `failsafe` plugin).
- Build fails if any integration tests fail.

> Plugin: `maven-failsafe-plugin`

---

### 2. ✅ Static Code Analysis and Quality Checks
- Checks for code style, complexity, naming rules, etc.
- Helps enforce coding standards.

> Common Plugins:
- `maven-checkstyle-plugin`
- `maven-pmd-plugin`
- `spotbugs-maven-plugin`

---

### 3. ✅ Security and Dependency Checks
- Scans project dependencies for known security vulnerabilities.
- Fails the build if high-severity issues are detected.

> Plugin: `dependency-check-maven` (OWASP)

---

### 4. ✅ Custom Validation Rules
- You can create or integrate plugins for domain-specific checks.
- These can perform custom business logic validations.

---

## ❌ When It Fails

The `verify` phase will fail if any of the following conditions are true:

| ❌ Cause                            | ⚠️ Example/Error Message |
|-----------------------------------|---------------------------|
| Integration test failure          | `Tests run: 3, Failures: 1, Errors: 0` |
| Checkstyle rule violation         | `Checkstyle violation in src/main/java/com/example/MyClass.java` |
| PMD rule violation                | `Avoid unused private fields` |
| Security vulnerability found      | `High severity vulnerability in commons-collections` |
| Custom validation plugin failure  | Plugin throws exception or fails a rule |

---

### 📉 Example: Integration Test Failure

```text
[INFO] --- maven-failsafe-plugin:3.0.0-M5:verify (default) ---
[ERROR] There are test failures.

Please refer to target/failsafe-reports for the individual test results.
```

# Maven Install Phase

## ✅ Purpose

The **Install** phase installs the project's packaged artifact (e.g., JAR, WAR) into the **local Maven repository** (`~/.m2/repository`).

---

## 📋 What It Does

- Installs the output of the `package` phase.
- Copies:
  - `.jar` / `.war` file
  - `pom.xml`
  - Checksums
- Makes the artifact available for **other local Maven projects**.

### 📌 Example

After `mvn install`, another local project can reference it:

```xml
<dependency>
  <groupId>com.example</groupId>
  <artifactId>my-library</artifactId>
  <version>1.0.0</version>
</dependency>
```

7. Deploy Phase

Purpose:
Uploads the artifact to a remote repository (like Nexus, Artifactory, etc.)

What it does:

Pushes JAR/WAR and pom.xml to a repository manager

Example failure:
Incorrect credentials or repo URL:
```xml
<distributionManagement>
  <repository>
    <id>my-repo</id>
    <url>http://repo.example.com/maven2</url>
  </repository>
</distributionManagement>
```

| Phase    | What it does                     | Example Failure                          |
|----------|----------------------------------|------------------------------------------|
| Validate | Checks `pom.xml` & project setup | Missing `<artifactId>`                   |
| Compile  | Compiles Java source             | Compilation error                        |
| Test     | Runs unit tests                  | Failing JUnit test                       |
| Package  | Creates JAR/WAR                  | Unknown packaging type                   |
| Verify   | Runs additional checks           | Java version mismatch                    |
| Install  | Adds artifact to local repo      | Dependency conflict or local write error |
| Deploy   | Publishes to remote repo         | Bad credentials or network failure       |

