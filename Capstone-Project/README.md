# Refugees in the Age of Global Warming
### Data Engineering Capstone Project

#### Project Summary
This project focuses on monitoring refugee and population information around the world based on temperature changes over time.

The project follows the following steps:
* Step 1: Scope the Project and Gather Data
* Step 2: Explore, Assess, and Clean the Data
* Step 3: Standardize City, Country, and State Names Across Data Sources
* Step 4: Define the Data Model
* Step 5: Create Table Schemas Based on Conceptual Model
* Step 6: Build the ETL Pipeline
* Step 7: Complete Project Write Up

There are four source datasets:
 1. City_temperature.csv
     - Summary: the average daily temperature for all major cities in the world from 1995 - 2020
     - Source: the University of Dayton - separate txt files available for each city [here](https://academic.udayton.edu/kissock/http/Weather/default.htm). The data is available for research and non-commercial purposes only. Refer to [this page](https://academic.udayton.edu/kissock/http/Weather/default.htm) for license.
     - Secondary source: SRK via Kaggle - [link](https://www.kaggle.com/sudalairajkumar/daily-temperature-of-major-cities)
 2. Country_population_total_long.csv
     - Summary: annual population counts by country from 1960 - 2017
     - Source: The World Bank - [link](https://data.worldbank.org/indicator/SP.POP.TOTL)
     - Secondary source: Devakumar kp via Kaggle - [link](https://www.kaggle.com/imdevskp/world-population-19602018?select=population_total_long.csv)
 3. UNdata_City_Population_20210315.csv
     - Summary: annual population counts by city from 1970 - 2020
     - Source: UN Data - [link](https://data.un.org/Data.aspx?d=POP&f=tableCode%3A240)
 4. UNdata_Refugees_20210217.csv
     - Summary: annual refugee counts from 1975 - 2016 by country of residence and country of origin
     - Source: UN Data - [link](http://data.un.org/Data.aspx?d=UNHCR&f=indID%3aType-Ref)
     - Footnote 1: Persons recognized as refugees under the 1951 UN Convention/1967 Protocol, the 1969 OAU Convention, in accordance with the UNHCR Statute, persons granted a complementary form of protection and those granted temporary protection. In the absence of Government estimates, UNHCR has estimated the refugee population in 24 industrialized countries based on 10 years of individual refugee recognition.
     - Footnote 2: The category of people in refugee-like situations is descriptive in nature and includes groups of persons who are outside their country or territory of origin and who face protection risks similar to those of refugees, but for whom refugee status has, for practical or other reasons, not been ascertained.

The request is to build a data warehouse for analytical processes, so analysts can design recurring and ad hoc SQL reports over time. There is a strong emphasis on ensuring the warehouse is easy to interpret, performant, and quality assured.

#### Execution steps
1. Install packages referenced at the top of Capstone-Project.ipynb
2. Read in the data and execute step 2 to familiarize with the data sets and clean the data as needed
3. Execute steps up to 3.3 to standardize city, country, and state names across data sources
4. Create a redshift cluster with an attached IAM role that has S3 read access
5. Update dwh.cfg to reflect the parameters of your cluster, IAM role, and access key info
6. Execute section 3.4, which creates an S3 bucket and loads the staging tables
7. Run step 5 which creates table schemas based on the conceptual model
8. Run step 6 to build the ETL pipeline, run data quality checks, and create a data dictionary
9. Delete your Redshift cluster when finished!

#### Data model selection
The data warehouse will reflect a relational data model with a star schema. The relational model complements the use case of analytical processes, especially with expected changes in business requirements over time. Utilizing a dimensional model gives end users an intuitive layout, the flexibility to use SQL, and high data integrity.
 
Fact Tables
 1. **Temperature_Fact** - records from the temperature dataset associated with daily average temperatures in cities around the world
     - Temperature_Key, Avg_Temperature, Date_Key, Country_Key, City_Key
 2. **Country_Population_Fact** - records from the country population dataset reflecting population counts by year and country
     - Country_Population_Key, Country_Population,  Country_Key
 3. **City_Population_Fact** - records from the city population dataset reflecting population counts by year and city
     - City_Population_Key, City_Population, Source_Key, City_Key,  Country_Key
 4. **Refugee_Fact** - records from the refugee dataset associated with refugee and refugee-like populations by country and year
     - Refugee_Key,  Refugee_Population, Refugees_Assisted_By_UNHCR, Refugee_Like_Population, Refugee_Like_Population_Assisted_By_UNHCR, Asylum_Country_Key, Origin_Country_Key

Dimension Tables
 1. **Date_Dim** - dates of temperature recordings
     - Date_Key, Date, Month, Day, Year
 2. **Country_Dim** - country, year, and region content in climate refugee database
     - Country_Key, Country, Region, Year
 3. **City_Dim** - city, year, and state content in climate refugee database
     - City_Key, City, City_Type, State, Year
 4. **Source_Dim** - source details in climate refugee database
     - Source_Key, Source_Year, City_Population_Notes, Record_Type, Reliability

#### How would Spark or Airflow be incorporated?
If Airflow were added in, we would establish an automation system for scheduled refreshes and a simplistic pipeline monitoring design.

Spark would become beneficial as more data becomes available and the data warehouse grows. The opportunity to use a data lake increases scalability, and there is more flexibility to integrate all types of data.

#### Rationale for the choice of tools and technologies
Although relational data models are known for slower performance due to ACID transactions, they can become more efficient when built in a cloud-based data warehouse. Given this, the data warehouse is built in Amazon's Redshift, which provides scalability and elasticity where one can scale nodes according to their performance needs.

#### Recommended usage - how often the data should run and why?
Outside of temperature changes being captured daily, all other information is aggregated by year. Due to the large fluctuation in granularity, refreshes should occur monthly or quarterly. Until other sources can be captured more frequently, the refresh timeline should reflect the need for monitoring average temperatures.

#### Recommended adjustments given the following scenarios:
 - If the data was increased by 100x - make use of Apache Spark and data lakes, which allow for increased scalability. Also, different distribution styles may need to be considered to minimize skewness and shuffling.
 - If the pipelines were run daily by 7 am - utilize Airflow's scheduler and monitoring capabilities, and add a distributed system where data can be partitioned for more timely results.
 - If the database needed to be accessed by 100+ people - consider converting to a NoSQL data model, which offers low latency and high availability.
