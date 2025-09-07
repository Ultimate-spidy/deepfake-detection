# Deployment Document

**Project Title:** Deepfake Detection System using MesoNet  
**Version:** 1.0  
**Date:** April 12, 2025  
**Prepared by:** The DeepFake Detection Team  

## 1. Introduction

### 1.1 Purpose
This document describes the deployment process and requirements for the Deepfake Detection System. It provides instructions for setting up the application in development, staging, and production environments.

### 1.2 Scope
This document covers:
- System requirements
- Environment setup
- Installation procedures
- Configuration management
- Deployment strategies
- Post-deployment verification
- Rollback procedures

### 1.3 Definitions and Acronyms
- **WSGI:** Web Server Gateway Interface
- **Gunicorn:** Green Unicorn, a Python WSGI HTTP Server
- **PostgreSQL:** Open-source relational database system
- **ENV:** Environment Variables
- **VCS:** Version Control System

## 2. System Requirements

### 2.1 Hardware Requirements
- **Server:** Virtual Machine or Physical Server
  - CPU: Minimum 2 cores, recommended 4 cores
  - RAM: Minimum 4GB, recommended 8GB
  - Disk Space: Minimum 20GB, recommended 50GB
  - Network: Stable internet connection with minimum 5 Mbps

### 2.2 Software Requirements
- **Operating System:** Linux (Ubuntu 20.04 LTS or newer)
- **Python:** Version 3.11 or newer
- **Database:** PostgreSQL 13 or newer
- **Web Server:** Gunicorn 23.0.0 or newer
- **Runtime Dependencies:**
  - Flask 3.1.0 or newer
  - Flask-SQLAlchemy 3.1.1 or newer
  - OpenCV-Python 4.11.0 or newer
  - Pillow 11.1.0 or newer
  - Psycopg2-Binary 2.9.10 or newer
  - TensorFlow 2.14.0 or newer (for full model implementation)
  - Other dependencies as specified in requirements.txt

## 3. Environment Setup

### 3.1 Development Environment
1. Clone the repository:
   ```bash
   git clone https://github.com/organization/deepfake-detection.git
   cd deepfake-detection
   ```

2. Create and activate a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Set up environment variables:
   ```bash
   export FLASK_APP=main.py
   export FLASK_ENV=development
   export DATABASE_URL=postgresql://username:password@localhost:5432/deepfake_db
   ```

5. Initialize the database:
   ```bash
   flask db init
   flask db migrate
   flask db upgrade
   ```

6. Run the development server:
   ```bash
   flask run --host=0.0.0.0 --port=5000
   ```

### 3.2 Staging Environment
1. Follow steps 1-5 from the Development Environment setup.

2. Configure Gunicorn:
   ```bash
   gunicorn --bind 0.0.0.0:5000 --workers=2 main:app
   ```

3. Set up Nginx as a reverse proxy (optional but recommended):
   ```nginx
   server {
       listen 80;
       server_name staging.deepfake-detection.com;

       location / {
           proxy_pass http://127.0.0.1:5000;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
       }
   }
   ```

4. Set up SSL with Let's Encrypt (optional):
   ```bash
   sudo certbot --nginx -d staging.deepfake-detection.com
   ```

### 3.3 Production Environment
1. Set up a production server with required hardware and software.

2. Use a deployment tool like Ansible or Docker for consistent deployment:
   
   Example Docker setup:
   ```dockerfile
   FROM python:3.11-slim

   WORKDIR /app

   COPY requirements.txt .
   RUN pip install --no-cache-dir -r requirements.txt

   COPY . .

   ENV DATABASE_URL=postgresql://username:password@db:5432/deepfake_db

   EXPOSE 5000

   CMD ["gunicorn", "--bind", "0.0.0.0:5000", "--workers=4", "main:app"]
   ```

3. Set up a production database with proper backup procedures.

4. Configure load balancing for high availability (if needed).

5. Set up monitoring and logging.

## 4. Installation Procedure

### 4.1 Prerequisites
- Access to the hosting environment
- Database credentials
- SSL certificates (for HTTPS)
- Domain configuration (if applicable)

### 4.2 Installation Steps
1. Prepare the server environment:
   ```bash
   sudo apt update
   sudo apt install python3-pip python3-venv postgresql nginx
   ```

2. Set up the PostgreSQL database:
   ```bash
   sudo -u postgres psql
   CREATE DATABASE deepfake_db;
   CREATE USER deepfake_user WITH ENCRYPTED PASSWORD 'secure_password';
   GRANT ALL PRIVILEGES ON DATABASE deepfake_db TO deepfake_user;
   \q
   ```

3. Clone and configure the application:
   ```bash
   git clone https://github.com/organization/deepfake-detection.git /opt/deepfake-detection
   cd /opt/deepfake-detection
   python3 -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```

4. Create a systemd service file for the application:
   ```
   [Unit]
   Description=Deepfake Detection Application
   After=network.target

   [Service]
   User=ubuntu
   WorkingDirectory=/opt/deepfake-detection
   Environment="PATH=/opt/deepfake-detection/venv/bin"
   Environment="DATABASE_URL=postgresql://deepfake_user:secure_password@localhost:5432/deepfake_db"
   ExecStart=/opt/deepfake-detection/venv/bin/gunicorn --workers 4 --bind 0.0.0.0:5000 main:app
   Restart=always

   [Install]
   WantedBy=multi-user.target
   ```

5. Enable and start the service:
   ```bash
   sudo systemctl enable deepfake-detection
   sudo systemctl start deepfake-detection
   ```

