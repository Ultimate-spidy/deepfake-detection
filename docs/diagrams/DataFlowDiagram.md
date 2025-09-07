# Data Flow Diagram for Deepfake Detection System

## Context Level (Level 0) DFD

```
              +----------+
              |          |
     +------->+   User   +------+
     |        |          |      |
     |        +----------+      |
     |                          |
     |                          v
     |                    +-------------+
     |                    |             |
+----+--------------------+  Deepfake   |
|    |                    |  Detection  |
|    |                    |   System    |
|    |                    |             |
|    |                    +-------------+
|    |                          ^
|    |                          |
|    |        +----------+      |
|    |        |          |      |
|    +--------+ Database +------+
             |          |
             +----------+
```

## Level 1 DFD

```
                            +----------+
                            |          |
             +------------->+   User   +------------+
             |              |          |            |
             |              +----------+            |
             |                                      |
             |                                      v
             |                               +--------------+
             |                               |              |
     +-------+------+                        |   Upload     |
     |              |                        |   Image      |
     |   View       |                        |              |
     |   Results    |                        +--------------+
     |              |                               |
     +--------------+                               |
             ^                                      v
             |                               +--------------+
             |                               |              |
     +-------+------+                        |   Validate   |
     |              |                        |   Image      |
     |   Generate   |                        |              |
     |   Results    |                        +--------------+
     |              |                               |
     +--------------+                               |
             ^                                      v
             |                               +--------------+
             |                               |              |
     +-------+------+                        |   Detect     |
     |              |                        |    Face      |
     |   Process    |                        |              |
     |   Image      |                        +--------------+
     |              |                               |
     +--------------+                               |
             ^                                      v
             |                               +--------------+
             |                               |              |
     +-------+------+                        |  Preprocess  |
     |              |                        |    Image     |
     |   MesoNet    |                        |              |
     |   Model      |                        +--------------+
     |              |                               |
     +--------------+                               |
             ^                                      |
             |                                      |
             |                                      |
             +--------------------------------------+
             
             
     +-------+------+                        +--------------+
     |              |                        |              |
     |   Access     +<-----------------------+   Browse     |
     |  Educational |                        | Educational  |
     |   Content    |                        |   Content    |
     |              |                        |              |
     +--------------+                        +--------------+
             ^                                      ^
             |                                      |
             |                                      |
             |                                      |
             |                                      |
             |              +----------+            |
             |              |          |            |
             +--------------+   User   +------------+
                            |          |
                            +----------+


     +-------+------+                        +--------------+
     |              |                        |              |
     |    Log       |                        |   Query      |
     |   Results    +----------------------->+  Database    |
     |              |                        |              |
     +--------------+                        +--------------+
             ^                                      ^
             |                                      |
             |                                      |
     +-------+------+                        +--------------+
     |              |                        |              |
     |   Generate   |                        |    View      |
     |   Results    |                        |   Results    |
     |              |                        |              |
     +--------------+                        +--------------+
```

## Level 2 DFD: Image Processing and Classification

```
                      +---------------+
                      |               |
                      |  Preprocessed |
                      |     Image     |
                      |               |
                      +---------------+
                             |
                             v
              +-----------------------------+
              |                             |
              |  MesoNet Model Processing   |
              |                             |
              +-----------------------------+
                  |                   |
                  |                   |
     +------------v-----+    +-------v-----------+
     |                  |    |                   |
     | Convolutional    |    | Feature           |
     | Layer Processing |    | Extraction        |
     |                  |    |                   |
     +------------------+    +-------------------+
                  |                   |
                  |                   |
                  v                   v
              +-----------------------------+
              |                             |
              |   Feature Map Calculation   |
              |                             |
              +-----------------------------+
                             |
                             v
              +-----------------------------+
              |                             |
              |    Classification Layer     |
              |                             |
              +-----------------------------+
                  |                   |
                  |                   |
     +------------v-----+    +-------v-----------+
     |                  |    |                   |
     |    Real Image    |    |    Fake Image     |
     |   Classification |    |   Classification  |
     |                  |    |                   |
     +------------------+    +-------------------+
                  |                   |
                  |                   |
                  v                   v
              +-----------------------------+
              |                             |
              |    Confidence Calculation   |
              |                             |
              +-----------------------------+
                             |
                             v
              +-----------------------------+
              |                             |
              |     Classification Result   |
              |    (Real/Fake + Confidence) |
              |                             |
              +-----------------------------+
```

## Level 2 DFD: Database Operations

```
              +-----------------------------+
              |                             |
              |     Classification Result   |
              |    (Real/Fake + Confidence) |
              |                             |
              +-----------------------------+
                             |
                             v
              +-----------------------------+
              |                             |
              |      Log Result Function    |
              |                             |
              +-----------------------------+
                             |
                             v
              +-----------------------------+
              |                             |
              |   Database Connection Pool  |
              |                             |
              +-----------------------------+
                             |
                             v
              +-----------------------------+
              |                             |
              |       Execute SQL Insert    |
              |                             |
              +-----------------------------+
                             |
                             v
              +-----------------------------+
              |                             |
              |        image_log Table      |
              |                             |
              +-----------------------------+

              +-----------------------------+
              |                             |
              |    get_recent_results       |
              |        Function             |
              |                             |
              +-----------------------------+
                             |
                             v
              +-----------------------------+
              |                             |
              |   Database Connection Pool  |
              |                             |
              +-----------------------------+
                             |
                             v
              +-----------------------------+
              |                             |
              |       Execute SQL Query     |
              |                             |
              +-----------------------------+
                             |
                             v
              +-----------------------------+
              |                             |
              |        image_log Table      |
              |                             |
              +-----------------------------+
                             |
                             v
              +-----------------------------+
              |                             |
              |     Recent Results Data     |
              |                             |
              +-----------------------------+
                             |
                             v
              +-----------------------------+
              |                             |
              |       Homepage Display      |
              |                             |
              +-----------------------------+
```

## Data Dictionary

### External Entities
- **User:** End-user who interacts with the system
- **Database:** PostgreSQL database for storing detection results

### Processes
- **Upload Image:** Handles user image upload
- **Validate Image:** Checks if the image meets format and size requirements
- **Detect Face:** Uses OpenCV to detect faces in the image
- **Preprocess Image:** Resizes and normalizes the image for the model
- **MesoNet Model:** Analyzes the image for deepfake detection
- **Process Image:** Coordinates the image processing workflow
- **Generate Results:** Creates the classification result with confidence score
- **View Results:** Displays the detection results to the user
- **Log Results:** Stores the detection results in the database
- **Query Database:** Retrieves detection results from the database
- **Access Educational Content:** Provides educational information
- **Browse Educational Content:** User interaction with educational pages

### Data Stores
- **image_log Table:** Stores detection results with metadata

### Data Flows
- **Uploaded Image:** Image file from the user
- **Validated Image:** Image that passed format and size validation
- **Face Detection Result:** Result of face detection process
- **Preprocessed Image:** Normalized image ready for the model
- **Feature Maps:** Intermediate model processing results
- **Classification Result:** Final Real/Fake classification with confidence
- **Recent Results Data:** Historical detection results
- **Educational Content:** Information about deepfakes and detection methods