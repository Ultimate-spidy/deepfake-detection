# Sequence Diagram for Deepfake Detection System

## Image Upload and Detection Sequence

```
+-------+      +------------+    +--------------+   +----------------+   +------------+   +--------------+
| User  |      | Web UI     |    | Flask App    |   | Preprocessing  |   | MesoNet    |   | Database    |
+-------+      +------------+    +--------------+   +----------------+   +------------+   +--------------+
    |                |                 |                    |                  |                 |
    | Upload Image   |                 |                    |                  |                 |
    |--------------->|                 |                    |                  |                 |
    |                | POST /upload    |                    |                  |                 |
    |                |---------------->|                    |                  |                 |
    |                |                 | Validate Image     |                    |               |
    |                |                 |--------------------|                    |               |
    |                |                 |                    |                    |               |
    |                |                 | is_valid_image()   |                    |               |
    |                |                 |------------------->|                    |               |
    |                |                 |                    | Check for Face     |               |
    |                |                 |                    |----                |               |
    |                |                 |                    |   |                |               |
    |                |                 |                    |<---                |               |
    |                |                 |                    |                    |               |
    |                |                 |<-------------------|                    |               |
    |                |                 |                    |                    |               |
    |                |                 | If no face: Return Error               |               |
    |                |                 |----                |                    |               |
    |                |                 |   |                |                    |               |
    |                |                 |<---                |                    |               |
    |                |                 |                    |                    |               |
    |                | Error Response  |                    |                    |               |
    |                |<----------------|                    |                    |               |
    | Show Error     |                 |                    |                    |               |
    |<---------------|                 |                    |                    |               |
    |                |                 |                    |                    |               |
    |                |                 | If face present:   |                    |               |
    |                |                 | preprocess_image() |                    |               |
    |                |                 |------------------->|                    |               |
    |                |                 |                    | Resize & Normalize |               |
    |                |                 |                    |----                |               |
    |                |                 |                    |   |                |               |
    |                |                 |                    |<---                |               |
    |                |                 |<-------------------|                    |               |
    |                |                 |                    |                    |               |
    |                |                 | predict()          |                    |               |
    |                |                 |---------------------------------------->|               |
    |                |                 |                    |                    | Run Model     |
    |                |                 |                    |                    |----           |
    |                |                 |                    |                    |   |           |
    |                |                 |                    |                    |<---           |
    |                |                 |<----------------------------------------|               |
    |                |                 |                    |                    |               |
    |                |                 | log_result()       |                    |               |
    |                |                 |-------------------------------------------------------->|
    |                |                 |                    |                    |               | Store Result
    |                |                 |                    |                    |               |----
    |                |                 |                    |                    |               |   |
    |                |                 |                    |                    |               |<---
    |                |                 |<--------------------------------------------------------|
    |                |                 |                    |                    |               |
    |                | Render result.html                   |                    |               |
    |                |<----------------|                    |                    |               |
    | View Result    |                 |                    |                    |               |
    |<---------------|                 |                    |                    |               |
    |                |                 |                    |                    |               |
```

## Homepage Access Sequence

```
+-------+      +------------+    +--------------+   +--------------+
| User  |      | Web UI     |    | Flask App    |   | Database     |
+-------+      +------------+    +--------------+   +--------------+
    |                |                 |                   |
    | Access Homepage|                 |                   |
    |--------------->|                 |                   |
    |                | GET /           |                   |
    |                |---------------->|                   |
    |                |                 | get_recent_results()|
    |                |                 |------------------>|
    |                |                 |                   | Query Results
    |                |                 |                   |----
    |                |                 |                   |   |
    |                |                 |                   |<---
    |                |                 |<------------------|
    |                |                 |                   |
    |                | Render index.html with recent results|
    |                |<----------------|                   |
    | View Homepage  |                 |                   |
    |<---------------|                 |                   |
    |                |                 |                   |
```

## Educational Content Access Sequence

