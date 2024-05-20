<p align="center">
    <a href="utilities/imgs/aaie.png">
        <img src="utilities/imgs/aaie.png" alt="aaie image" width="auto" height="150">
    </a>
</p>
<p align="center">
    <a href="https://sites.google.com/corp/google.com/genai-solutions/home?authuser=0">
        <img src="utilities/imgs/opendataqna.png" alt="logo" width="400" height="auto">
    </a>
</p>
<h1 align="center">Open Data QnA - Chat with your SQL Database</h1> 

Overview
-------------
The **Open Data QnA** python library enables you to chat with your databases by leveraging LLM Agents on Google Cloud.

Open Data QnA enables a conversational approach to interacting with your data. Ask questions about your PostgreSQL or BigQuery databases in natural language and receive informative responses, without needing to write SQL. Open Data QnA leverages Large Language Models (LLMs) to bridge the gap between human language and database queries, streamlining data analysis and decision-making.

**Key Features:**

* **Conversational Querying:** Ask questions naturally, without requiring SQL knowledge.
* **SQL Generation:** Automatically generates SQL queries based on your questions.
* **Query Refinement:** Validates and debugs queries to ensure accuracy.
* **Natural Language Responses:** DRun queries and present results in clear, easy-to-understand language.
* **Visualizations (Optional):** Explore data visually with generated charts.
* **Extensible:** Customize and integrate with your existing workflows(API, UI, Notebooks).


It is built on a modular design and currently supports the following components: 

### Database Connectors
* **Google Cloud SQL for PostgreSQL**
* **Google BigQuery**

### Vector Stores 
* **PGVector on Google Cloud SQL for PostgreSQL**
* **BigQuery Vector Store**
* **ChromaDB (WIP)**

### Agents 
* **BuildSQLAgent:** An agent specialized in generating SQL queries for BigQuery or PostgreSQL databases. It analyzes user questions, available table schemas, and column descriptions to construct syntactically and semantically correct SQL queries, adapting its process based on the target database type.
* **ValidateSQLAgent:** An agent that validates the syntax and semantic correctness of SQL queries. It uses a language model to analyze queries against a database schema and returns a JSON response indicating validity and potential errors.
* **DebugSQLAgent:** An agent designed to debug and refine SQL queries for BigQuery or PostgreSQL databases. It interacts with a chat-based language model to iteratively troubleshoot queries, using error messages to generate alternative, correct queries.
* **DescriptionAgent:** An agent specialized in generating descriptions for database tables and columns. It leverages a large language model to create concise and informative descriptions that aid in understanding data structures and facilitate SQL query generation.
* **EmbedderAgent:** An agent specialized in generating text embeddings using Large Language Models (LLMs). It supports direct interaction with Vertex AI's TextEmbeddingModel or uses LangChain's VertexAIEmbeddings for a simplified interface.
* **ResponseAgent:** An agent that generates natural language responses to user questions based on SQL query results. It acts as a data assistant, interpreting SQL results and transforming them into user-friendly answers using a language model.
* **VisualizeAgent:** An agent that generates JavaScript code for Google Charts based on user questions and SQL results. It suggests suitable chart types and constructs the JavaScript code to create visualizations of the data.

**Note:** the library was formerly named Talk2Data. You may still find artifacts with the old naming in this repository. 

Architecture
-------------
<p align="center">
    <a href="utilities/imgs/Open Data QnA Solution Architecture.png">
        <img src="utilities/imgs/OpenDataQnA Solution Architecture - v1.png" alt="aaie image">
    </a>
</p>


Architecture Summary
-------------
Open Data QnA operates in a sequence of well-defined steps, orchestrating various agents to process user queries and generate informative responses:

* **Vector Store Creation:** The vector store is initialized, storing embeddings of known good SQL queries, table schemas, and column details. This serves as a knowledge base for retrieval-augmented generation (RAG).

* **RAG (Retrieval-Augmented Generation):** User queries are embedded and compared to the vector store to retrieve relevant context (table/column details and similar past queries) for improved query generation.

* **SQL Generation (BuildSQLAgent):**  The BuildSQLAgent leverages the retrieved context and the user's natural language question to generate an initial SQL query.

* **Optional Validation (ValidateSQLAgent):** If enabled, the ValidateSQLAgent assesses the generated SQL for syntactic and semantic correctness.

* **Optional Debugging (DebugSQLAgent):** If the initial SQL is invalid and debugging is enabled, the DebugSQLAgent iteratively refines the query based on error feedback.

