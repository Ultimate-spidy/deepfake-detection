# Software Requirements Specification (SRS)

**Project Title:** Deepfake Detection System using MesoNet  
**Version:** 1.0  
**Date:** April 12, 2025  
**Prepared by:** The DeepFake Detection Team  

## 1. Introduction

### 1.1 Purpose
This document specifies the requirements for the Deepfake Detection System, which aims to identify manipulated images (deepfakes) using the MesoNet convolutional neural network architecture. The system will provide a user-friendly interface for users to upload images and receive classifications with confidence scores.

### 1.2 Scope
The Deepfake Detection System will process uploaded images, analyze them using the pre-trained MesoNet model, and report whether the image contains a real face or a deepfake with an associated confidence score. The system will also include educational components to inform users about deepfake technology and detection methodologies.

### 1.3 Definitions and Acronyms
- **Deepfake:** Synthetic media where a person's likeness is replaced with someone else's using AI techniques
- **MesoNet:** A CNN architecture designed specifically for deepfake detection
- **CNN:** Convolutional Neural Network
- **UI:** User Interface
- **API:** Application Programming Interface

### 1.4 References
- MesoNet: a Compact Facial Video Forgery Detection Network (Afchar et al., 2018)
- Flask Documentation (https://flask.palletsprojects.com/)
- OpenCV Documentation (https://docs.opencv.org/)

### 1.5 Team Roles and Responsibilities

| Name | Role | Responsibilities |
|------|------|-----------------|
| Alice Sharma | Project Manager | Oversee project phases, timeline management, communication with stakeholders |
| Ravi Mehta | Lead Developer (ML/Python) | Develop and optimize MesoNet model, preprocessing, and integration |
| Priya Kapoor | Web Developer (Flask) | Create UI for upload, response page, backend connection to model |
| Anil Joshi | Database Engineer | Design and implement database schema, manage data flow and storage |

## 2. Overall Description

### 2.1 Product Perspective
The Deepfake Detection System is a web-based application that interfaces with users through a browser. It processes images using a machine learning model and displays results. The system includes both functional components (detection) and educational resources about deepfake technology.

### 2.2 Product Features
1. Image upload interface
2. Face detection preprocessing
3. Deepfake classification using MesoNet
4. Result visualization with confidence scores
5. Recent detection history
6. Educational content about deepfakes and detection methods
7. Interactive demo of convolutional operations
8. Quiz on deepfake technology

### 2.3 User Classes and Characteristics
- **General Users:** Individuals who want to check if an image is a deepfake
- **Students/Researchers:** Those who want to learn about deepfake technology and detection methods
- **Educators:** Those who want to use the system as a teaching tool

### 2.4 Operating Environment
- Web browsers: Chrome, Firefox, Safari, Edge (latest versions)
- Server: Python 3.11+, Flask, Gunicorn
- Database: PostgreSQL
- ML Framework: TensorFlow/Keras (implementation version)

### 2.5 Design and Implementation Constraints
- The system must use the MesoNet architecture for detection
- The web interface must be responsive for desktop and mobile devices
- Processing time per image should not exceed 5 seconds
- The system must securely handle user-uploaded content

### 2.6 Assumptions and Dependencies
- Pre-trained MesoNet model weights are available
- The system has sufficient computational resources for image processing
- Internet connectivity is available for web access

## 3. Specific Requirements

### 3.1 Functional Requirements

#### 3.1.1 User Interface
- FR1.1: The system shall provide a homepage with an image upload option
- FR1.2: The system shall validate that uploaded files are of an accepted image format (JPEG, PNG)
- FR1.3: The system shall enforce a maximum file size limit of 16MB
- FR1.4: The system shall display appropriate error messages for invalid uploads

#### 3.1.2 Image Processing
- FR2.1: The system shall detect if a face is present in the uploaded image
- FR2.2: The system shall preprocess images to meet the model's input requirements
- FR2.3: The system shall display a warning when no face is detected in an image

#### 3.1.3 Deepfake Detection
- FR3.1: The system shall classify images as "Real" or "Fake" using the MesoNet model
- FR3.2: The system shall display the classification result with a confidence percentage
- FR3.3: The system shall log detection results to a database

#### 3.1.4 Educational Components
- FR4.1: The system shall provide an "About" page with information on deepfakes
- FR4.2: The system shall provide a "How It Works" page explaining the detection methodology
- FR4.3: The system shall provide an interactive demonstration of convolutional operations
- FR4.4: The system shall include a quiz on deepfake technology
- FR4.5: The system shall display a gallery of example deepfakes and authentic images

### 3.2 Non-Functional Requirements

#### 3.2.1 Performance
- NFR1.1: The system shall process and classify images within 5 seconds
- NFR1.2: The system shall support concurrent uploads from at least 10 users
- NFR1.3: The system shall achieve a minimum classification accuracy of 85% on benchmark datasets

#### 3.2.2 Security
- NFR2.1: The system shall not store user-uploaded images permanently
- NFR2.2: The system shall sanitize user inputs to prevent injection attacks
- NFR2.3: The system shall use secure HTTP connections

#### 3.2.3 Usability
- NFR3.1: The user interface shall be intuitive and require no prior training
- NFR3.2: The system shall display loading indicators during processing
- NFR3.3: The system shall provide clear instructions at each step
- NFR3.4: The system shall be responsive on different screen sizes

#### 3.2.4 Reliability
- NFR4.1: The system shall handle errors gracefully without crashing
- NFR4.2: The system shall provide appropriate error messages for system failures
- NFR4.3: The system shall maintain a 99% uptime

## 4. External Interface Requirements

### 4.1 User Interfaces
- UI1: Homepage with upload functionality and recent detections
- UI2: Results page with classification and confidence score
- UI3: About page with information on deepfake technology
- UI4: How It Works page explaining MesoNet
- UI5: Interactive convolution demonstration page
- UI6: Quiz page for testing knowledge
- UI7: Gallery page with example images
- UI8: Error pages (404, 500, etc.)

### 4.2 Hardware Interfaces
- Not applicable for web-based application

### 4.3 Software Interfaces
- SI1: The system shall connect to a PostgreSQL database for storing detection logs
- SI2: The system shall use OpenCV for face detection and image preprocessing
- SI3: The system shall use TensorFlow/Keras for the MesoNet model (implementation version)

### 4.4 Communications Interfaces
- CI1: The system shall communicate with users via HTTP/HTTPS

## 5. Appendices

### 5.1 Appendix A: Use Case Diagram
[See separate diagram in docs/diagrams folder]

### 5.2 Appendix B: Data Flow Diagram
[See separate diagram in docs/diagrams folder]