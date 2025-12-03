# Customer Churn Prediction Data Platform

**Keystone Data Solutions**

## Table of Contents
1. [Description of the Problem](#description-of-the-problem)
2. [Platform Architecture](#platform-architecture)
3. [Technology Stack](#technology-stack)
4. [System Requirements](#system-requirements)
5. [Installation Instructions](#installation-instructions)
6. [Running the Project](#running-the-project)
7. [Model Performance](#model-performance)
8. [Troubleshooting](#troubleshooting)

---

## Description of the Problem

### Executive Summary
Customer churn is a critical business challenge that directly impacts revenue and profitability. Acquiring new customers costs 5-25x more than retaining existing ones, making churn prevention a top priority for sustainable business growth.

**Keystone Data Solutions** has developed a comprehensive **Big Data Customer Churn Prediction Platform** that uses distributed computing and machine learning to identify customers at high risk of leaving before they churn. By predicting churn probability in real-time, businesses can deploy targeted, proactive retention strategies, significantly increasing customer lifetime value (CLV) and overall profitability.

### Keystone Data Solutions' Business Requirements

The goal of Keystone Data Solutions is to provide a real-time Customer Churn Prediction Platform for businesses and organizations. This platform enables companies to identify customers at high risk of churning before they leave, allowing deployment of targeted, proactive retention strategies.

### Key Stakeholders

1. **Marketing & Loyalty Teams**: Execute targeted retention campaigns based on churn scores
2. **Customer Service Management**: Prioritize high-value, at-risk customers for immediate outreach
3. **Executive Leadership**: Strategic planning and ROI measurement of retention initiatives
4. **Data Science & Analytics Team**: Model monitoring, evaluation, and continuous improvement

---

## Platform Architecture

### Keystone Data Solutions' Data Pipeline
```
┌─────────────────────────────────────────────────────────────────────────┐
│              Keystone Data Solutions Platform Architecture               │
└─────────────────────────────────────────────────────────────────────────┘

    Data Sources → Ingestion → Storage → Processing → ML Models → API/Dashboard
         ↓             ↓          ↓          ↓           ↓            ↓
    
  ┌──────────┐   ┌────────┐  ┌───────────┐  ┌──────┐  ┌─────────┐  ┌─────────┐
  │Customer  │──▶│ Kafka  │─▶│PostgreSQL │─▶│Pandas│─▶│ XGBoost │─▶│ FastAPI │
  │   Data   │   │Streams │  └───────────┘  │  ETL │  │   SVM   │  │   API   │
  │  (CSV,   │   └────────┘  ┌───────────┐  │ Numpy│  │Logistic │  └─────────┘
  │   JSON)  │               │ Cassandra │  └──────┘  │   Reg   │       │
  └──────────┘               │Time-Series│            └─────────┘       ↓
                             └───────────┘                         ┌──────────┐
                                                                   │Streamlit │
                                                                   │Dashboard │
                                                                   └──────────┘
```
---

## Technology Stack

### Keystone Data Solutions' Complete Tech Stack

| Layer | Technology | Purpose |
|-------|------------|---------|
| **Data Sources** | CSV, JSON, APIs | Customer data, transactions, behavior |
| **Ingestion** | **Apache Kafka** | Real-time event streaming |
| **Storage** | **PostgreSQL** | Structured relational data |
| | **Cassandra** | Unstructured/time-series data |
| **Processing** | **Pandas** | Data manipulation and ETL |
| | **NumPy** | Numerical computations |
| **ML Models** | **XGBoost** | Gradient boosting classifier |
| | **SVM** (Scikit-learn) | Support vector machine |
| | **Logistic Regression** | Linear classifier |
| | **Scikit-learn** | ML framework |
| | **Imbalanced-learn** | SMOTE for class imbalance |
| **API** | **FastAPI** | RESTful API for predictions |
| **Dashboard** | **Streamlit** | Interactive visualization |
| **Deployment** | **Docker** | Containerization |
| | **Docker Compose** | Multi-container orchestration |

### Platform Capabilities

#### 1. **Data Ingestion & Integration**
- **Apache Kafka**: Real-time streaming of customer events and behavior
- Batch ingestion via CSV/JSON files
- API connectors for third-party data sources

#### 2. **Data Storage & Organization**
- **PostgreSQL**: Master customer records, transactions, demographics
- **Cassandra**: High-volume event logs, clickstream data, time-series
- Optimized for both OLTP and OLAP workloads

#### 3. **Data Processing & Feature Engineering**
- **Pandas**: ETL pipeline, data cleaning, feature extraction
- **NumPy**: Mathematical operations, array processing
- Automated feature engineering (RFM scores, engagement metrics)

#### 4. **Predictive Modeling**
- **XGBoost**: Primary model for churn prediction
- **SVM with SMOTE**: Handling class imbalance
- **Logistic Regression**: Baseline model and interpretability
- Model comparison and ensemble methods

#### 5. **Application Layer**
- **FastAPI**: REST API for real-time predictions
- **Streamlit**: Interactive dashboard for stakeholders
- RESTful endpoints for integration with existing systems

---

## System Requirements

### Hardware
- **Minimum**: 8GB RAM, 4 CPU cores, 50GB disk space
- **Recommended**: 16GB RAM, 8 CPU cores, 100GB SSD

### Software
- **Operating System**: Ubuntu 22.04 LTS (or later)
- **Python**: 3.8 or higher
- **Docker**: Latest version
- **PostgreSQL**: 13 or higher
- **Apache Kafka**: 3.0 or higher (via Docker)
- **Cassandra**: 4.0 or higher (via Docker)

---

## Installation Instructions

### Step 1: Ubuntu System Setup

#### 1.1 Update System
```bash
sudo apt update
sudo apt upgrade -y
```

#### 1.2 Install Python 3 and pip
```bash
# Install Python, pip, venv, and git
sudo apt install python3-pip python3-venv git wget -y

# Verify installations
python3 --version
pip3 --version
git --version
```
---
### Step 2: Install Docker and Docker Compose
```bash
# Download Docker installation script
curl -fsSL https://get.docker.com -o get-docker.sh

# Run Docker installation
sudo sh get-docker.sh

# Add user to docker group
sudo usermod -aG docker $USER

# Activate group membership
newgrp docker

# Verify Docker installation
docker --version

# Remove old docker-compose (if exists)
sudo apt remove docker-compose -y

# Install Docker Compose plugin
sudo apt install docker-compose-plugin -y

# Verify Docker Compose
docker compose version
```

---
### Step 3: Clone Repository and Install Python Dependencies

#### 3.1 Clone Repository
```bash
# Clone the Keystone Data Solutions project
cd ~
git clone https://github.com/keystone-data-solutions/churn-prediction-platform.git
cd...
```
###  Instead of cloning, you can manually create directories
```bash
mkdir -p docs \                                                     
         data/raw \
         data/processed \
         data/models \
         notebooks \
         src \
         dashboard \
         tests \
         vm-setup
```
---
### Step 4: Set Up Infrastructure with Docker Compose
```bash
nano docker-compose.yml
```
### Copy and paste Docker Compose Configuration below

Create `docker-compose.yml` in your project root:
```yaml
version: '3.8'

services:
  postgres:
    image: postgres:14
    container_name: churn-postgres
    environment:
      POSTGRES_USER: churn_user
      POSTGRES_PASSWORD: churn_pass
      POSTGRES_DB: churn_db
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: unless-stopped

  cassandra:
    image: cassandra:4.1
    container_name: churn-cassandra
    ports:
      - "9042:9042"
    environment:
      CASSANDRA_CLUSTER_NAME: ChurnCluster
      CASSANDRA_DC: dc1
      CASSANDRA_RACK: rack1
    volumes:
      - cassandra_data:/var/lib/cassandra
    restart: unless-stopped

  kafka:
    image: confluentinc/cp-kafka:7.5.0
    container_name: churn-kafka
    ports:
      - "9092:9092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
    depends_on:
      - zookeeper
    restart: unless-stopped

  zookeeper:
    image: confluentinc/cp-zookeeper:7.5.0
    container_name: churn-zookeeper
    ports:
      - "2181:2181"
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
    volumes:
      - zookeeper_data:/var/lib/zookeeper
    restart: unless-stopped

volumes:
  postgres_data:
  cassandra_data:
  zookeeper_data:
```
---
### Step 5: Set Up Infrastructure with Docker Compose
```bash
nano requirements.txt
```
### Copy and paste requirements below
```bash
# Data Processing
pandas==2.0.0
numpy==1.24.0

# Machine Learning
scikit-learn==1.3.0
xgboost==1.7.6
imbalanced-learn==0.11.0

# Visualization
matplotlib==3.7.0
seaborn==0.12.0

# Jupyter
jupyter==1.0.0
notebook==7.0.0

# Database Connectors
psycopg2-binary==2.9.9
cassandra-driver==3.28.0
sqlalchemy==2.0.0

# Kafka
kafka-python==2.0.2

# API & Dashboard
fastapi==0.104.1
uvicorn==0.24.0
streamlit==1.28.0
pydantic==2.5.0

# Utilities
joblib==1.3.0
python-dotenv==1.0.0
requests==2.31.0
```
---
### 5.1 Create .gitignore
```bash
nano .gitignore
```
# copy and paste file below:
```bash
# -------------------------
# PYTHON
# -------------------------
**/__pycache__/
*.py[cod]
*$py.class
*.so
.Python

venv/
env/
ENV/

*.egg-info/
dist/
build/
*.whl

# Typing / Testing
.mypy_cache/
.pytest_cache/

# -------------------------
# NOTEBOOKS
# -------------------------
.ipynb_checkpoints/
*.ipynb_checkpoints
*.ipynb_autosave

# -------------------------
# DATA FILES
# -------------------------
data/raw/*
data/processed/*
data/models/*
!data/raw/.gitkeep
!data/processed/.gitkeep
!data/models/.gitkeep
```
---
### Step 6: Starting Docker services
```bash
# Start all services (first time will download images - takes 3-5 minutes)
docker compose up -d

# Wait for services to initialize
sleep 30

# Verify all services are running
docker compose ps

# Output: 4 containers (postgres, cassandra, kafka, zookeeper) with status "Up"
```
---
### Step 7: Create Virtual Environment
```bash
# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate

# Upgrade pip
pip install --upgrade pip
# pip install -r requirements.txt

# Verify packages
pip list | grep -E "pandas|scikit-learn|fastapi|streamlit|cassandra"

```
---
### Step 8: Download Dataset

```bash
Download IBM Telco Customer Churn dataset        
wget -O data/raw/telco_churn.csv https://raw.githubusercontent.com/IBM/telco-customer-churn-on-icp4d/master/data/Telco-Customer-Churn.csv

# Verify the download
ls -lh data/raw/
wc -l data/raw/telco_churn.csv
# Output: ~7044 lines (7043 customers + 1 header)
```
---

### Step 9: Create Database Scripts
```bash
nano src/db_postgres.py
```
# Create Cassandra handler
```bash
nano src/db_cassandra.py
```
# Create Data ingestion file
```bash
nano src/ingest.py
```
---
### Step 10: Database initialization
```bash
# Test PostgreSQL connection
python src/db_postgres.py --test
# Output: ✓ Connected to PostgreSQL

# Initialize PostgreSQL schema
python src/db_postgres.py --init
# Output: ✓ Table 'customers' created

# Test Cassandra connection
python src/db_cassandra.py --test
# Output: ✓ Connected to Cassandra

# Initialize Cassandra schema
python src/db_cassandra.py --init
# Output: ✓ Keyspace and tables created
```
---
### Step 11: Load Data
```bash
# Load customer data into PostgreSQL
python src/ingest.py --batch data/raw/telco_churn.csv
# Output: ✓ Inserted 7043 customers into PostgreSQL

# Generate sample events in Cassandra
python src/ingest.py --events 100
# Output: ✓ Inserted 100 events into Cassandra

# Generate sample support tickets in Cassandra
python src/ingest.py --tickets 50
# Output: ✓ Inserted 50 support tickets into Cassandra
```
---
### Step 12: Test and Verify
```bash
# Check all Docker containers are running
docker compose ps

# Test database connections
python src/db_postgres.py --test
python src/db_cassandra.py --test

# Check data loaded
# (We'll add query scripts later)
```
---

## Troubleshooting

### Common Issues

#### 1. Docker Containers Won't Start
```bash
# Check Docker is running
sudo systemctl status docker

# Restart Docker
sudo systemctl restart docker

# Check container logs
docker-compose logs -f

# Restart specific service
docker-compose restart postgres
```

#### 2. Database Connection Errors
```bash
# PostgreSQL
docker exec -it postgres-churn psql -U keystonedata -d churn_db

# Cassandra
docker exec -it cassandra-churn cqlsh

# Check if services are listening
sudo netstat -tulpn | grep -E '5432|9042|9092'
```

#### 3. Port Already in Use
```bash
# Find process using port
sudo lsof -i :8000

# Kill process
sudo kill -9 <PID>

# Or use different port
uvicorn src.api.main:app --port 8001
```

#### 4. Python Package Issues
```bash
# Reinstall specific package
pip install --upgrade --force-reinstall xgboost

# Clear pip cache
pip cache purge

# Reinstall all
pip install -r requirements.txt --force-reinstall
```

---

## Monitoring & Maintenance

### Access Services

| Service | URL | Credentials |
|---------|-----|-------------|
| FastAPI Docs | http://localhost:8000/docs | - |
| Streamlit Dashboard | http://localhost:8501 | - |
| PostgreSQL | localhost:5432 | keystonedata/keystonedata2024 |
| Cassandra | localhost:9042 | - |
| Kafka | localhost:9092 | - |

### Check Services Status
```bash
# Docker services
docker ps

# System resources
htop

# Database connections
docker exec -it postgres-churn pg_isready
```

### Stop All Services
```bash
# Stop Docker services
docker-compose down

# Stop Docker and remove volumes
docker-compose down -v

# Deactivate Python environment
deactivate
```

---

## Future Enhancements

1. **Kubernetes Deployment**: Auto-scaling and orchestration
2. **MLflow Integration**: Experiment tracking and model registry
3. **Apache Airflow**: Workflow orchestration
4. **Grafana Dashboard**: Real-time monitoring
5. **Mobile App**: iOS/Android for on-the-go insights
6. **A/B Testing Framework**: Measure retention campaign effectiveness

---

## References

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [Streamlit Documentation](https://docs.streamlit.io/)
- [Apache Kafka Documentation](https://kafka.apache.org/documentation/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Cassandra Documentation](https://cassandra.apache.org/doc/)
- [XGBoost Documentation](https://xgboost.readthedocs.io/)
- [Scikit-learn Documentation](https://scikit-learn.org/)

---

## License
MIT License - Copyright (c) 2024 Keystone Data Solutions

---

## Contact

**Keystone Data Solutions**  
*Transforming Data into Actionable Insights*

- **Email**: info@keystonedatasolutions.com
- **GitHub**: [https://github.com/keystone-data-solutions](https://github.com/keystone-data-solutions)
- **LinkedIn**: [Keystone Data Solutions](https://linkedin.com/company/keystone-data-solutions)


---

## Acknowledgments

- Dataset: IBM Sample Telco Customer Churn
- Our sincere regards to our stakholders
- Open-source community for invaluable tools and resources

---

**© 2025 Keystone Data Solutions. All Rights Reserved.**

*Empowering businesses through predictive analytics and big data solutions.*