* **SQL Execution (Dry Run/Explain):** The refined SQL query is tested with a dry run (BigQuery) or explain plan (PostgreSQL) to estimate resource usage and identify potential errors.

* **SQL Execution (Full Run):** If the query is deemed valid, it's executed against the database to fetch the results.

* **Response Generation (ResponseAgent):** The ResponseAgent analyzes the SQL results and the user's question to generate a natural language response, providing a clear and concise answer.

* **Optional Visualization (VisualizeAgent):** If enabled, the VisualizeAgent suggests suitable chart types and generates JavaScript code for Google Charts to display the SQL results in a visually appealing manner.


**Key Points:**

* **Modularity:** Each step is handled by a specialized agent, allowing for flexibility and customization.
* **RAG Enhancement:** The use of retrieval-augmented generation leverages existing knowledge for better query formulation.
* **Validation and Debugging:** Optional agents enhance the reliability and accuracy of generated queries.
* **Informative Responses:** The ResponseAgent aims to provide meaningful and contextually relevant answers.
* **Visual Appeal:** The optional visualization adds an interactive layer to the user experience.



Solution Overview
-------------
<p align="center">
    <a href="utilities/imgs/Open Data QnA Solution Overview.png">
        <img src="utilities/imgs/OpenDataQnA-SolutionOverview.png" alt="aaie image">
    </a>
</p>





Getting Started: Quick Start   
-------------

**Quickstart with Open Data QnA: [Standalone BigQuery Notebook](/notebooks/(standalone)Run_OpenDataQnA.ipynb)**

This notebook offers a streamlined way to experience the core functionality of Open Data QnA using BigQuery as both the data source and vector store. While it doesn't encompass the full flexibility of the repository setup, it's a perfect starting point to quickly test and explore the conversational querying capabilities of Open Data QnA with your own BigQuery datasets.


Getting Started: Main Repository 
-------------
    
### Clone the repository and switch to the correct branch 
   
    git clone -b opendataqna git@github.com:GoogleCloudPlatform/applied-ai-engineering-samples.git
    cd applied-ai-engineering-samples

You can setup this solution with two approaches. Choose one based on your requirement
  - Using Jupyter Notebooks (For better view at what is happening at each stage of the solution)
  - Using CLI (For ease of use and running with simple python command without the need to understand every step of the solution)

## Jupyter Notebook Based Approach

### 1. Run the [1_Setup_OpenDataQnA](/notebooks/1_Setup_OpenDataQnA.ipynb)

**Notebook Summary**

This notebook guides you through the setup and execution of the Open Data QnA application. It provides comprehensive instructions for configuring your environment, preparing the vector store.

### 2. Run the [2_Run_OpenDataQnA](/notebooks/2_Run_OpenDataQnA.ipynb)

**Notebook Summary**

This notebook guides you by reading the configuration you setup with [1_Setup_OpenDataQnA](/1_Setup_OpenDataQnA) and running the pipeline to answer questions about your data.

### 3. [Loading Known Good SQL Examples](/notebooks/3_LoadKnownGoodSQL.ipynb)
   
   In case you want to separately load Known Good SQLs please run this notebook once the config variables are setup in config.ini file. This can be run multiple times just to load the known good sql queries and create embeddings for it.

## Command Line Interface (CLI) Based Approach

### 1. Add Configuration values for the solution in [config.ini](/config.ini)

For setup we require details for vector store, source etc. Edit the [config.ini](/config.ini) add values for the parameters based of below information

This section assumes that a datasource is already set up in your GCP project. If a datasource has not been set up, use the notebooks below to copy a public data set from BigQuery to Cloud SQL or BigQuery on your GCP project


Enabled Data Sources:
* PostgreSQL on Google Cloud SQL (Copy Sample Data: [0_CopyDataToCloudSqlPG.ipynb](0_CopyDataToCloudSqlPG.ipynb))
* BigQuery (Copy Sample Data: [0_CopyDataToBigQuery.ipynb](0_CopyDataToBigQuery.ipynb))

Enabled Vector Stores:
* pgvector on PostgreSQL 
* BigQuery vector


