# Supply Chain Demand Forecaster with Vendor Sync

## Overview

This project is a **Machine Learning-powered supply chain forecasting system** that predicts demand and provides actionable insights for inventory management, pricing strategy, and vendor synchronization. It integrates **AWS (S3, Bedrock), ML models (ARIMA, XGBoost), FAISS for vector search, and a user query system** to generate dynamic forecasts and recommendations.

## User/Supplier manager expectations

- **Demand Forecasting**: Predicts demand for a future date based on historical trends.
- **Stockout Risk Assessment**: Estimates stockout probability using real-time inventory and lead time.
- **Price Sensitivity Analysis**: Evaluates how demand fluctuates with price changes.
- **Vendor Synchronization**: Suggests optimal reorder dates and quantities to prevent stockouts.

## Architecture

1. **Data Pipeline & Storage**
   - Supply chain data (sales, inventory, lead times) stored in **AWS S3**.
   - **ETL pipeline** extracts, transforms, and loads data into a structured format.
2. **Feature Engineering & Model Training**
   - Creates features such as **lagged sales, moving averages, and lead time-adjusted demand**.
   - Trains **ARIMA** (time-series) and **XGBoost** (machine learning) models.
   - Stores predicted demand and features as embeddings for fast retrieval.
3. **User Query System & Predictions**
   - Users input a **future date** and select a prediction type.
   - Retrieves necessary values from FAISS (if available) or calculates using ML models.
   - Generates insights and recommendations dynamically.

## Dataset

The dataset should contain the following key columns:

- **Date**: Timestamp of sales or inventory data.
- **Sales Volume**: Number of units sold per day.
- **Inventory Level**: Available stock at the warehouse.
- **Lead Time**: Time taken for replenishment (days).
- **Price**: Unit price of the product.
- **Vendor Details**: Supplier information and delivery schedules.

## Machine Learning Models

- **ARIMA (AutoRegressive Integrated Moving Average)**: Used for time-series forecasting.
- **XGBoost (Extreme Gradient Boosting)**: Captures complex demand patterns using tabular features.

## User Query Options

| Query Type | Input Parameters | Calculation Method |
| --- | --- | --- |
| **Demand Forecasting** | Future Date | ML Model Prediction |
| **Stockout Risk** | Future Date, Inventory, Lead Time | `StockoutRisk = (Inventory + SafetyStock - PredictedDemand × LeadTime) / (PredictedDemand × LeadTime)` |
| **Price Sensitivity** | Future Date, % Price Change | `ΔDemand = ElasticityCoefficient × %ΔPrice` |
| **Vendor Sync** | Future Date | Predicts optimal reorder date |

## Workflow

1. **Data Ingestion & Storage**
   - Raw data is stored in **S3** and processed via an **ETL pipeline**.
2. **Feature Engineering**
   - Computes relevant features such as **moving averages, elasticity coefficients, and lag variables**.
3. **Model Training & Predictions**
   - Trains **ARIMA** and **XGBoost** models to predict demand for the next week.
4. **Embedding Storage & Retrieval**
   - Converts **features and demand predictions** into **embeddings** stored in **FAISS**.
5. **User Query Processing**
   - When a query is received, retrieves **related values from embeddings or ML models** and performs calculations.
6. **Response Generation**
   - Outputs formatted insights based on predictions and real-time conditions.

## Deployment

- **Infrastructure**: Deployed on **AWS using Terraform** to set up VPC, S3, IAM roles, and Bedrock.
- **Security**: Uses **encrypted S3 storage** and **least-privilege IAM policies**.
- **Model Hosting**: Runs ML models in **AWS Lambda or SageMaker**.

## Example User Queries & Responses

1. **Predict Demand for a Future Date**
   - **Query**: "What is the expected demand for March 10, 2025?"
   - **Response**: "Estimated demand: 5,800 units."
2. **Estimate Stockout Risk**
   - **Query**: "What happens if stock is low on March 12?"
   - **Response**: "Stockout risk increases by 30%, consider replenishing."
3. **Analyze Price Sensitivity**
   - **Query**: "If we increase price by 10%, how will demand change?"
   - **Response**: "Demand is expected to drop by 5%."
4. **Vendor Synchronization**
   - **Query**: "When should we reorder for March 15?"
   - **Response**: "Order 6,000 units from Vendor X by March 8 to avoid delays."

## Future Enhancements

- **Expand Dataset**: Integrate real-time IoT sensor data for warehouse monitoring.
- **Advanced Forecasting**: Implement LSTMs or Transformer-based models.
- **Automated Decision-Making**: AI-driven inventory restocking based on market trends.

## Conclusion

This project **demonstrates a scalable, AI-driven approach** to supply chain optimization, making it a valuable tool for logistics firms, e-commerce platforms, and manufacturing companies.
