  # Azure End To End Data LakeHouse
![Azure data Engineer Project](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/9bc9ec59-6105-4713-bcdf-fbe7363ebb57)

## Introduction
The goal of this project is to implement a Data LakeHouse architecture in Azure cloud, using the innovative Medallion architecture.

Here's a summary of the data pipeline stages described:

- Stage 1 - Extracting data from the local SQL Server using Data Factory and ingesting it into Data Lake Gen 2 in Parquet format in the Bronze layer.

- Stage 2 - Utilizing DataBricks to read and clean the data from Data Lake, formatting columns and types. After processing, the cleaned and transformed data is stored in the Silver layer.

- Stage 3 - Silver to Gold transition involves making minor adjustments to the data, such as standardizing column names and performing business aggregations.

- Stage 4 - Views are created in Synapse to directly consume the processed data from the LakeHouse in the Gold layer, ensuring reliable data and high performance.

This structured approach ensures data integrity, efficient processing, and supports analytics and reporting needs effectively through the layers of Bronze, Silver, and Gold within the LakeHouse architecture.

## Concepts
![Concept Data LakeHouse](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/0069e7ff-f1d7-448b-a891-0f2f19d13248)

### Data Lakehouse is an innovative concept.

This concept combines the best features of a Data Lake and a Data Warehouse, integrating organization, flexibility, and performance into a single platform. 
Additionally, it provides governance and control over data and processes.

The issue that Lakehouse resolves: </b>
Imagine a company with a data lake receiving continuous data and multiple applications consuming these data streams, such as a Machine Learning application or a Data Warehouse.

During data ingestion or transformation, processing errors or data inconsistencies can occur.

In the traditional model, that inconsistent data transferred, as there is no transaction control.

With Lakehouse and the ACID concept, only committed and ready-to-use data. This ensures accuracy and reliability, transforming how your company manages and utilizes data.


### Medallion Architecture
Layered Data Processing: Organizes data processing into distinct Bronze, Silver, and Gold layers:

 - Bronze Layer: Stores raw data directly from sources while preserving its original integrity, serving as the initial landing zone for incoming data.
 - Silver Layer: Transforms and cleanses data to improve quality and standardization, preparing it for intermediate analytics and processing.
 - Gold Layer: Highly refines and optimizes data for fast, precise analytical queries, often involving aggregation and summarization for efficient querying and reporting.
![Concept Medalhao](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/e9cb737f-153c-4eb2-b827-a38262d239ab)

## Ok! Let's go to talk about the Project!

### Security

How to maintain security when connecting to the source and extracting data? It is crucial to securely manage user credentials to prevent sensitive information, such as connection details to your transactional server, from being exposed or unprotected.

Using Azure Key Vault, we register access credentials to the transactional server, such as username and password, only once and in a single location. 

This also simplifies its integration with other cloud services, crucially ensuring security, monitoring access, and safeguarding sensitive data centrally.
![Key Vault](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/c31a19da-0000-4bdd-be41-dc236bf944be)

### Data Source
Now, with the secure connection between the source and the cloud established, let's move on to the next step.


How to automate getting various tables and load this on Data Lake?

![image](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/be3a268e-6bc3-4c4f-afbe-fa20939b1cf9)

## Ingestion and Orchestration

### Data Factory

Using Data Factory, I parameterized a Lookup activity to execute a query in local database metadata and retrieve tables with the SalesLT schema. 

Using this information listed within a Data Factory variable, we utilized it as a parameter for the next step: For Each.

Within this loop, we read each database table using the list as a parameter and loaded the data into the Bronze layer, organized into folders where each table corresponds to a separate folder.

After that, I configured Data Factory to read and execute the Databricks notebooks in sequence, applying the logic for data transformation and transferring data between the layers.

### LookUp Configuration
![image](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/d7bbf0cb-8f91-4214-91ff-ffab87628410)

### ForEach Configuration
![image](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/92c86ea7-b300-4df7-9ad3-832ccc083552)

### Data in Layer Bronze 
![Files Parquet Bronze](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/6426fa02-ccb1-48ac-9dbf-89c7a9e36609)

## Transformation 

### Databricks

Here, with this excellent tool, we see the data lakehouse in action through data reading and storage on top of the Delta framework, which is composed of JSON and Parquet files, ensuring ACID transactions and various features such as Time Travel. 

Time Travel allows you to view and recover previous versions of tables and query changes at the row level.

In this part, some data transformations and typecasting are performed. Refer to the file in the Databricks folder to view the complete code.

![image](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/42067c62-1c95-47e6-a31b-f12c30281776)

## Load

After the data is processed in each layer, the treated data is saved in their respective folders in Data Lake Gen 2.

Check the codes and transformations performed in the notebooks inside the Databricks folder.

![Data Lake gen 2](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/affe9536-a634-4b11-9779-3ad431b348d0)

## Synapse Analytics

Within Azure Synapse Analytics, I created a SQL Serverless instance to be used for data consumption in the Gold layer.


How do we consume the data?


Views were created from the final tables in the Gold layer, which are in Delta format.

![Azure Synapse](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/4942ad0e-7d27-462d-a6eb-6aaf8bb02334)

### Why?

Data Governance: Views ensure that data is governed in a centralized and controlled manner.


 - Single Point of Consumption: All data is consumed from a single location, avoiding redundancies and inconsistencies.


- Storage Efficiency: Using views allows for more efficient storage, as there is no need to duplicate data for other applications.


- Flexibility and Cost Savings: Utilizing views provides flexibility in data access and cost savings, as there is no need to replicate data for different applications to consume.

The main benefit is that using views allows for flexibility and cost savings while maintaining data efficiency and integrity.


Now, the data is ready to be consumed by BI applications, data analyst teams, data scientist teams, and users for ad hoc queries.

## Conclusion
If you've read this far, thank you very much! This project allowed me to understand how the Data Lakehouse architecture works in practice, enabling me to apply it in different scenarios.
