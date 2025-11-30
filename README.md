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

### Detailed Architecture Diagram
```
┌─────────────────────────────────────────────────────────────────────────┐
│                          DATA INGESTION LAYER                            │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                           │
│  ┌──────────────────┐         ┌───────────────────────────────────┐    │
│  │  Data Sources    │         │      Apache Kafka                 │    │
│  │                  │────────▶│   (Real-time Event Streaming)     │    │
│  │ • CSV Files      │         │   - Customer events               │    │
│  │ • JSON Streams   │         │   - Transaction logs              │    │
│  │ • APIs           │         │   - Behavioral data               │    │
│  └──────────────────┘         └───────────────┬───────────────────┘    │
│                                                │                         │
└────────────────────────────────────────────────┼─────────────────────────┘
                                                 │
┌────────────────────────────────────────────────┼─────────────────────────┐
│                          STORAGE LAYER         ↓                         │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                           │
│  ┌────────────────────────────┐      ┌────────────────────────────┐    │
│  │       PostgreSQL           │      │       Cassandra            │    │
│  │  (Structured Data)         │      │  (Unstructured/Time-Series)│    │
│  │                            │      │                            │    │
│  │ • Customer profiles        │      │ • Event logs               │    │
│  │ • Transactions             │      │ • Clickstream data         │    │
│  │ • Demographics             │      │ • Social media sentiment   │    │
│  │ • Service usage            │      │ • Real-time events         │    │
│  └─────────────┬──────────────┘      └──────────┬─────────────────┘    │
│                │                                 │                       │
└────────────────┼─────────────────────────────────┼───────────────────────┘
                 │                                 │
┌────────────────┼─────────────────────────────────┼───────────────────────┐
│                ↓         PROCESSING LAYER        ↓                       │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                           │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │                    Pandas + NumPy                                 │  │
│  │                 (Data Processing & ETL)                           │  │
│  │                                                                    │  │
│  │  • Data cleaning & validation                                     │  │
│  │  • Feature engineering (RFM, engagement scores)                   │  │
│  │  • Missing value handling                                         │  │
│  │  • Data transformation & normalization                            │  │
│  │  • Time-series aggregation                                        │  │
│  └────────────────────────────┬─────────────────────────────────────┘  │
│                                │                                         │
└────────────────────────────────┼─────────────────────────────────────────┘
                                 │
┌────────────────────────────────┼─────────────────────────────────────────┐
│                                ↓     ML MODELS LAYER                     │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                           │
│  ┌───────────────────────────────────────────────────────────────────┐ │
│  │            Machine Learning Pipeline (Scikit-learn)               │ │
│  ├───────────────────────────────────────────────────────────────────┤ │
│  │                                                                     │ │
│  │  ┌─────────────┐    ┌─────────────┐    ┌──────────────────┐     │ │
│  │  │   XGBoost   │    │     SVM     │    │Logistic Regression│     │ │
│  │  │             │    │             │    │                   │     │ │
│  │  │ • Gradient  │    │ • RBF Kernel│    │ • L2 Regularization│    │ │
│  │  │   Boosting  │    │ • SMOTE     │    │ • Class Weights   │     │ │
│  │  │ • Class     │    │ • Threshold │    │ • Feature         │     │ │
│  │  │   Weights   │    │   Tuning    │    │   Selection       │     │ │
│  │  └──────┬──────┘    └──────┬──────┘    └─────────┬────────┘     │ │
│  │         │                   │                     │               │ │
│  │         └───────────────────┴─────────────────────┘               │ │
│  │                             │                                     │ │
│  │                    ┌────────▼────────┐                           │ │
│  │                    │  Model Ensemble │                           │ │
│  │                    │  & Predictions  │                           │ │
│  │                    └────────┬────────┘                           │ │
│  └─────────────────────────────┼───────────────────────────────────┘ │
│                                │                                       │
└────────────────────────────────┼───────────────────────────────────────┘
                                 │
┌────────────────────────────────┼───────────────────────────────────────┐
│                                ↓   APPLICATION LAYER                   │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                           │
│  ┌────────────────────┐                  ┌────────────────────────┐    │
│  │     FastAPI        │                  │      Streamlit         │    │
│  │  (REST API)        │                  │    (Dashboard)         │    │
│  │                    │                  │                        │    │
│  │ • /predict         │                  │ • Real-time metrics    │    │
│  │ • /batch-predict   │◀────────────────▶│ • Interactive plots    │    │
│  │ • /model-metrics   │                  │ • Customer insights    │    │
│  │ • /health          │                  │ • Model comparison     │    │
│  └────────────────────┘                  └────────────────────────┘    │
│           │                                          │                  │
│           ↓                                          ↓                  │
│  ┌─────────────────────────────────────────────────────────────────┐  │
│  │                    End Users / Stakeholders                      │  │
│  │  • Marketing Teams  • Customer Service  • Executives  • Analysts│  │
│  └─────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
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

### Step 3: Set Up Infrastructure with Docker Compose

#### 3.1 Create Docker Compose Configuration

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

### Step 4: Clone Repository and Install Python Dependencies

#### 4.1 Clone Repository
```bash
# Clone the Keystone Data Solutions project
cd ~
git clone https://github.com/keystone-data-solutions/churn-prediction-platform.git
cd churn-prediction-platform
```
### 4.2.1 Instead of cloning, you can manually create directories
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

#### 4.2 Create Virtual Environment
```bash
# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate

