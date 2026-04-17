# BANA7075-Final
PMIS MVP: Predictive Maintenance Machine Learning Pipeline
Overview
This project implements an end-to-end Machine Learning pipeline designed to predict industrial machine failures within a 4-hour window. The system fetches validated sensor data, trains competing classification models (Random Forest and XGBoost), evaluates them using performance metrics, and registers the "Champion" model into a PostgreSQL database for production use.

Key Features
Automated Data Ingestion: Connects to a Supabase-hosted PostgreSQL instance to retrieve engineered features.

Multi-Model Training: Trains and tunes both RandomForestClassifier and XGBClassifier with class-weight balancing to handle imbalanced failure data.

Experiment Tracking: Uses MLflow to log parameters, metrics (Recall, Precision, F1, AUC), and model artifacts.

Model Registry: Automatically identifies the best-performing model based on F1-score and inserts the metadata into a model_registry table.

Tech Stack
Language: Python 3.12

ML Frameworks: Scikit-Learn, XGBoost

MLOps: MLflow

Database: SQLAlchemy, Psycopg2 (Supabase/PostgreSQL)

Environment: Google Colab / Jupyter Notebook

Getting Started
Prerequisites
You will need a PostgreSQL database (the code is configured for Supabase) with a table named public.validated_features.

Installation
Run the following to install necessary dependencies:

Bash
pip install pandas scikit-learn xgboost mlflow joblib psycopg2-binary sqlalchemy
Configuration
The pipeline uses Google Colab's userdata for secure credential management. Ensure you have the following secret set in your environment:

DB_PASS: Your database password.

Workflow Details
1. Data Retrieval
The pipeline executes a SQL query to fetch sensor metrics including:

Vibration & Temperature: Mean, Std Dev, Rate of Change (ROC), and Kurtosis.

Current & Acoustic: Mean, Std Dev, and ROC.

Oil Pressure: Mean, Std Dev, and ROC.

2. Feature Engineering
Categorical Encoding: machine_type, shift, and material are transformed using One-Hot Encoding.

Class Balancing: Uses class_weight='balanced' for Random Forest and scale_pos_weight for XGBoost to account for the rarity of failure events.

3. Model Competition
Models are compared based on a tiered sorting logic:

F1-Score (Primary metric for balanced precision/recall)

Recall (Secondary metric to ensure failures aren't missed)

AUC

4. Artifact Storage
The winning model and its specific feature list are serialized using joblib and stored in an /artifacts directory, while also being logged to MLflow.

Authors
Steven H. Jones Assistant Professor, University of Cincinnati

BANA 7075 - Final Project
