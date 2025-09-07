# Detailed Design Architecture (DDA)

**Project Title:** Deepfake Detection System using MesoNet  
**Version:** 1.0  
**Date:** April 12, 2025  
**Prepared by:** The DeepFake Detection Team  

## 1. Introduction

### 1.1 Purpose
This document provides a detailed design for the Deepfake Detection System, describing system architecture, component interactions, and implementation details. It serves as a guide for development and a reference for maintenance.

### 1.2 Scope
This document covers the detailed design of all system components, including:
- Web application architecture
- Database schema
- MesoNet model architecture
- Image preprocessing pipeline
- User interface design
- Educational components

### 1.3 Definitions and Acronyms
- **MVC:** Model-View-Controller architectural pattern
- **API:** Application Programming Interface
- **CNN:** Convolutional Neural Network
- **DB:** Database
- **HTML:** HyperText Markup Language
- **CSS:** Cascading Style Sheets
- **JS:** JavaScript

### 1.4 References
- Software Requirements Specification (SRS) document
- MesoNet: a Compact Facial Video Forgery Detection Network (Afchar et al., 2018)
- Flask Documentation (https://flask.palletsprojects.com/)
- Bootstrap Documentation (https://getbootstrap.com/docs/)

## 2. System Architecture

### 2.1 Architectural Overview
The Deepfake Detection System follows an MVC (Model-View-Controller) architecture pattern. The main components are:

1. **Presentation Layer (View):**
   - HTML templates with Bootstrap CSS
   - JavaScript for client-side interactivity

2. **Application Layer (Controller):**
   - Flask routes and request handlers
   - Image upload and validation
   - Result generation and display

3. **Business Logic Layer (Model):**
   - MesoNet model implementation
   - Image preprocessing
   - Face detection

4. **Data Layer:**
   - PostgreSQL database for logging results
   - File storage for temporary uploaded images

### 2.2 Component Diagram
[See separate diagram in docs/diagrams folder]

### 2.3 Deployment Architecture
- Web application hosted on a Python-compatible server with Gunicorn
- PostgreSQL database for persistent storage
- Static assets served via CDN or static file server

## 3. Component Design

### 3.1 Web Application (Flask)

#### 3.1.1 Module Structure
```
/
├── main.py              # Entry point for the application
├── app.py               # Flask application setup
├── mesonet.py           # MesoNet model implementation
├── preprocess.py        # Image preprocessing functions
├── database.py          # Database connection and operations
├── static/              # Static assets
│   ├── css/             # CSS stylesheets
│   ├── js/              # JavaScript files
│   ├── images/          # Static images
│   ├── uploads/         # User uploaded images (temporary)
│   └── model/           # Model weights
└── templates/           # HTML templates
    ├── layout.html      # Base template
    ├── index.html       # Homepage
    ├── result.html      # Results page
    ├── about.html       # About page
    ├── how_it_works.html# How it works page
    ├── convolution.html # Convolution demo page
    ├── quiz.html        # Quiz page
    ├── gallery.html     # Gallery page
    ├── 404.html         # Not found page
    └── 500.html         # Server error page
```

#### 3.1.2 Flask Routes
| Route | Method | Function | Description |
|-------|--------|----------|-------------|
| `/` | GET | `home()` | Displays the homepage with upload form |
| `/upload` | POST | `upload()` | Processes uploaded image and redirects to results |
| `/about` | GET | `about()` | Displays information about deepfakes |
| `/how_it_works` | GET | `how_it_works()` | Explains the detection methodology |
| `/convolution` | GET | `convolution()` | Shows interactive convolution demo |
| `/quiz` | GET | `quiz()` | Displays the deepfake quiz |
| `/gallery` | GET | `gallery()` | Shows example deepfake images |

#### 3.1.3 Error Handlers
| Error | Function | Description |
|-------|----------|-------------|
| 404 | `page_not_found()` | Handles not found errors |
| 413 | `too_large()` | Handles file too large errors |
| 500 | `server_error()` | Handles server errors |

### 3.2 Database Design

#### 3.2.1 PostgreSQL Schema
```sql
CREATE TABLE IF NOT EXISTS image_log (
    id SERIAL PRIMARY KEY,
    filename TEXT,
    result TEXT,
    confidence REAL,
    timestamp TIMESTAMP
);
```

#### 3.2.2 Entity Relationship Diagram
[See separate diagram in docs/diagrams folder]

### 3.3 MesoNet Model

#### 3.3.1 Architecture
The MesoNet model is a CNN with the following architecture:
- Input: 256x256x3 RGB image
- 4 convolutional blocks with batch normalization and max pooling
- Flatten layer
- 2 fully connected layers
- Output: Sigmoid activation for binary classification (Real/Fake)

```
Input(256, 256, 3)
↓
Conv2D(8, (3,3), padding='same', activation='relu')
BatchNormalization()
MaxPooling2D(pool_size=(2,2), padding='same')
↓
Conv2D(8, (5,5), padding='same', activation='relu')
BatchNormalization()
MaxPooling2D(pool_size=(2,2), padding='same')
↓
Conv2D(16, (5,5), padding='same', activation='relu')
BatchNormalization()
MaxPooling2D(pool_size=(2,2), padding='same')
↓
Conv2D(16, (5,5), padding='same', activation='relu')
BatchNormalization()
MaxPooling2D(pool_size=(4,4), padding='same')
↓
Flatten()
↓
Dense(16, activation='relu')
↓
Dense(1, activation='sigmoid')
```

#### 3.3.2 Model Functions
- `Meso4()`: Creates the MesoNet model architecture
- `load_model()`: Loads the model with pre-trained weights
- `predict(model, image)`: Makes a prediction on an input image

### 3.4 Image Preprocessing

#### 3.4.1 Processing Pipeline
1. Face detection using Haar Cascade Classifier
2. Image resizing to 256x256 pixels
3. Pixel normalization (0-1 range)

#### 3.4.2 Functions
- `is_valid_image(path)`: Checks if the image contains a detectable face
- `preprocess_image(path)`: Loads, resizes, and normalizes the image

## 4. User Interface Design

### 4.1 Homepage
- Upload form with drag-and-drop functionality
- Instructions for use
- Recent detection results
- Links to educational pages

### 4.2 Results Page
- Uploaded image display
- Classification result (Real/Fake)
- Confidence percentage
- Return to homepage option

### 4.3 Educational Pages
- About: Information on deepfake technology and implications
- How It Works: Explanation of MesoNet and detection methodology
- Convolution: Interactive demo of convolutional operations
- Quiz: Test knowledge about deepfakes
- Gallery: Example images with classifications

### 4.4 Wireframes
[See separate diagrams in docs/diagrams folder]

## 5. Algorithms and Pseudocode

### 5.1 Upload and Detection Process
```
FUNCTION upload():
    IF no file in request:
        Show error message
        Redirect to homepage
    
    IF file is empty:
        Show error message
        Redirect to homepage
    
    IF file type not allowed:
        Show error message
        Redirect to homepage
    
    Save file with unique name
    
    IF not is_valid_image(filepath):
        Show warning about no face detected
        Redirect to homepage
    
    preprocess_image(filepath)
    result, confidence = predict(model, image)
    log_result(filename, result, confidence)
    
    Display result page with classification
```

### 5.2 Face Detection
```
FUNCTION is_valid_image(path):
    Load face cascade classifier
    Load image in grayscale
    Detect faces using cascade
    Return true if at least one face detected
```

### 5.3 Image Preprocessing
```
FUNCTION preprocess_image(path):
    Load image
    Resize to 256x256 pixels
    Normalize pixel values to [0,1]
    Return processed image
```

### 5.4 MesoNet Prediction
```
FUNCTION predict(model, image):
    Add batch dimension to image
    Get prediction from model
    Extract confidence value
    
    IF confidence > 0.5:
        Return "Real", confidence
    ELSE:
        Return "Fake", 1.0 - confidence
```

## 6. Security Design

### 6.1 Input Validation
- Filename sanitization using secure_filename
- File type validation
- File size limitation

### 6.2 Error Handling
- Custom error pages
- Detailed logging
- Graceful failure modes

### 6.3 Privacy Considerations
- Temporary image storage only
- No user identification required
- Transparency about data processing

## 7. Performance Considerations

### 7.1 Optimization Strategies
- Efficient image preprocessing
- Database connection pooling
- Static asset caching

### 7.2 Scalability
- Stateless application design
- Database scaling via PostgreSQL features
- Independent components for easier scaling

## 8. Appendices

### 8.1 Appendix A: Class Diagram
[See separate diagram in docs/diagrams folder]

### 8.2 Appendix B: Sequence Diagram
[See separate diagram in docs/diagrams folder]