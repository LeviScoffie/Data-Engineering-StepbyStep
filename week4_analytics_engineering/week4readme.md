# ANALYTICS ENGINEERING

Developments in the data domain like -- cloud data warehousses such as big query, snowflak or redshift have helped lower the cost of storage adn computing.

Stitch- Data_Pipelines_as_a_service SIMPLY ETL process
Looker- SQL-first 
MOde- Self service analytics
Data Governance 

Data Engineer- Prepares and maintain infrastructure that data team needs.

Analytics Engineering - Introduces the good software engineering practices to the efforts of data analysts and data scientists.


## ANALYTICS ENGINEERING TOOLING
Data Loading= Stitch
Data Storing= BigQuery,Snowflaake,Redshift
Data Modelling= dbt, Dataform
Data Presentation= Looker,Mode, Tableau



### DATA MODELLING CONCEPTS
#### ETL vs ELT
Extract from sources, transform using DAGs, Load into BQ datawarehouse- this is ETL

Extract from sources, Load into data warehouse, Transaform while in BQ- this is ELT


ETL- more stable and compliant data analysis but higher storage and compute costs while ELT is fast data analysis , lower cost and maintenance.


#### Kimball's Dimensional Modeling

Objective- deliver data understandable to the business users. Fast query perfomance.
Approach- Prioritise user comprehension and query perfomance over non redundant data (3NF)


##### Elements of Dimensional Modeling
* Fact tables -- verbs: orders, sales etc
* Dimension tables -- nouns: customers, productts



Staging Area(raw data) -> Processing (make data models) -> Presentation (Exposure to business stakeholders)


What is dbt?
 Its the data build tool, helps with the transformation of the data. Involves knowing SQL and other software engineering best practices like CI/CD and documentation.

 Procedure : Develop models > Test and Document > Deployment (Version control and CI/CD)


 How does DBT work?
 Using a .sql file we add a modelling layer where we are going to be transforming the data, and the model with tha transformee data is persisted back to the data warehouse, there are no DDL or DML. 


 How to use dbt?
 * *dbt Core* is the open source project that allows the data transformation.
    - It is going to build and run the dbt project(.sql, .yml files)
    - SQL compilation logic, macros(fxns) and database adapters.
    - CLI for dbt commands (E.G dbt init, dbt compile, dbt deps etc)

 * *dbt Cloud* web application  where we can manage and develop our dbt project
    - Web-based IDE to develop, run and test a dbt project
    - Jobs orchestration
    - Logging and Alerting
    - Integrated documentation 

BigQuery- use the cloud IDE (no local installation)
Postgres- Using Local IDE of your choice (we need to install dbt core locally connectinf to Postgres database), run dbt models thru CLI

## STARTING A dbt PROJECT
dbt provides a starter project with all the basic folders and files

We will need dbt_project.yml. 
This file helps define the global details such as the name , 
profile- used to configure which database its going to be using.

In a given folder, every model that is there is going to be a view or its going to be a materialized table unless  otherwise.

USING THE DATASET THAT WE CREATED IN WEEK 3:trips_data_all


Create a new dataset and name it how u want- this will be the dataset where we are going to create the models while we are deceloping them- like sandbox.


Create another one (CALLED *production*) where we are going to be running the models after deploynent



### Development of dbt models

In the sql files where we are going to write sql statements to transform our data we are also going to need in jinja.

Normally between curly brackets. Within the brackets u can use fxns alias **macros** in jinj that are going to help us genereate when full compiled sql code.


First, we are going to use the _config_ macro. It helps add the ddl or dml to the model that we are writing eg.  one param is **materialized** - this is the materialization strategy.the stratgies are : table, view, incremental, epheremal.

`Table` is going to drop the table if it already exists in our data warehouse and its going to create the table in the schema that we are working with.

`View` kind of similar to table; where it creates or alters view

`Incremental` which is essentially a table; allows us to run our model incrementally. Used for data that doesnt change every day. After running our model, it transforms and inserts in our table only the latest data.

`Ephemeral` similar to having a cte separated in another file tat we could emebed its a derived model.



##### From clause of a dbt model

**Sources**
- The data loaded to our dwh that we use as sources for our models.
- Configuration defined in the yml files in the models folder.
- Used with the source macro that will resolve the name to the right schema, plus buils the dependecies automatically
- Source freshness can be defined and tested.

**Seeds**
- CSV files stored in our repo under the seed folder
- Benefits of version controlling
- Equivalent to a copy command
- Recommended for data that doesn't change frequently
- Runs with `dbt seed -s file name`


**Ref** 
- Macro to reference the underlying tables and views that were building the data warehouse.
- Run the same code in any environment, it will resoolve the correct schema for you .
- Dependencies are built automatically.

When tables/views are created from dbt models, we can call them by using the macro ref. Onlyone name added because the ref is going to resolve that for us.


In the sources.yml file we define the sources and the freshness checks.
E.g to each source in the tabke, we could define a field that is being used to load that data from sources. So for example, the data is being loaded every hour; so then a freshness thsheold of 3 hours.


**Macros** 
 These are similar to functiions in python

 - Use the control structures; (e.g if statements and for loops) in SQL
 - However, this is not going to return a result but a code. e.g we have 2 params 'a' and 'b' and want to concat them; normall the reuslt would be 'ab' or sth but becasue of the macro, it would return the code to concat both params.
 - Help us to mainatain smae type of transformation inside our models and then we only wtite it ones jus like fxns in python