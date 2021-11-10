## Project Documentation

1. Discuss the purpose of this database in the context of the startup, Sparkify, and their analytical goals.
    - The NoSQL keyspace *sparktifydb* is constructed where the following three requests from Sparktify can be easily accessed:
      1. Give the artist, song title, and song's length in the music app history that was heard during sessionId = 338, and itemInSession = 4
      2. Give only the name of the artist, song (sorted by itemInSession), and user (first and last name) for userid = 10, sessionid = 182
      3. Give every user name (first and last) in my music app history who listened to the song 'All Hands Against His Own'


2. State and justify your data model schema design and ETL pipeline.
    - By having exact specifications of what the end-users want, a NoSQL data model supplies them answers without needing to aggregate, filter, or join.
    - A couple of benefits of this data model are having fast reads and writes (high throughput) and high availability.


### Jupyter notebook, NoSQL_Apache_Cassandra_ETL, process
  - Execute ETL pipeline for pre-processing files
  - Create an Apache Cassandra cluster
  - Create a NoSQL keyspace *sparktifydb*
  - Create tables and queries to align with each request stated above.
  - Verify the results align with the expectation
  - Drop the tables
  - Close the session and cluster connection
