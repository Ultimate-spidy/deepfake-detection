# Entity Relationship Diagram for Deepfake Detection System

## Logical ERD

```
+-------------------+
|     image_log     |
+-------------------+
| PK id             |
|    filename       |
|    result         |
|    confidence     |
|    timestamp      |
+-------------------+
```

## Physical ERD (PostgreSQL)

```
+-----------------------------------+
|             image_log             |
+-----------------------------------+
| PK id: SERIAL                     |
|    filename: TEXT                 |
|    result: TEXT                   |
|    confidence: REAL               |
|    timestamp: TIMESTAMP           |
+-----------------------------------+
```

## Entity Descriptions

### image_log
This entity stores the results of deepfake detection operations.

#### Attributes:
- **id**: A unique identifier for each detection result (Primary Key)
  - Data Type: SERIAL (auto-incrementing integer)
  - Constraints: PRIMARY KEY, NOT NULL
  
- **filename**: The name of the processed image file
  - Data Type: TEXT
  - Description: The unique filename of the uploaded image, including timestamp prefix
  - Example: "20250412_161536_example.jpg"
  
- **result**: The detection result classification
  - Data Type: TEXT
  - Description: Whether the image was classified as "Real" or "Fake"
  - Constraints: Limited to "Real" or "Fake" values
  
- **confidence**: The confidence score of the classification
  - Data Type: REAL
  - Description: A decimal value between 0.0 and 1.0 representing the model's confidence
  - Range: 0.0 to 1.0
  
- **timestamp**: When the detection was performed
  - Data Type: TIMESTAMP
  - Description: Date and time when the detection was performed
  - Default: Current timestamp

## Database Implementation

The PostgreSQL database schema is created with the following SQL:

```sql
CREATE TABLE IF NOT EXISTS image_log (
    id SERIAL PRIMARY KEY,
    filename TEXT,
    result TEXT,
    confidence REAL,
    timestamp TIMESTAMP
);
```

## Indexing Strategy

For improved query performance, especially for the frequently accessed recent results, we can add the following index:

```sql
CREATE INDEX idx_image_log_timestamp ON image_log (timestamp DESC);
```

This index will optimize the retrieval of recent detection results that are displayed on the homepage.

## Data Integrity Constraints

Although not explicitly defined in the current schema implementation, the following constraints are enforced at the application level:

1. **result** values are limited to "Real" or "Fake"
2. **confidence** values are constrained to the range 0.0 to 1.0
3. **filename** values are unique due to the timestamp prefix added during upload

## Future Schema Extensions

In future versions of the application, the database schema could be extended to include:

1. **User accounts** for personalized detection history:
```
+-------------------+       +-------------------+
|       users       |       |     image_log     |
+-------------------+       +-------------------+
| PK user_id        |------>| PK id             |
|    username       |       |    filename       |
|    email          |       |    result         |
|    password_hash  |       |    confidence     |
|    created_at     |       |    timestamp      |
+-------------------+       | FK user_id        |
                            +-------------------+
```

2. **Detection models** for tracking which model version was used:
```
+-------------------+       +-------------------+       +-------------------+
|      models       |       |    detections     |       |     image_log     |
+-------------------+       +-------------------+       +-------------------+
| PK model_id       |------>| PK detection_id   |------>| PK id             |
|    name           |       | FK model_id       |       |    filename       |
|    version        |       | FK image_log_id   |       |    result         |
|    accuracy       |       |    model_confidence|       |    confidence     |
|    created_at     |       +-------------------+       |    timestamp      |
+-------------------+                                   +-------------------+
```

3. **Feedback** for user-provided corrections to improve the model:
```
+-------------------+       +-------------------+
|     image_log     |       |     feedback      |
+-------------------+       +-------------------+
| PK id             |------>| PK feedback_id    |
|    filename       |       | FK image_log_id   |
|    result         |       |    user_correction|
|    confidence     |       |    submitted_at   |
|    timestamp      |       +-------------------+
+-------------------+
```

These extensions would support enhanced functionality while maintaining data integrity and proper relational design principles.