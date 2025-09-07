# State Transition Diagram for Deepfake Detection System

## Image Upload and Processing State Transitions

```
                   +-------------------+
                   |                   |
      +----------->|   Initial State   |
      |            |   (Homepage)      |
      |            |                   |
      |            +-------------------+
      |                     |
      |                     | [User selects image]
      |                     v
      |            +-------------------+
      |            |                   |
      |            |   Image Selected  |
      |            |                   |
      |            +-------------------+
      |                     |
      |                     | [User clicks "Detect Deepfake"]
      |                     v
      |            +-------------------+
      |            |                   |
      |            |  Validating Image |
      |            |                   |
      |            +-------------------+
      |                     |
      |                     | [Decision Point]
      |                     v
      |                    / \
      |                   /   \
      |     [Invalid]    /     \    [Valid]
      |                 /       \
      |                v         v
      |    +-------------------+ +-------------------+
      |    |                   | |                   |
      |    |   Error State     | |   Face Detection  |
      |    |                   | |                   |
      |    +-------------------+ +-------------------+
      |             |                     |
      |             |                     | [Decision Point]
      |             |                     v
      |             |                    / \
      |             |                   /   \
      |             |    [No Face]     /     \    [Face Detected]
      |             |                 /       \
      |             |                v         v
      |             |    +-------------------+ +-------------------+
      |             |    |                   | |                   |
      |             |    |   Warning State   | |   Processing      |
      |             |    |                   | |                   |
      |             |    +-------------------+ +-------------------+
      |             |             |                     |
      |             |             |                     | [Image processed]
      |             |             |                     v
      |             |             |           +-------------------+
      |             |             |           |                   |
      |             |             |           |   Classifying     |
      |             |             |           |                   |
      |             |             |           +-------------------+
      |             |             |                     |
      |             |             |                     | [Decision Point]
      |             |             |                     v
      |             |             |                    / \
      |             |             |                   /   \
      |             |             |     [Fake]       /     \    [Real]
      |             |             |                 /       \
      |             |             |                v         v
      |             |             |    +-------------------+ +-------------------+
      |             |             |    |                   | |                   |
      |             |             |    |   Fake Result     | |   Real Result     |
      |             |             |    |                   | |                   |
      |             |             |    +-------------------+ +-------------------+
      |             |             |                |                |
      |             |             |                | [User views]   | [User views]
      |             |             |                v                v
      |             |             |              +-------------------+
      |             |             |              |                   |
      +-------------+-------------+--------------+   Results Page    |
                                                 |                   |
                                                 +-------------------+
                                                          |
                                                          | [User clicks "Try Another"]
                                                          v
                                                 +-------------------+
                                                 |                   |
                                                 |   Initial State   |
                                                 |   (Homepage)      |
                                                 |                   |
                                                 +-------------------+
```

## Educational Content Navigation State Transitions

```
                                          +-----------------+
                                          |                 |
             +------------------------+-->|    Homepage     |<--+
             |                        |   |                 |   |
             |                        |   +-----------------+   |
             |                        |           |             |
             |                        |           | [Navigate]  |
             |                        |           v             |
             |                       / \------------------------/ \
             |       [About]        /   \        [How]         /   \  [Convolution]
             |                     /     \                    /     \
             v                    v       v                  v       v
      +-----------------+ +-----------------+ +-----------------+ +-----------------+
      |                 | |                 | |                 | |                 |
      |    About Page   | |   How It Works  | |   Convolution   | |      Quiz       |
      |                 | |      Page       | |      Demo       | |      Page       |
      +-----------------+ +-----------------+ +-----------------+ +-----------------+
             |                    |                  |                    |
             |                    |                  | [Select Kernel]    | [Start Quiz]
             |                    |                  v                    v
             |                    |           +-----------------+ +-----------------+
             |                    |           |                 | |                 |
             |                    |           | Kernel Selected | |  Quiz Started   |
             |                    |           |                 | |                 |
             |                    |           +-----------------+ +-----------------+
             |                    |                  |                    |
             |                    |                  | [Apply]            | [Answer]
             |                    |                  v                    v
             |                    |           +-----------------+ +-----------------+
             |                    |           |                 | |                 |
             |                    |           |  Filtering      | |  Question with  |
             |                    |           |  Applied        | |    Feedback     |
             |                    |           |                 | |                 |
             |                    |           +-----------------+ +-----------------+
             |                    |                  |                    |
             |                    |                  | [Reset]            | [Next Q/Finish]
             |                    |                  |                    v
             |                    |                  |           +-----------------+
             |                    |                  |           |                 |
             |                    |                  |           |  Quiz Results   |
             |                    |                  |           |                 |
             |                    |                  |           +-----------------+
             |                    |                  |                    |
             | [Return to Home]   | [Return to Home] | [Return to Home]  | [Return to Home]
             |                    |                  |                    |
             +--------------------+------------------+--------------------+
```

## State Descriptions

### Image Processing States

- **Initial State (Homepage)**: User is at the application homepage with option to upload an image
- **Image Selected**: User has selected an image, but not yet submitted for processing
- **Validating Image**: System is checking if the image meets format and size requirements
- **Error State**: Image failed validation (wrong format, too large, etc.)
- **Face Detection**: System is scanning for faces in the validated image
- **Warning State**: No face detected in the image
- **Processing**: Image is being preprocessed for the model
- **Classifying**: MesoNet model is analyzing the image
- **Fake Result**: Image classified as a deepfake
- **Real Result**: Image classified as containing a real face
- **Results Page**: Showing classification results with confidence score

### Educational Content States

- **Homepage**: Starting point for navigation
- **About Page**: Information about deepfake technology
- **How It Works Page**: Explanation of MesoNet and detection methodology
- **Convolution Demo**: Interactive demonstration
- **Kernel Selected**: User has chosen a convolution kernel
- **Filtering Applied**: Kernel has been applied to the image
- **Quiz Page**: Entry point for the knowledge quiz
- **Quiz Started**: User has begun the quiz
- **Question with Feedback**: User has answered a question and received feedback
- **Quiz Results**: Summary of quiz performance

## State Transitions

### Image Processing Transitions

- **[User selects image]**: User chooses an image file from their device
- **[User clicks "Detect Deepfake"]**: User submits the image for analysis
- **[Decision Point - Valid/Invalid]**: System determines if the image is valid
- **[Decision Point - Face/No Face]**: System determines if a face is present
- **[Image processed]**: Preprocessing completed successfully
- **[Decision Point - Real/Fake]**: Classification decision by the model
- **[User views]**: User views the detection result
- **[User clicks "Try Another"]**: User returns to homepage to upload another image

### Educational Content Transitions

- **[Navigate]**: User navigates to a different section
- **[Select Kernel]**: User selects a convolution kernel type
- **[Apply]**: User applies the kernel to see the effect
- **[Reset]**: User resets the demo to its initial state
- **[Start Quiz]**: User begins the knowledge quiz
- **[Answer]**: User submits an answer to a quiz question
- **[Next Q/Finish]**: User moves to the next question or completes the quiz
- **[Return to Home]**: User navigates back to the homepage