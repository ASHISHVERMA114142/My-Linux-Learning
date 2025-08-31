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


### 2. **Compile Phase**

**Purpose:**  
Compiles the source code (typically from `src/main/java`) into `.class` files.  
Basically, it compiles all Java code in the project using the configured `javac`.

**What it does:**
- Runs `javac` on source files
- Places output in `target/classes/`


---

### 3. **Test Phase**

**Purpose:**  
Runs **unit tests** located in `src/test/java` using frameworks like **JUnit** or **TestNG**.

**What it does:**
- Compiles test source code
- Executes unit tests
- Fails the build if any tests fail

---

### 4. **Package Phase**

**Purpose:**  
Packages compiled code into a distributable artifact (`.jar`, `.war`, `.ear`, `.pom`, `maven-plugin`, etc.).

**What it does:**
- Uses configuration in `pom.xml` to determine packaging type
- Packages compiled classes and resources into the defined format (e.g., JAR/WAR)
- Places the artifact in the `target/` directory

**Example failure:**
Fails when the `<pac


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



# Maven Clean Lifecycle

The **Clean Lifecycle** is designed for cleaning the project, ensuring that artifacts generated from previous builds are removed before starting a new build process.

It consists of **3 phases**:

---

## 1. `pre-clean` Phase

**Purpose:**  
Executes processes before cleaning.

**What it might do:**
- Back up logs or artifacts
- Notify systems (via scripts or plugins)
- Run custom plugin executions

**Example Use Case:**  
Archive previous logs before deletion by copying the log file to a backup folder using the `maven-antrun-plugin`:

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-antrun-plugin</artifactId>
  <executions>
    <execution>
      <phase>pre-clean</phase>
      <configuration>
        <tasks>
          <copy file="logs/build.log" tofile="backup/build.log" />
        </tasks>
      </configuration>
      <goals>
        <goal>run</goal>
      </goals>
    </execution>
  </executions>
</plugin>

BUILD FAILURE
Error copying file: logs/build.log (No such file or directory)
```
# 2. Clean Phase

## Purpose
Removes files generated by the previous build, typically the `target/` directory, which contains compiled classes, packaged artifacts, and temporary files.

## What it removes
- `target/`
- `classes/` (compiled code)
- Packaged artifacts (`*.jar`, `*.war`, etc.)
- Temporary files and reports

## Example Command
```bash
mvn clean
[INFO] Deleting /path/to/project/target
```

# 3. Post-clean Phase

## Purpose
Executes processes after cleaning.

## What it might do
- Send notifications (email, webhook)
- Log clean success
- Run preparation scripts

## Example Use Case
Log a message after cleaning completes successfully using the `maven-antrun-plugin`:

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-antrun-plugin</artifactId>
  <executions>
    <execution>
      <phase>post-clean</phase>
      <configuration>
        <tasks>
          <echo message="Clean completed successfully." />
        </tasks>
      </configuration>
      <goals>
        <goal>run</goal>
      </goals>
    </execution>
  </executions>
</plugin>
BUILD FAILURE
Element type "echo" must be declared.
```
# Dependencies scope
| Scope        | Available In                          | Included in Artifact? | Typical Use Case                                                   |
|--------------|----------------------------------------|------------------------|---------------------------------------------------------------------|
| **compile**  | compile, test, runtime (all phases)    | Yes                    | Core libraries needed wherever—compile time, testing, runtime       |
| **provided** | compile, test                          | No                     | APIs provided by the runtime environment (e.g., Servlet API)        |
| **runtime**  | test, runtime                          | Yes                    | Libraries needed at runtime but not at compile-time (e.g., JDBC)    |
| **test**     | test only                              | No                     | Testing frameworks (e.g., JUnit, Mockito)                           |
| **system**   | compile, test (local only)             | No                     | Local-only JARs not in repositories (discouraged)                   |
| **import**   | dependencyManagement (type="pom") only | No                     | Import BOMs or centralized dependency versions for multi‑module projects |