```
+-------+      +------------+    +--------------+
| User  |      | Web UI     |    | Flask App    |
+-------+      +------------+    +--------------+
    |                |                 |
    | Access About   |                 |
    |--------------->|                 |
    |                | GET /about      |
    |                |---------------->|
    |                | Render about.html|
    |                |<----------------|
    | View About Page|                 |
    |<---------------|                 |
    |                |                 |
    | Access How It Works |            |
    |--------------->|                 |
    |                | GET /how_it_works|
    |                |---------------->|
    |                | Render how_it_works.html|
    |                |<----------------|
    | View How It Works|               |
    |<---------------|                 |
    |                |                 |
    | Access Convolution Demo |        |
    |--------------->|                 |
    |                | GET /convolution|
    |                |---------------->|
    |                | Render convolution.html|
    |                |<----------------|
    | View Demo      |                 |
    |<---------------|                 |
    |                |                 |
    | Access Quiz    |                 |
    |--------------->|                 |
    |                | GET /quiz       |
    |                |---------------->|
    |                | Render quiz.html|
    |                |<----------------|
    | View Quiz      |                 |
    |<---------------|                 |
    |                |                 |
    | Access Gallery |                 |
    |--------------->|                 |
    |                | GET /gallery    |
    |                |---------------->|
    |                | Render gallery.html|
    |                |<----------------|
    | View Gallery   |                 |
    |<---------------|                 |
    |                |                 |
```

## Interactive Convolution Demo Sequence

```
+-------+      +------------+    +-------------------+
| User  |      | Web UI     |    | JavaScript (Client)|
+-------+      +------------+    +-------------------+
    |                |                      |
    | View Demo Page |                      |
    |--------------->|                      |
    |                | Load convolution.js  |
    |                |--------------------->|
    |                |                      | Initialize Demo
    |                |                      |----
    |                |                      |   |
    |                |                      |<---
    |                |                      |
    | Select Kernel  |                      |
    |--------------->|                      |
    |                | updateKernelTable()  |
    |                |--------------------->|
    |                |                      | Update Displayed Kernel
    |                |                      |----
    |                |                      |   |
    |                |                      |<---
    |                | Show Updated Kernel  |
    |                |<---------------------|
    | View Kernel    |                      |
    |<---------------|                      |
    |                |                      |
    | Apply Convolution|                    |
    |--------------->|                      |
    |                | applyConvolution()   |
    |                |--------------------->|
    |                |                      | Process Image with Kernel
    |                |                      |----
    |                |                      |   |
    |                |                      |<---
    |                | Show Processed Image |
    |                |<---------------------|
    | View Result    |                      |
    |<---------------|                      |
    |                |                      |
```

## Quiz Interaction Sequence

```
+-------+      +------------+    +-------------------+
| User  |      | Web UI     |    | JavaScript (Client)|
+-------+      +------------+    +-------------------+
    |                |                      |
    | View Quiz Page |                      |
    |--------------->|                      |
    |                | Load quiz.js         |
    |                |--------------------->|
    |                |                      | Initialize Quiz
    |                |                      |----
    |                |                      |   |
    |                |                      |<---
    |                |                      |
    | Start Quiz     |                      |
    |--------------->|                      |
    |                | startQuiz()          |
    |                |--------------------->|
    |                |                      | Load First Question
    |                |                      |----
    |                |                      |   |
    |                |                      |<---
    |                | Display Question     |
    |                |<---------------------|
    | View Question  |                      |
    |<---------------|                      |
    |                |                      |
    | Select Answer  |                      |
    |--------------->|                      |
    |                | selectOption()       |
    |                |--------------------->|
    |                |                      | Check Answer
    |                |                      |----
    |                |                      |   |
    |                |                      |<---
    |                | Show Feedback        |
    |                |<---------------------|
    | View Feedback  |                      |
    |<---------------|                      |
    |                |                      |
    | Next Question  |                      |
    |--------------->|                      |
    |                | nextQuestion()       |
    |                |--------------------->|
    |                |                      | Load Next Question
    |                |                      |----
    |                |                      |   |
    |                |                      |<---
    |                |                      |
    |                | Display Question     |
    |                |<---------------------|
    | View Question  |                      |
    |<---------------|                      |
    |                |                      |
    |       ...      |        ...           |           ...
    |                |                      |
    | Complete Quiz  |                      |
    |--------------->|                      |
    |                | showResults()        |
    |                |--------------------->|
    |                |                      | Calculate Score
    |                |                      |----
    |                |                      |   |
    |                |                      |<---
    |                | Display Results      |
    |                |<---------------------|
    | View Results   |                      |
    |<---------------|                      |
    |                |                      |
```