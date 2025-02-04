!Alt text
![image](https://github.com/user-attachments/assets/fe2664c0-3786-458b-a262-9b467f52c0ec)


# End-to-End Data Pipeline Project

## Project Overview

This project demonstrates an end-to-end data pipeline solution, showcasing the integration of various Azure services to migrate data from an on-premise SQL Server database to the cloud. The pipeline starts with data ingestion using Azure Data Factory (ADF), followed by data transformation using Azure Databricks, and finally loading the transformed data into Azure Synapse Analytics. The data is then visualized using Microsoft Power BI. Additionally, Microsoft Entra ID (formerly Azure Active Directory) and Azure Key Vault are used for security and governance.

## Architecture Summary

1. **Data Migration and Ingestion**: 
   - **Source**: On-premise SQL Server database.
   - **Tool**: Azure Data Factory (ADF).
   - **Destination**: Azure Data Lake Storage Gen2 (Bronze Layer).

2. **Data Transformation**:
   - **Tool**: Azure Databricks.
   - **Process**: Transform raw data from the Bronze Layer to the Silver Layer (simple transformations like renaming columns and changing data types) and then to the Gold Layer (data cleaning).

3. **Data Storage**:
   - **Tool**: Azure Synapse Analytics.
   - **Process**: Load transformed data from the Gold Layer into Azure Synapse Analytics for further analysis.

4. **Data Visualization**:
   - **Tool**: Microsoft Power BI.
   - **Process**: Create interactive dashboards and reports using data from Azure Synapse Analytics.

5. **Security and Governance**:
   - **Tools**: Microsoft Entra ID and Azure Key Vault.
   - **Purpose**: Manage access and store sensitive information securely.

## Detailed Workflow

### 1. Data Migration and Ingestion

- **Azure Data Factory (ADF)**:
  - **Linked Service**: Create a linked service in ADF to connect to the on-premise SQL Server database.
  - **Pipeline**: Develop a pipeline in ADF to copy all tables from the SQL Server database to Azure Data Lake Storage Gen2. The data is stored in the Bronze Layer, which serves as the raw data storage.

### 2. Data Transformation

- **Azure Databricks**:
  - **Workspace Setup**: Set up an Azure Databricks workspace.
  - **Bronze to Silver Transformation**:
    - Connect to the Bronze Layer in Azure Data Lake Storage Gen2.
    - Perform initial transformations such as renaming columns and changing data types.
    - Load the transformed data into the Silver Layer.
  - **Silver to Gold Transformation**:
    - Further transform the data in the Silver Layer, focusing on data cleaning and enrichment.
    - Load the cleaned and enriched data into the Gold Layer.

### 3. Data Storage

- **Azure Synapse Analytics**:
  - **Workspace Setup**: Set up an Azure Synapse Analytics workspace.
  - **Table Creation**: Create the necessary tables in Azure Synapse Analytics.
  - **Data Loading**: Load the transformed data from the Gold Layer into Azure Synapse Analytics for further analysis and querying.

### 4. Data Visualization

- **Microsoft Power BI**:
  - **Connection**: Connect Power BI to Azure Synapse Analytics.
  - **Report Creation**: Develop interactive dashboards and reports using the data from Azure Synapse Analytics. These reports can include various visualizations such as charts, graphs, and tables to provide insights into the data.

### 5. Security and Governance

- **Microsoft Entra ID (Azure Active Directory)**:
  - **Access Management**: Use Microsoft Entra ID to manage access to the various Azure services and ensure that only authorized users can access the data and resources.
- **Azure Key Vault**:
  - **Secret Management**: Store sensitive information such as database connection strings and passwords securely in Azure Key Vault.

### 6. Automation

- **Pipeline Automation**:
  - **Triggering**: Set up triggers in ADF to automate the pipeline. For example, configure the pipeline to trigger whenever a new row is added to the on-premise SQL Server database.
  - **End-to-End Process**: Ensure that the entire process, from data ingestion to visualization, is automated. This includes copying data to the Bronze Layer, transforming it through the Silver and Gold Layers, loading it into Azure Synapse Analytics, and updating the Power BI reports.

## Getting Started

### Prerequisites

- Azure Subscription
- On-premise SQL Server database
- Azure Data Factory
- Azure Data Lake Storage Gen2
- Azure Databricks
- Azure Synapse Analytics
- Microsoft Power BI
- Microsoft Entra ID
- Azure Key Vault

### Setup Instructions

1. **Azure Data Factory**:
   - Create a linked service to connect to the on-premise SQL Server database.
   - Create a pipeline to copy data from SQL Server to Azure Data Lake Storage Gen2 (Bronze Layer).

2. **Azure Databricks**:
   - Set up a Databricks workspace.
   - Create notebooks to transform data from the Bronze Layer to the Silver and Gold Layers.

3. **Azure Synapse Analytics**:
   - Set up a Synapse workspace.
   - Create tables and load data from the Gold Layer.

4. **Microsoft Power BI**:
   - Connect Power BI to Azure Synapse Analytics.
   - Create and publish reports.

5. **Security and Governance**:
   - Configure Microsoft Entra ID for access management.
   - Use Azure Key Vault to store sensitive information.

## Conclusion

This project showcases a comprehensive data pipeline solution using Azure services, from data migration and ingestion to visualization. It highlights the integration of various tools to achieve a seamless and automated data processing workflow.