```
[CONFIG]
embedding_model = <LLM Model that should be used for created vector embeddings> # Options: 'vertex' or 'vertex-lang'
description_model = <Vertex AI model that will be used to create missing description for your tables and columns> #Options 'gemini-1.0-pro', 'gemini-1.5-pro', 'text-bison-32k'
data_source = 'bigquery' #  Options: 'bigquery' and 'cloudsql-pg' 
vector_store = 'bigquery-vector' # Options: 'bigquery-vector', 'cloudsql-pgvector'
logging = <Boolean value to either add logs or not. Currently logs into BigQuery Only> #Options True or False 
kgq_examples = <Boolean value to either tell the solution that we have Known Good SQLs or Not> #Options True or False 

[GCP]
project_id = <Your GCP Project ID>

[PGCLOUDSQL]
# If you want to use PG as source, fill out the values below
pg_region = <GCP valid region code>
pg_instance = <PostgreSQL Instance name you want to create/provide the existing one>
pg_database = <PostgreSQL Database name you want to create/provide the existing one>
pg_user = <PostgreSQL username that to be created for the solution to connect>
pg_password = <Password for the above user created>
pg_schema = <PostgreSQL Schema name on which you want to run the solution against>

[BIGQUERY]
# If you want to use BQ as source, fill out the values below
bq_dataset_region = <GCP valid region code>
bq_dataset_name = <BigQuery Dataset name that you want to run the solution against>

# Name for the BQ dataset created for bigquery-vector and/or logging
bq_opendataqna_dataset_name = 'opendataqna' #defaulted value so that the solution use the dataset for logs and vector store etc
bq_log_table_name = 'audit_log_table'  #defaulted value for log table name
bq_table_list = None # either None or a list of table names in format ['reviews', 'ratings']. This will be used to filter specific tables from BigQuery
```

Once filled the config.ini something like below

```
[CONFIG]
embedding_model = vertex
description_model = gemini-1.0-pro
data_source = bigquery
vector_store = bigquery-vector
debugging = yes
logging = yes
kgq_examples = no

[GCP]
project_id = three-p-o

[PGCLOUDSQL]
pg_region = us-central1
pg_instance = pg15-opendataqna
pg_database = opendataqna-db
pg_user = pguser
pg_password = pg123
pg_schema = pg-vector-store

[BIGQUERY]
bq_dataset_region = us-central1
bq_dataset_name = fda_food
bq_opendataqna_dataset_name = opendataqna
bq_log_table_name = audit_log_table
bq_table_list= None
```

### 2. Creating Virtual Environment and Install Dependencies

```
cd applied-ai-engineering-samples
git checkout opendataqna

pip install poetry --quiet
poetry lock
poetry install --quiet
poetry env info
poetry shell

```
Authenticate your credentials

```
gcloud auth login
gcloud auth application-default login

```
```
gcloud services enable \
    serviceusage.googleapis.com \
    cloudresourcemanager.googleapis.com --project <<Enter Project Id>>
```
```
gcloud auth application-default set-quota-project <<Enter Project Id for using resources>>

```
Enable APIs for the solution setup

```
gcloud services enable \
  cloudapis.googleapis.com \
  cloudbuild.googleapis.com \
  compute.googleapis.com \
  container.googleapis.com \
  containerregistry.googleapis.com \
  iam.googleapis.com \
  run.googleapis.com \
  sqladmin.googleapis.com \
  aiplatform.googleapis.com \
  artifactregistry.googleapis.com \
  bigquery.googleapis.com \
  firebase.googleapis.com \
  monitoring.googleapis.com \
  storage.googleapis.com \
  orgpolicy.googleapis.com --project <<Enter Project Id>>

```

### 3. Run [env_setup.py](/env_setup.py) to create vector store based on the configuration you did in Step-1

```
python env_setup.py
```

### 4. Run [opendataqna.py](/opendataqna.py) to run the pipeline you just setup

*user_question* : Enter your question in string
*user_database* : Enter the BQ_DATASET_NAME for BigQuery sources or PG_SCHEMA for PostgreSQL sources (refer your [config.ini](/config.ini) file)

```
python opendataqna.py --user_question "What are the top 5 cities with highest recalls?" --user_database "fda_food"
```


#### If you are looking to deploy backend apis for the solution and frontend UI refer to the README.md under [`/backend-apis`](/backend-apis/)




Documentation
-------------

* [Open Data QnA Source Code (GitHub)](<https://github.com/GoogleCloudPlatform/applied-ai-engineering-samples/tree/opendataqna>)
* [Open Data QnA usage notebooks](/notebooks)



Quotas and limits
------------------

[BigQuery quotas](<https://cloud.google.com/bigquery/quotas>) including hardware, software, and network components.

[Gemini quotas](<https://cloud.google.com/gemini/docs/quotas>).


License
-------

Open Data QnA is distributed with the [Apache-2.0 license](<LICENSE>).

It also contains code derived from the following third-party packages:

* [pandas](<https://pandas.pydata.org/>)
* [Python](<https://www.python.org/>)



Getting Help
----------

If you have any questions or if you found any problems with this repository, please report through GitHub issues.
