# AWS Lambda SnapStart – Java Cold Start Demonstration

## 1. Project Overview

This project demonstrates **AWS Lambda SnapStart using Java 21**.

The main purpose of this project is to understand the **Java cold start problem** in AWS Lambda and demonstrate how AWS Lambda SnapStart uses a pre-initialized snapshot to reduce repeated initialization work.

### Technologies Used

- AWS Lambda
- AWS Lambda SnapStart
- Java 21
- Maven
- Amazon CloudWatch
- AWS Management Console
- GitHub

---

## 2. Problem Statement

When AWS Lambda runs a Java function in a new execution environment, it needs to initialize the Java runtime, load classes, and initialize the application.

This additional startup time is called a **cold start**.

### Simple Daily-Life Example

A laptop that is completely shut down takes more time to start because the operating system and applications need to load.

A laptop in sleep mode can become ready more quickly.

Similarly:

```text
Cold Start
    ↓
Create Execution Environment
    ↓
Initialize Java
    ↓
Load Application
    ↓
Execute Function
```

SnapStart helps by restoring a previously initialized environment.

---

## 3. What is AWS Lambda SnapStart?

**AWS Lambda SnapStart** is a feature designed to improve startup performance for supported Lambda functions.

For a Java Lambda function, the basic concept is:

```text
Initialize Application
        ↓
Create Snapshot
        ↓
Publish Version
        ↓
Restore Snapshot
        ↓
Execute Function
```

Instead of repeating the complete initialization process, Lambda can restore the previously initialized state from the snapshot.

---

## 4. Project Objectives

The main objectives of this project are:

- Understand the Java cold start problem.
- Create a Java Lambda function.
- Configure Java 21 runtime.
- Observe Lambda initialization.
- Enable AWS Lambda SnapStart.
- Publish a Lambda version.
- Invoke the published version.
- Observe the restore process.
- Analyze CloudWatch logs.
- Understand initialization and restore behavior.

---

## 5. Technologies Used

| Technology | Purpose |
|---|---|
| **AWS Lambda** | Serverless function execution |
| **AWS Lambda SnapStart** | Snapshot-based startup optimization |
| **Java 21** | Lambda runtime |
| **Maven** | Java project management |
| **Amazon CloudWatch** | Monitoring and log analysis |
| **AWS Management Console** | Configuration and testing |
| **GitHub** | Source code and documentation |

---

## 6. System Architecture and Workflow

### Architecture

```text
                 AWS Lambda
                      |
                      ↓
                Java 21 Runtime
                      |
                      ↓
                Lambda Function
                      |
              ┌───────┴────────┐
              ↓                ↓
       Initialization       SnapStart
              ↓                ↓
      Initialized State     Snapshot
              ↓                ↓
              └───────┬────────┘
                      ↓
               Published Version
                      ↓
                   Invoke
                      ↓
                Restore Snapshot
                      ↓
               Execute Function
                      ↓
               CloudWatch Logs
```

### Complete Workflow

```text
Create Lambda Function
          ↓
Configure Java 21
          ↓
Create / Upload Java Code
          ↓
Run Lambda Function
          ↓
Observe Initialization
          ↓
Enable SnapStart
          ↓
Publish Lambda Version
          ↓
Invoke Published Version
          ↓
Observe Restore
          ↓
Check CloudWatch Logs
```

---

## 7. AWS Lambda Configuration

The Lambda function was configured with the following settings:

| Configuration | Value |
|---|---|
| **Runtime** | Java 21 |
| **Memory** | 512 MB |
| **Timeout** | 10 seconds |
| **SnapStart** | Enabled |

SnapStart is demonstrated using a **published Lambda version**.

---

## 8. Java Lambda Function and Testing

A simple Java Lambda function was created to demonstrate the Lambda execution lifecycle.

The function can be tested using a simple JSON event:

```json
{}
```

### Testing Steps

1. Open the Lambda function.
2. Select the required version.
3. Open the **Test** section.
4. Create a test event.
5. Use `{}` as the JSON event.
6. Run the function.
7. Check the execution result.
8. Open CloudWatch logs.
9. Observe initialization or restore information.

The project focuses on demonstrating the Lambda initialization and SnapStart restore behavior rather than implementing a complex business application.

---

## 9. Cold Start and Initialization

During a cold start, Lambda needs to prepare a new execution environment.

The process can be represented as:

```text
New Execution Environment
          ↓
Start Java Runtime
          ↓
Load Classes
          ↓
Initialize Application
          ↓
Initialization Complete
          ↓
Execute Function
```

