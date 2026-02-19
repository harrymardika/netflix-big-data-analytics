# 🎬 Netflix Prize Data Warehouse & Recommendation System

> A complete end-to-end data engineering and machine learning project for building a Netflix-scale recommendation system. From raw rating data to intelligent predictions.

---

## 📋 Table of Contents

- [🎬 Netflix Prize Data Warehouse \& Recommendation System](#-netflix-prize-data-warehouse--recommendation-system)
  - [📋 Table of Contents](#-table-of-contents)
  - [🎯 Project Overview](#-project-overview)
    - [Key Metrics](#key-metrics)
  - [📁 Repository Structure](#-repository-structure)
  - [🚀 Quick Start](#-quick-start)
    - [Prerequisites](#prerequisites)
    - [Setup (5 minutes)](#setup-5-minutes)
    - [Next Steps](#next-steps)
  - [🏗️ System Architecture](#️-system-architecture)
  - [🔄 Data Flow](#-data-flow)
  - [📦 Submodules](#-submodules)
    - [Data Ingestion Pipeline](#data-ingestion-pipeline)
    - [Recommendation Modelling](#recommendation-modelling)
  - [🛠️ Tech Stack](#️-tech-stack)
    - [Data Engineering](#data-engineering)
    - [Machine Learning](#machine-learning)
    - [Development \& Deployment](#development--deployment)
  - [📥 Installation](#-installation)
    - [Option 1: Quick Setup with Docker (Recommended)](#option-1-quick-setup-with-docker-recommended)
    - [Option 2: Manual PostgreSQL Setup](#option-2-manual-postgresql-setup)
    - [Python Environment](#python-environment)
  - [💻 Usage](#-usage)
    - [Running the ETL Pipeline](#running-the-etl-pipeline)
    - [Running Recommendation Models](#running-recommendation-models)
    - [Database Queries](#database-queries)
  - [🗄️ Database Schema](#️-database-schema)
  - [✨ Key Features](#-key-features)
    - [Data Ingestion](#data-ingestion)
    - [Recommendation System](#recommendation-system)
  - [🔀 Project Workflow](#-project-workflow)

---

## 🎯 Project Overview

This repository contains a production-grade, end-to-end pipeline for analyzing and modeling the **Netflix Prize dataset** - a collection of **100M+ movie ratings** from **480K+ customers** across **17K+ movie titles** spanning from October 1998 to December 2005.

The project is divided into two complementary submodules working in tandem:

1. **Data Ingestion**: Extract, transform, and load Netflix Prize data into a dimensional data warehouse using Apache Spark
2. **Recommendation Modelling**: Build collaborative filtering recommendation models using the processed data

### Key Metrics

| Metric                   | Value                                    |
| ------------------------ | ---------------------------------------- |
| **Total Ratings**        | 100M+ records                            |
| **Unique Customers**     | 480K+                                    |
| **Unique Movies**        | 17K+ titles                              |
| **Date Range**           | Oct 1998 - Dec 2005                      |
| **Processing Framework** | Apache Spark 3.5+                        |
| **Database**             | PostgreSQL 12+                           |
| **Models**               | SVD, ALS (Spark MLlib)                   |
| **Graph Analytics**      | Community detection, centrality analysis |

---

## 📁 Repository Structure

```
netflix/
├── README.md                           # This file - Project overview
├── data-ingestion/                     # ETL Pipeline (Submodule 1)
│   ├── README.md                       # Detailed ETL documentation
│   ├── etl_pipeline_spark.py          # Main Spark ETL pipeline
│   ├── schema.sql                      # PostgreSQL database schema
│   ├── docker-compose.yml              # Local PostgreSQL setup
│   ├── Dockerfile                      # Spark container image
│   ├── pyproject.toml                  # Project metadata
│   ├── requirements.txt                # Python dependencies
│   ├── data/                           # Raw dataset files
│   ├── checkpoints/                    # Resumable pipeline checkpoints
│   ├── logs/                           # Pipeline execution logs
│   └── temp_csv/ & temp_parquet/       # Intermediate data storage
│
└── modelling/                          # Recommendation Models (Submodule 2)
    ├── README.md                       # Detailed modelling documentation
    ├── model.ipynb                     # Jupyter notebook with full analysis
    ├── requirements.txt                # Python dependencies
    ├── about_dataset.txt               # Dataset information
    ├── assignment.txt                  # Project assignment details
    └── schemas.txt                     # Database schema reference
```

---

## 🚀 Quick Start

### Prerequisites

- Python 3.11+
- PostgreSQL 12+ (or use Docker)
- Apache Spark 3.5+
- Git

### Setup (5 minutes)

```bash
# Clone the repository
git clone <repository-url>
cd netflix

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # Linux/macOS
# or: .\venv\Scripts\Activate.ps1  # Windows

# Install data-ingestion dependencies
cd data-ingestion
pip install -r requirements.txt

# Set up PostgreSQL (Option A: Docker)
docker-compose up -d

# Create database schema
psql -h localhost -U postgres -f schema.sql
# Enter password when prompted

# Configure environment
cp .env.example .env
# Edit .env with your database credentials

# Run ETL pipeline
python etl_pipeline_spark.py
```

### Next Steps

After the ETL completes:

```bash
# Navigate to modelling submodule
cd ../modelling
pip install -r requirements.txt

# Open and run the Jupyter notebook
jupyter notebook model.ipynb
```

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    NETFLIX PRIZE DATASET                         │
│  (Raw Files: movie_titles.csv, probe.txt, qualifying.txt, etc)  │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         ▼
        ┌────────────────────────────────────┐
        │   DATA INGESTION PIPELINE          │
        │   (Apache Spark + PySpark)         │
        │                                    │
        │  • Extract raw rating data         │
        │  • Transform to Star Schema        │
        │  • Load into PostgreSQL            │
        │  • Resumable checkpoints           │
        └────────────┬───────────────────────┘
                     │
                     ▼
        ┌────────────────────────────────────┐
        │   DIMENSIONAL DATA WAREHOUSE       │
        │   (PostgreSQL)                     │
        │                                    │
        │  • fact_ratings                    │
        │  • dim_customer                    │
        │  • dim_movie                       │
        │  • dim_date                        │
        └────────────┬───────────────────────┘
                     │
                     ▼
        ┌────────────────────────────────────┐
        │   RECOMMENDATION MODELLING         │
        │   (Scikit-Learn + Spark MLlib)     │
        │                                    │
        │  • Collaborative Filtering         │
        │  • Matrix Factorization (SVD)      │
        │  • ALS (Alternating Least Squares) │
        │  • Graph Analytics                 │
        │  • Performance Metrics             │
        └────────────────────────────────────┘
```

---

## 🔄 Data Flow

```
Raw Ratings Data
    ↓
[Data Validation] → Remove nulls/duplicates
    ↓
[Spark Transformation] → Normalize, aggregate, compute features
    ↓
[Star Schema Mapping] → Create fact/dimension tables
    ↓
[PostgreSQL Load] → Insert into data warehouse
    ↓
[Checkpoint System] → Save progress for resumability
    ↓
[Data Ready for Modeling]
    ↓
[Feature Engineering] → User/movie bias, normalized ratings
    ↓
[Model Training] → SVD, ALS hyperparameter tuning
    ↓
[Model Evaluation] → MAE, RMSE, MAPE metrics
    ↓
[Recommendations Generated]
```

---

## 📦 Submodules

### Data Ingestion Pipeline

**Location**: [data-ingestion/](data-ingestion/)

A production-grade ETL pipeline that processes 100M+ Netflix ratings into a normalized data warehouse.

**Key Features**:

- ✅ Resumable processing with automatic checkpoints
- ✅ Duplicate-safe data insertion
- ✅ Star Schema dimensional modeling
- ✅ Apache Spark for distributed processing
- ✅ Real-time progress tracking
- ✅ Docker support for quick setup

**Main Components**:

- `etl_pipeline_spark.py`: Core ETL orchestration
- `schema.sql`: PostgreSQL dimensional schema
- `docker-compose.yml`: Local development environment

For detailed documentation, see [data-ingestion/README.md](data-ingestion/README.md)

### Recommendation Modelling

**Location**: [modelling/](modelling/)

Machine learning models and graph analytics for Netflix-scale recommendation systems.

**Key Features**:

- ✅ Collaborative filtering implementation
- ✅ Matrix factorization (SVD)
- ✅ Spark MLlib ALS for scalability
- ✅ Hyperparameter tuning with GridSearch
- ✅ Comprehensive evaluation metrics
- ✅ Graph analytics & community detection

**Main Components**:

- `model.ipynb`: Complete Jupyter notebook with all analyses
- Feature engineering and data quality checks
- Model performance comparison
- Network visualization and community detection

For detailed documentation, see [modelling/README.md](modelling/README.md)

---

## 🛠️ Tech Stack

### Data Engineering

- **Apache Spark 3.5+** - Distributed data processing
- **PySpark** - Spark Python API
- **PostgreSQL 12+** - Data warehouse
- **SQLAlchemy** - ORM and SQL toolkit
- **Pandas** - Data manipulation

### Machine Learning

- **Scikit-Learn** - Traditional ML algorithms (SVD)
- **Apache Spark MLlib** - Distributed ML (ALS)
- **NetworkX** - Graph analytics
- **Jupyter** - Interactive notebooks

### Development & Deployment

- **Docker** - Containerization
- **Python-dotenv** - Environment configuration
- **Git** - Version control

---

## 📥 Installation

### Option 1: Quick Setup with Docker (Recommended)

```bash
# Navigate to data-ingestion
cd data-ingestion

# Start PostgreSQL
docker-compose up -d

# The database is ready on localhost:5432
```

### Option 2: Manual PostgreSQL Setup

```bash
# Install PostgreSQL (macOS with Homebrew)
brew install postgresql

# Start services
brew services start postgresql

# Create database
createdb netflix_warehouse

# Load schema
psql netflix_warehouse -f schema.sql
```

### Python Environment

```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate

# Install all dependencies
pip install -r data-ingestion/requirements.txt
pip install -r modelling/requirements.txt
```

---

## 💻 Usage

### Running the ETL Pipeline

```bash
cd data-ingestion

# First run
python etl_pipeline_spark.py

# Resume after interruption (automatic)
python etl_pipeline_spark.py

# Start fresh (delete checkpoint)
rm etl_checkpoint.json
python etl_pipeline_spark.py
```

### Running Recommendation Models

```bash
cd modelling

# Start Jupyter server
jupyter notebook

# Open model.ipynb and run cells in sequence
```

### Database Queries

```bash
# Connect to database
psql -h localhost -U postgres -d netflix_warehouse

# Example queries
SELECT COUNT(*) FROM fact_ratings;
SELECT COUNT(DISTINCT customer_id) FROM dim_customer;
SELECT COUNT(DISTINCT movie_id) FROM dim_movie;
```

---

## 🗄️ Database Schema

The data warehouse follows a **Star Schema** design optimized for analytical queries:

```sql
-- Fact Table
fact_ratings (
  rating_key INT PRIMARY KEY,
  customer_key INT (FK),
  movie_key INT (FK),
  date_key INT (FK),
  rating INT,
  ...
)

-- Dimensions
dim_customer (customer_key, customer_id, ...)
dim_movie (movie_key, movie_id, title, year, ...)
dim_date (date_key, date, year, month, day, ...)
```

See [data-ingestion/schema.sql](data-ingestion/schema.sql) for complete schema definition.

---

## ✨ Key Features

### Data Ingestion

- **📊 Distributed Processing**: Apache Spark handles 100M+ records efficiently
- **🔄 Resumable**: Automatic checkpoint system prevents data loss
- **🛡️ Data Quality**: Validation, duplicate detection, null handling
- **📈 Scalable**: Star Schema design supports analytical queries
- **🐳 Container Ready**: Docker setup for reproducibility

### Recommendation System

- **🤖 Multiple Algorithms**: SVD (local), ALS (distributed)
- **🔧 Hyperparameter Tuning**: GridSearch for optimal models
- **📉 Comprehensive Metrics**: MAE, RMSE, MAPE, MSE evaluation
- **🕸️ Graph Analytics**: Network analysis, community detection
- **📊 Visual Analysis**: Performance charts and network visualizations

---

## 🔀 Project Workflow

```
Phase 1: Data Ingestion
├── Extract raw Netflix Prize data files
├── Validate and clean data
├── Transform to dimensional model
├── Load into PostgreSQL
└── Create checkpoint for resumability

Phase 2: Data Exploration
├── Query data warehouse
├── Compute user/movie statistics
├── Analyze rating distributions
└── Validate data quality

Phase 3: Feature Engineering
├── Calculate user bias (avg rating)
├── Calculate movie bias (avg rating)
├── Normalize features for models
└── Create train/test splits

Phase 4: Model Training
├── Implement SVD (Scikit-Learn)
├── Implement ALS (Spark MLlib)
├── Hyperparameter tuning
├── Cross-validation
└── Save trained models

Phase 5: Evaluation & Analysis
├── Calculate performance metrics
├── Compare model performance
├── Generate recommendations
├── Graph analytics
└── Visualize results
```
