# Use Case Diagram for Deepfake Detection System

```
+------------------------------------------------------------------------------+
|                          Deepfake Detection System                           |
+------------------------------------------------------------------------------+
|                                                                              |
|  +--------+                                                                  |
|  |        |                                                                  |
|  |  User  |                                                                  |
|  |        |                                                                  |
|  +--------+                                                                  |
|      |                                                                       |
|      |                  +------------------------+                           |
|      +----------------->| Upload Image for       |                           |
|      |                  | Deepfake Detection     |                           |
|      |                  +------------------------+                           |
|      |                                                                       |
|      |                  +------------------------+                           |
|      +----------------->| View Detection Results |                           |
|      |                  +------------------------+                           |
|      |                                                                       |
|      |                  +------------------------+                           |
|      +----------------->| Learn About Deepfakes  |<-----------------------+  |
|      |                  +------------------------+                        |  |
|      |                              ^                                     |  |
|      |                              |                                     |  |
|      |                  +-----------+-----------+                         |  |
|      +----------------->| Explore How MesoNet   |------------------------>+  |
|      |                  | Detects Deepfakes     |                         |  |
|      |                  +-----------------------+                         |  |
|      |                                                                    |  |
|      |                  +------------------------+                        |  |
|      +----------------->| Interact with          |----------------------->+  |
|      |                  | Convolution Demo       |                        |  |
|      |                  +------------------------+                        |  |
|      |                                                                    |  |
|      |                  +------------------------+                        |  |
|      +----------------->| Take Deepfake Quiz     |----------------------->+  |
|      |                  +------------------------+                        |  |
|      |                                                                    |  |
|      |                  +------------------------+                        |  |
|      +----------------->| View Example Gallery   |----------------------->+  |
|                         +------------------------+                           |
|                                                                              |
|                                                                              |
|  +------------+          +----------------------+                            |
|  |            |          |                      |                            |
|  | Developer  |--------->| View System Logs     |                            |
|  |            |          |                      |                            |
|  +------------+          +----------------------+                            |
|                                                                              |
+------------------------------------------------------------------------------+
```

## Use Case Descriptions

### 1. Upload Image for Deepfake Detection
- **Actor:** User
- **Description:** User uploads an image to check if it contains a deepfake
- **Preconditions:** User has an image file
- **Main Flow:**
  1. User selects an image file from their device
  2. User submits the image for analysis
  3. System validates the image format and size
  4. System checks for a face in the image
  5. System processes the image through the MesoNet model
  6. System displays the detection result
- **Alternative Flows:**
  - If the image format is invalid, system shows an error message
  - If the image size exceeds the limit, system shows an error message
  - If no face is detected, system shows a warning message
- **Postconditions:** Detection result is displayed and logged in the database

### 2. View Detection Results
- **Actor:** User
- **Description:** User views the result of a deepfake detection analysis
- **Preconditions:** User has submitted an image for analysis
- **Main Flow:**
  1. System displays the uploaded image
  2. System shows the classification result (Real or Fake)
  3. System shows the confidence score
- **Postconditions:** User is informed about the detection result

### 3. Learn About Deepfakes
- **Actor:** User
- **Description:** User accesses information about deepfake technology
- **Main Flow:**
  1. User navigates to the About page
  2. System displays information about deepfakes, their creation, and implications
- **Postconditions:** User gains knowledge about deepfake technology

### 4. Explore How MesoNet Detects Deepfakes
- **Actor:** User
- **Description:** User learns about the MesoNet detection methodology
- **Main Flow:**
  1. User navigates to the How It Works page
  2. System displays information about MesoNet architecture and detection process
- **Postconditions:** User understands how the detection system works

### 5. Interact with Convolution Demo
- **Actor:** User
- **Description:** User interacts with a demo showing how convolutional operations work
- **Main Flow:**
  1. User navigates to the Convolution Demo page
  2. User selects different kernels and sees their effect on an image
  3. System displays the visual result of applying convolutions
- **Postconditions:** User understands the concept of convolution in CNNs

### 6. Take Deepfake Quiz
- **Actor:** User
- **Description:** User tests their knowledge about deepfakes through a quiz
- **Main Flow:**
  1. User navigates to the Quiz page
  2. User answers multiple-choice questions about deepfakes and detection
  3. System scores the quiz and provides feedback
- **Postconditions:** User receives a score and educational feedback

### 7. View Example Gallery
- **Actor:** User
- **Description:** User views example images of real and fake faces
- **Main Flow:**
  1. User navigates to the Gallery page
  2. System displays labeled examples of real and fake images
- **Postconditions:** User can visually compare real and deepfake images

### 8. View System Logs
- **Actor:** Developer
- **Description:** Developer accesses system logs for monitoring and debugging
- **Preconditions:** Developer has appropriate access credentials
- **Main Flow:**
  1. Developer accesses the logging system
  2. System displays application logs and detection history
- **Postconditions:** Developer can monitor system performance and diagnose issues