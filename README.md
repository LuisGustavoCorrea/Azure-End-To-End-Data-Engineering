  # Azure End To End Data LakeHouse
![Azure data Engineer Project](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/9bc9ec59-6105-4713-bcdf-fbe7363ebb57)

## Introduction
The goal of this project is to implement a Data LakeHouse in Azure cloud, using the innovative Medallion architecture.

Here's a summary of the data pipeline stages described:

- Stage 1 - Extracting data from the local SQL Server using Data Factory and ingesting it into Data Lake Gen 2 in Parquet format in the Bronze layer.

- Stage 2 - Utilizing Data Bricks to read and clean the data from Data Lake, formatting columns and types. After processing, the cleaned and transformed data is stored in the Silver layer.

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

## Data Factory

### Ingestion

Using Data Factory, I parameterized a Lookup activity to execute a query in local database metadata and retrieve tables with the SalesLT schema. 

Using this information listed within a Data Factory variable, we utilized it as a parameter for the next step: For Each.

Within this loop, we read each database table using the list as a parameter and loaded the data into the Bronze layer, organized into folders where each table corresponds to a separate folder.

### LookUp Configuration
![image](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/d7bbf0cb-8f91-4214-91ff-ffab87628410)

### ForEach Configuration
![image](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/92c86ea7-b300-4df7-9ad3-832ccc083552)

### Data in Layer Bronze 
![Files Parquet Bronze](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/6426fa02-ccb1-48ac-9dbf-89c7a9e36609)

![data factory flow](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/560b9ef0-2a86-46c1-9326-6681b2d3301d)
![Data Lake gen 2](https://github.com/LuisGustavoCorrea/Azure-End-To-End-Data-Engineering/assets/18196788/affe9536-a634-4b11-9779-3ad431b348d0)