# Upgrade pip
pip install --upgrade pip
```

#### 4.3 Install Python Packages
```bash
# Install all dependencies
pip install -r requirements.txt
```

**requirements.txt:**
```
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

### Step 5: Initialize Databases

#### 5.1 PostgreSQL Setup
```bash
# Connect to PostgreSQL
docker exec -it postgres-churn psql -U keystonedata -d churn_db

# Create tables (SQL)
CREATE TABLE customers (
    customer_id VARCHAR(50) PRIMARY KEY,
    gender VARCHAR(10),
    senior_citizen INTEGER,
    partner VARCHAR(5),
    dependents VARCHAR(5),
    tenure INTEGER,
    phone_service VARCHAR(5),
    paperless_billing VARCHAR(5),
    monthly_charges DECIMAL(10,2),
    total_charges DECIMAL(10,2),
    churn VARCHAR(5),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

# Exit
\q
```

#### 5.2 Cassandra Setup
```bash
# Connect to Cassandra
docker exec -it cassandra-churn cqlsh

# Create keyspace
CREATE KEYSPACE IF NOT EXISTS churn_events
WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};

# Create table
CREATE TABLE churn_events.customer_events (
    customer_id text,
    event_timestamp timestamp,
    event_type text,
    event_data map<text, text>,
    PRIMARY KEY (customer_id, event_timestamp)
) WITH CLUSTERING ORDER BY (event_timestamp DESC);

# Exit
exit
```

---

### Step 6: Verify Installation
```bash
# Check Python packages
pip list

# Test database connections
python3 -c "import psycopg2; print('PostgreSQL connector OK')"
python3 -c "from cassandra.cluster import Cluster; print('Cassandra connector OK')"
python3 -c "from kafka import KafkaProducer; print('Kafka connector OK')"

# Test ML libraries
python3 -c "import xgboost; print('XGBoost OK')"
python3 -c "from sklearn.svm import SVC; print('Scikit-learn OK')"

# Start Jupyter Notebook
jupyter notebook
```

---

## Running the Project

### Option 1: Using Jupyter Notebooks (Recommended for Development)
```bash
# Navigate to project directory
cd ~/churn-prediction-platform

# Activate virtual environment
source venv/bin/activate

# Start Jupyter Notebook
jupyter notebook

# Open notebooks in order:
# 1. notebooks/01_data_analysis.ipynb
# 2. notebooks/02_model_training.ipynb
```

---

### Option 2: Running FastAPI Backend
```bash
# Navigate to src directory
cd ~/churn-prediction-platform

# Activate virtual environment
source venv/bin/activate

# Run FastAPI server
uvicorn src.api.main:app --reload --host 0.0.0.0 --port 8000

# API will be available at:
# - API: http://localhost:8000
# - Docs: http://localhost:8000/docs
# - ReDoc: http://localhost:8000/redoc
```

**Example FastAPI endpoints:**
```
GET  /health              - Health check
POST /predict             - Single prediction
POST /batch-predict       - Batch predictions
GET  /model-metrics       - Model performance metrics
GET  /feature-importance  - Feature importance scores
```

---

### Option 3: Running Streamlit Dashboard
```bash
# Navigate to project directory
cd ~/churn-prediction-platform

# Activate virtual environment
source venv/bin/activate

# Run Streamlit dashboard
streamlit run src/dashboard/app.py

# Dashboard will open automatically at:
# http://localhost:8501
```

