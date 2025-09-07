# Class Diagram for Deepfake Detection System

```
+---------------------------------------+       +-------------------------------------+
|               Flask App               |       |            Database Module          |
+---------------------------------------+       +-------------------------------------+
| - app: Flask                          |       | - get_db_connection()               |
| - model: MesoNet                      |       | - init_db()                         |
| - UPLOAD_FOLDER: str                  |       | - log_result(filename, result,      |
| - ALLOWED_EXTENSIONS: set             |       |    confidence)                      |
| - MAX_CONTENT_LENGTH: int             |       | - get_recent_results(limit)         |
+---------------------------------------+       +-------------------------------------+
| + allowed_file(filename): bool        |       |                                     |
| + is_valid_image(path): bool          |<----->|                                     |
| + home(): Response                    |       |                                     |
| + upload(): Response                  |       |                                     |
| + about(): Response                   |       |                                     |
| + how_it_works(): Response            |       |                                     |
| + convolution(): Response             |       |                                     |
| + gallery(): Response                 |       |                                     |
| + quiz(): Response                    |       |                                     |
| + too_large(e): Response              |       |                                     |
| + page_not_found(e): Response         |       |                                     |
| + server_error(e): Response           |       |                                     |
+---------------------------------------+       |                                     |
                   ^                            |                                     |
                   |                            |                                     |
                   |                            |                                     |
+------------------+--------------------+       |                                     |
|               Main Module             |       |                                     |
+---------------------------------------+       |                                     |
| + app: Flask (imported)               |       |                                     |
+---------------------------------------+       |                                     |
                   ^                            |                                     |
                   |                            |                                     |
+------------------+--------------------+       |                                     |
|            MesoNet Module             |       |                                     |
+---------------------------------------+       |                                     |
| - logger: Logger                      |       |                                     |
+---------------------------------------+       |                                     |
| + Meso4(): Model                      |<----->|                                     |
| + load_model(): Model                 |       |                                     |
| + predict(model, image): tuple        |       |                                     |
+---------------------------------------+       |                                     |
                   ^                            |                                     |
                   |                            |                                     |
                   |                            |                                     |
+------------------+--------------------+       |                                     |
|         Preprocessing Module          |       |                                     |
+---------------------------------------+       |                                     |
| - logger: Logger                      |       |                                     |
| - face_cascade: CascadeClassifier     |       |                                     |
+---------------------------------------+       |                                     |
| + preprocess_image(path): ndarray     |<------+                                     |
| + is_valid_image(path): bool          |                                             |
+---------------------------------------+                                             |
                                                                                      |
                                                                                      |
                                                                                      |
+---------------------------------------------------------------------------------+  |
|                              HTML Templates                                     |  |
+---------------------------------------------------------------------------------+  |
| - layout.html                                                                   |  |
| - index.html                                                                    |  |
| - result.html                                                                   |  |
| - about.html                                                                    |  |
| - how_it_works.html                                                             |  |
| - convolution.html                                                              |  |
| - quiz.html                                                                     |  |
| - gallery.html                                                                  |  |
| - 404.html                                                                      |  |
| - 500.html                                                                      |  |
+---------------------------------------------------------------------------------+  |
                    ^                                                                 |
                    |                                                                 |
                    |                                                                 |
+-------------------+-----------------------------------------------------------+     |
|                           Static Assets                                       |     |
+---------------------------------------------------------------------------------+  |
| - CSS (styles.css)                                                               |  |
| - JavaScript (script.js, convolution.js, quiz.js)                                |  |
| - Images (static images for the site)                                            |  |
| - Uploads (user-uploaded images)                                                 |  |
| - Model (model_weights.h5)                                                       |  |
+---------------------------------------------------------------------------------+  |
                                                  ^                                   |
                                                  |                                   |
                                                  |                                   |
+---------------------------------------------------------------------------------+  |
|                              PostgreSQL Database                                 |<-+
+---------------------------------------------------------------------------------+
| - image_log Table                                                                |
|   * id: SERIAL PRIMARY KEY                                                       |
|   * filename: TEXT                                                               |
|   * result: TEXT                                                                 |
|   * confidence: REAL                                                             |
|   * timestamp: TIMESTAMP                                                         |
+---------------------------------------------------------------------------------+
```

## Class and Module Descriptions

### Flask App Module (app.py)
- **Responsibilities:** Main web application controller, handling routes and requests
- **Attributes:**
  - `app`: Flask application instance
  - `model`: MesoNet model instance
  - `UPLOAD_FOLDER`: Directory for uploaded images
  - `ALLOWED_EXTENSIONS`: Set of allowed file extensions
  - `MAX_CONTENT_LENGTH`: Maximum allowed file size
- **Methods:**
  - Route handlers for various pages
  - Error handlers
  - Utility functions for file validation and image checking

### Main Module (main.py)
- **Responsibilities:** Application entry point for gunicorn
- **Attributes:**
  - `app`: Imported Flask application instance
- **Methods:** None (imports only)

### MesoNet Module (mesonet.py)
- **Responsibilities:** Defines and manages the MesoNet model
- **Attributes:**
  - `logger`: Logging instance
- **Methods:**
  - `Meso4()`: Creates the MesoNet model architecture
  - `load_model()`: Loads the model with pre-trained weights
  - `predict(model, image)`: Makes a prediction on an input image

### Preprocessing Module (preprocess.py)
- **Responsibilities:** Handles image preprocessing for the model
- **Attributes:**
  - `logger`: Logging instance
  - `face_cascade`: OpenCV face detection cascade classifier
- **Methods:**
  - `preprocess_image(path)`: Loads, resizes, and normalizes an image
  - `is_valid_image(path)`: Checks if an image contains a detectable face

### Database Module (database.py)
- **Responsibilities:** Manages database connections and operations
- **Methods:**
  - `get_db_connection()`: Creates a connection to the PostgreSQL database
  - `init_db()`: Initializes the database with required tables
  - `log_result(filename, result, confidence)`: Logs a detection result
  - `get_recent_results(limit)`: Retrieves recent detection results

### HTML Templates
- **Responsibilities:** Define the structure and presentation of web pages
- **Templates:**
  - `layout.html`: Base template with common elements
  - `index.html`: Homepage with upload form
  - `result.html`: Detection result display
  - `about.html`: Information about deepfakes
  - `how_it_works.html`: Explanation of detection methodology
  - `convolution.html`: Interactive convolution demo
  - `quiz.html`: Deepfake knowledge quiz
  - `gallery.html`: Example image gallery
  - Error pages (404, 500)

### Static Assets
- **Responsibilities:** Provide static resources for the web application
- **Categories:**
  - CSS: Styling for web pages
  - JavaScript: Client-side interactivity
  - Images: Static images for the site
  - Uploads: User-uploaded images (temporary storage)
  - Model: Pre-trained model weights

### PostgreSQL Database
- **Responsibilities:** Persistent storage for detection results
- **Tables:**
  - `image_log`: Stores detection results with timestamps

## Relationships

- The Flask App imports and uses the MesoNet, Preprocessing, and Database modules
- The Main module imports the Flask App
- The MesoNet and Preprocessing modules are independent of each other
- The Flask App renders HTML Templates
- HTML Templates use Static Assets (CSS, JavaScript)
- The Database module connects to the PostgreSQL Database