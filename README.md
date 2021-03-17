# Refugees in the Age of Gloabl Warming
### Data Engineering Capstone Project

#### Scope and goal of the project
 - The data utilizied in this project focuses on monitoring refugee and population information based on temperature changes over time. There are four source datasets:
     1. City_temperature.csv - average daily temperature for all major cities in the world from 1995 - 2020
     2. Country_population_total_long.csv - annual population counts by country from 1960 - 2017
     3. UNdata_City_Population_20210315.csv - annual population counts by city from 1970 - 2020 (contains gaps in 1970's)
     4. UNdata_Refugees_20210217.csv - annual refugee counts from 1975 - 2016 by country of residence and country of origin
 - The request is to build a data warehouse for analytical processes, so analysts can design recurring and ad hoc SQL reports over time. There is a strong emphasis in ensuring the warehouse is easy to interpret, performant, and quality assured.

#### Execution steps

#### Data model selection
 - To meet the needs of the request, the data warehouse will reflect a relational data model, particulary a snowflake schema. The relational model complements the need for analytical processes, especially expected changes in business requirements over time. Utilizing a dimensional model gives end users an intuitive layout, the flexibilty to use SQL, and high data integrity.

#### How would Spark or Airflow be incorporated?

#### Rationale for the choice of tools and technologies
 - Although relational data models are know for slower performance due to ACID transactions, they can become more efficient when built in a cloud-based data warehouse. Given this, the data warehouse is built in Amazon's Redshift, which provides scalability and elascticiy where one can scale nodes according to their perfomance needs. Mention something about the selected distribution style. 

#### Recommended usage - how often the data should be ran and why?
 - Look up frequency of daily temperature refresh. Easy assumption may by daily, but likely less frequently such as monthly, quaterly, or annually. Outside of temperature changes being captured daily, all other information is aggregated by year. So the frequency should reflect the needs for montioring average termperatures.

#### Recommended adjustments given the following scenarios:
 - If the data was increased by 100x
     - Consider features of Apache Spark, data lakes, or different distribution styles
 - If the pipelines were run on a daily basis by 7 am
     - Consider utilizing Airflow's scheduler and montiroing capabilities, a distributed system, and/or different partition approaches
 - If the database needed to be accessed by 100+ people
     - NoSQL data model (low latency and high avilability)