**Dashboard Features:**
- Real-time churn predictions
- Customer risk segmentation
- Model performance metrics
- Interactive visualizations
- Feature importance analysis
- Historical trends

---

### Option 4: Running Complete Pipeline
```bash
# Run end-to-end pipeline
python3 src/pipeline.py

# Or run specific components
python3 src/data/ingest_data.py
python3 src/preprocessing/clean_data.py
python3 src/models/train_models.py
python3 src/models/evaluate_models.py
```

---

## Project Structure
```
churn-prediction-platform/
├── README.md
├── requirements.txt
├── docker-compose.yml
├── .env
├── .gitignore
│
├── data/
│   ├── raw/                    # Original CSV/JSON files
│   ├── processed/              # Cleaned data
│   └── predictions/            # Model outputs
│
├── notebooks/
│   ├── 01_data_analysis.ipynb
│   └── 02_model_training.ipynb
│
├── src/
│   ├── __init__.py
│   │
│   ├── data/
│   │   ├── __init__.py
│   │   ├── ingest_postgres.py
│   │   ├── ingest_cassandra.py
│   │   └── kafka_producer.py
│   │
│   ├── preprocessing/
│   │   ├── __init__.py
│   │   ├── clean_data.py
│   │   └── feature_engineering.py
│   │
│   ├── models/
│   │   ├── __init__.py
│   │   ├── train_xgboost.py
│   │   ├── train_svm.py
│   │   ├── train_logistic.py
│   │   └── evaluate.py
│   │
│   ├── api/
│   │   ├── __init__.py
│   │   ├── main.py              # FastAPI app
│   │   ├── routes.py
│   │   ├── schemas.py
│   │   └── models.py
│   │
│   ├── dashboard/
│   │   ├── __init__.py
│   │   └── app.py               # Streamlit dashboard
│   │
│   ├── utils/
│   │   ├── __init__.py
│   │   ├── database.py
│   │   ├── logger.py
│   │   └── config.py
│   │
│   └── pipeline.py              # End-to-end pipeline
│
├── models/
│   ├── xgboost_model.pkl
│   ├── svm_model.pkl
│   ├── logistic_model.pkl
│   └── scaler.pkl
│
├── config/
│   ├── database.yaml
│   └── model_config.yaml
│
├── tests/
│   ├── test_data.py
│   ├── test_models.py
│   └── test_api.py
│
└── docs/
    ├── architecture.md
    ├── api_documentation.md
    └── business_requirements.md
```

---

## Model Performance

### Comparison of ML Models

| Model | Accuracy | Precision | Recall | F1-Score | ROC-AUC | Training Time |
|-------|----------|-----------|--------|----------|---------|---------------|
| **XGBoost** | 0.82 | 0.68 | 0.75 | 0.71 | 0.87 | 15s |
| SVM (SMOTE) | 0.79 | 0.56 | 0.73 | 0.63 | 0.84 | 45s |
| Logistic Reg | 0.81 | 0.65 | 0.72 | 0.68 | 0.85 | 8s |

**✅ Keystone Data Solutions Recommendation**: **XGBoost** for production deployment

### Key Features for Churn Prediction
1. **Tenure** (months with company)
2. **Monthly Charges**
3. **Contract Type**
4. **Total Charges**
5. **Payment Method**

---

## API Usage Examples

### Using cURL
```bash
# Health check
curl http://localhost:8000/health

# Single prediction
curl -X POST http://localhost:8000/predict \
  -H "Content-Type: application/json" \
  -d '{
    "tenure": 12,
    "monthly_charges": 70.5,
    "total_charges": 846.0,
    "contract_type": "Month-to-month",
    "payment_method": "Electronic check"
  }'

# Batch prediction
curl -X POST http://localhost:8000/batch-predict \
  -H "Content-Type: application/json" \
  -d '{
    "customers": [
      {"tenure": 12, "monthly_charges": 70.5, ...},
      {"tenure": 24, "monthly_charges": 85.0, ...}
    ]
  }'
```

### Using Python
```python
import requests

# API endpoint
url = "http://localhost:8000/predict"

# Customer data
customer = {
    "tenure": 12,
    "monthly_charges": 70.5,
    "total_charges": 846.0,
    "contract_type": "Month-to-month",
    "payment_method": "Electronic check"
}

# Make prediction
response = requests.post(url, json=customer)
result = response.json()

print(f"Churn Probability: {result['churn_probability']:.2%}")
print(f"Risk Level: {result['risk_level']}")
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