During testing, initialization information was observed in the Lambda and CloudWatch logs.

Example observation:

```text
Initialization completed in approximately 3000 ms
```

> **Note:** The actual initialization duration can vary depending on the Lambda configuration and execution environment.

---

## 10. SnapStart Snapshot and Restore

After the Java environment is initialized, SnapStart can create a snapshot of the initialized state.

The workflow is:

```text
Java Initialization
        ↓
Initialized Environment
        ↓
Create Snapshot
        ↓
Published Version
        ↓
Function Invocation
        ↓
Restore Snapshot
        ↓
Execute Function
```

CloudWatch logs can show restore-related information such as:

```text
RESTORE_START
```

and:

```text
RESTORE_REPORT
```

These log entries provide evidence of the restore stage of the SnapStart lifecycle.

---

## 11. CloudWatch Monitoring and Observations

Amazon CloudWatch was used to monitor the Lambda execution.

### Initialization

```text
Initialization completed
```

This indicates that Lambda initialized the execution environment.

### Restore Start

```text
RESTORE_START
```

This indicates the beginning of the snapshot restoration process.

### Restore Report

```text
RESTORE_REPORT
```

This provides information about the restore operation.

### Overall Observation

```text
Initialization
      ↓
Create and prepare execution environment
```

```text
Restore
      ↓
Recover previously initialized state
```

CloudWatch logs provide evidence of the Lambda initialization and restore lifecycle.

---

## 12. Without SnapStart vs With SnapStart

### Without SnapStart

```text
Request
   ↓
Create New Environment
   ↓
Initialize Java
   ↓
Initialize Application
   ↓
Execute Function
```

### With SnapStart

```text
Request
   ↓
Restore Snapshot
   ↓
Execute Function
```

### Comparison

| Without SnapStart | With SnapStart |
|---|---|
| New environment needs initialization | Previously initialized state can be restored |
| Java initialization occurs | Snapshot restoration occurs |
| Startup overhead can include initialization | Designed to reduce repeated initialization work |
| No SnapStart snapshot | Uses SnapStart snapshot |

> **Note:** The actual performance improvement should be measured using real test results rather than assumed from the feature alone.

---

## 13. Screenshots and Project Structure

### Screenshots Included

The `screenshots` folder contains evidence of the project implementation:

1. Lambda function configuration
2. Java 21 runtime
3. SnapStart configuration
4. Published Lambda version
5. Test event
6. Test execution
7. CloudWatch logs
8. Initialization information
9. Restore information

### Project Structure

```text
AWS-Lambda-SnapStart/
│
├── README.md
├── pom.xml
│
├── src/
│   └── main/
│       └── java/
│           └── LambdaFunction.java
│
└── screenshots/
    ├── 01-lambda-function.png
    ├── 02-java-runtime.png
    ├── 03-lambda-configuration.png
    ├── 04-snapstart-enabled.png
    ├── 05-published-version.png
    ├── 06-test-event.png
    ├── 07-test-execution.png
    ├── 08-cloudwatch-logs.png
    ├── 09-initialization.png
    └── 10-restore.png
```

> **Important:** If your actual screenshot filenames are different, replace the names above with your actual GitHub filenames.

---

## 14. Conclusion and Key Learning

This project demonstrates the **Java cold start problem in AWS Lambda** and the working concept of **AWS Lambda SnapStart**.

The complete concept can be summarized as:

```text
Java Lambda
     ↓
Cold Start
     ↓
Initialization
     ↓
SnapStart
     ↓
Snapshot
     ↓
Published Version
     ↓
Restore
     ↓
Function Execution
     ↓
CloudWatch Logs
```

### Key Learning

The main learning from this project is that **AWS Lambda SnapStart can reduce repeated Java initialization work by restoring a previously initialized execution environment**.

The project also demonstrates how:

- Java 21 works with AWS Lambda.
- Lambda initialization occurs.
- SnapStart creates a snapshot.
- A published version is used for SnapStart.
- Lambda restores the snapshot.
- CloudWatch logs show initialization and restore information.

### Daily-Life Analogy

```text
Cold Start
    =
Starting a laptop from complete shutdown
```

```text
SnapStart Restore
    =
Waking a laptop from sleep
```

This is only an analogy. AWS Lambda SnapStart restores a previously initialized execution environment rather than literally putting a computer into sleep mode.

---

## Author

**Project:** AWS Lambda SnapStart – Java Cold Start Demonstration

**Runtime:** Java 21

**Platform:** AWS Lambda

**Repository:** `AWS-Lambda-SnapStart`