6. Configure Nginx as a reverse proxy:
   ```
   server {
       listen 80;
       server_name deepfake-detection.com www.deepfake-detection.com;

       location / {
           proxy_pass http://127.0.0.1:5000;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
       }
   }
   ```

7. Set up SSL with Let's Encrypt:
   ```bash
   sudo certbot --nginx -d deepfake-detection.com -d www.deepfake-detection.com
   ```

## 5. Configuration Management

### 5.1 Environment Variables
The application requires the following environment variables:

| Variable | Description | Default | Example |
|----------|-------------|---------|---------|
| DATABASE_URL | PostgreSQL connection string | None | postgresql://user:password@host:port/db |
| SESSION_SECRET | Secret key for Flask sessions | "deepfake_detection_secret" | "random_secret_key" |
| LOG_LEVEL | Logging level | "INFO" | "DEBUG" |

### 5.2 Configuration Files
- **Nginx:** `/etc/nginx/sites-available/deepfake-detection`
- **Systemd:** `/etc/systemd/system/deepfake-detection.service`
- **Application:** Configuration is primarily through environment variables

### 5.3 Version Control
- Use Git tags for version management
- Follow semantic versioning (X.Y.Z)
- Maintain a changelog with each release

## 6. Deployment Strategy

### 6.1 Deployment Process
1. **Code Freeze:** Stop accepting new features for the release
2. **Testing:** Conduct thorough testing in staging environment
3. **Release Preparation:** Create a release branch and tag
4. **Deployment:**
   - Backup the database
   - Pull the new version
   - Install dependencies
   - Migrate the database (if needed)
   - Restart the service
5. **Verification:** Run post-deployment tests
6. **Monitoring:** Monitor application logs and performance

### 6.2 Deployment Schedule
- **Minor Updates:** Bi-weekly during off-peak hours
- **Major Updates:** Monthly with planned downtime
- **Emergency Fixes:** As needed with minimal disruption

### 6.3 Blue-Green Deployment (Optional)
For zero-downtime deployments:
1. Set up identical "Blue" and "Green" environments
2. Deploy to the inactive environment
3. Test the deployment
4. Switch traffic to the updated environment
5. Keep the previous environment as a backup

## 7. Post-Deployment Verification

### 7.1 Smoke Tests
- Verify the application starts correctly
- Confirm the database connection is established
- Check that all static assets are loaded

### 7.2 Functional Tests
- Upload a sample image and verify detection works
- Navigate through all pages
- Test educational components

### 7.3 Performance Tests
- Verify response times are within acceptable limits
- Check resource utilization (CPU, memory, disk)
- Test with simulated concurrent users

## 8. Rollback Procedure

### 8.1 Rollback Triggers
- Critical functionality not working
- Significant performance degradation
- Security vulnerabilities discovered
- Data corruption or loss

### 8.2 Rollback Steps
1. Stop the application service:
   ```bash
   sudo systemctl stop deepfake-detection
   ```

2. Revert to the previous version:
   ```bash
   cd /opt/deepfake-detection
   git checkout [previous_tag]
   ```

3. Restore the database (if needed):
   ```bash
   pg_restore -d deepfake_db [backup_file]
   ```

4. Restart the service:
   ```bash
   sudo systemctl start deepfake-detection
   ```

5. Verify the rollback was successful

### 8.3 Post-Rollback Analysis
- Document the issues encountered
- Analyze root causes
- Develop a plan to fix the issues
- Update the deployment process if needed

## 9. Monitoring and Maintenance

### 9.1 Monitoring
- Application logs in `/var/log/deepfake-detection/`
- System metrics via Prometheus/Grafana (recommended)
- Database metrics and query performance
- Error tracking and alerting

### 9.2 Backup Strategy
- **Database:** Daily full backups, hourly incremental backups
- **Application Code:** Version controlled in Git
- **Uploads:** Daily backups to secure storage
- **Configuration:** Version controlled and backed up

### 9.3 Maintenance Tasks
- Security updates for dependencies
- Database optimization
- Log rotation and cleanup
- Certificate renewal

## 10. Security Considerations

### 10.1 Network Security
- Restrict server access via firewall rules
- Use HTTPS for all connections
- Implement rate limiting to prevent abuse

### 10.2 Application Security
- Keep dependencies updated
- Follow secure coding practices
- Validate all user inputs
- Implement proper error handling

### 10.3 Data Security
- Encrypt sensitive data in transit and at rest
- Implement proper access controls
- Regularly audit access logs
- Follow data retention policies

## 11. Troubleshooting

### 11.1 Common Issues
- **Application Won't Start:** Check logs, verify environment variables
- **Database Connection Failure:** Verify credentials and network access
- **Slow Performance:** Check server resources, optimize queries
- **Upload Errors:** Verify permissions on upload directory

### 11.2 Logging
- Application logs: `/var/log/deepfake-detection/app.log`
- Nginx logs: `/var/log/nginx/access.log` and `/var/log/nginx/error.log`
- System logs: `journalctl -u deepfake-detection.service`

### 11.3 Support Channels
- GitHub Issues: [github.com/organization/deepfake-detection/issues](https://github.com/organization/deepfake-detection/issues)
- Email Support: support@deepfake-detection.com
- Internal Documentation: [wiki.organization.com/deepfake-detection](https://wiki.organization.com/deepfake-detection)

## 12. Appendices

### 12.1 Appendix A: Environment Variable Reference
[Detailed list of all environment variables and their usage]

### 12.2 Appendix B: Database Schema
[Database schema diagrams and migration scripts]

### 12.3 Appendix C: Deployment Checklist
[Step-by-step checklist for deployments]