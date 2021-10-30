# Big Data
- Storing, processing huge volumes of data w/ goal of generating insights
- Processed in rel time (streaming) or batches
- Traditional DBMS like Relational are not designed to handle such large volumes

## 5 V's of Big Data
1. **Volume**: Size of data needing to be ingested
    - At Terabytes (right after GB) and Petabytes (1024 TB), we are concerned about big data as traditional systems can't store this
    - Exabytes (1024 PB) of data are stored by Google
2. **Velocity**: Speed at which data enters solution
    - Higher speed --> lesser time to analyze
3. **Variety**: Data coming in from multiple sources, should be handled by solution
    - *Structured data*: Tabular (Excel, CSV)
    - *Semistructured data*: JSON, XML
    - *Unstructured data*: Text file, audio, etc.
4. **Veracity**: Degree to which data are accurate and trusted
5. **Value (most important)**: Data are useless unless they add value to organization or process

## Distributed Storage
Example: 1 TB --> [200 GB, 200 GB, 200 GB, 200 GB, 200 GB]
- Necessitates distributed computing: pulling everything from multiple machines into a single machine

# Hadoop
- Used for **distributed storage** and **distributed computing**
- Scale up from single servers to thousands of machines, each offering local computation and storage
    1. HDFS - Hadoop Distributed File System (**distributed storage**)
        - distributes large dataset among multiple machines with each machine having its own CPU and RAM
        - block size of 128 MB and each block is replicated across 3 given machines in the cluster --> Hadoop has **fault tolerance**, in case of hardware failure in other machines. Higher replication factor leads to higher fault tolerance but also more required storage space 
    2. MapReduce - Parallel processing (**distributed computing**)
- Multiple machines (**data nodes**) are assembled to form a **cluster** of computers
- Cluster is under **name node**, which is central authority in Hadoop framework (**Master-Slave architecture**)
- Google Cloud Dataproc: fully managed services for Apache Hadoop and Spark
    - **$300 free credit after signing up**
    - You pay for the time you use the cluster with *per-second billing*
    - Cost for a 3 node cluster with 4 CPUs and 16 GB RAM is $0.12 per hour or $2.88 per day

## Storing Files in HDFS
- SSH into master machine on Google Cloud Data proc. Below commands will exit the local environment and go into HDFS
    - `hadoop fs -ls` to list all files in Hadoop file system
    - `hadoop fs -mkdir ...` similarly, to make a directory
- To remove a file from HDFS and put it in local environment: `hadoop fs -get data.csv .`

## MapReduce and YARN
- Parallel processing on distributed computers - way of sending computational tasks to HDFS
- Split input dataset into independent chunks which are processed by the **map tasks** completely in parallel
- Sorts outputs of maps, which are input to **reduce tasks**
- Input and output both stored in HDFS
- Resource manager has job tracker, node managers have task trackers
    - Client sends request to resource manager, resource manager sends request to task trackers
    - Task tracker reads key-value pairs, stores output in disk
    - **map** tasks: read each row and fetch an element
    - **reduce** tasks: aggregation, may not always be needed
- YARN = Yet Another Resource Navigator: relieve MapReduce by *taking over responsibility of Resource Management and Job Scheduling*
    - Hadoop can now run non-MapReduce jobs (such as spark) within its own framework
    
**Overall**:
- Hadoop: Distributes large datasets using HDFS and performs computational tasks in parallel using MapReduce
- Resource management and job scheduling handled by YARN

## Hive
- What is Hive? Hive is a large-scale SQL-like query tool to process data stored in HDFS. MapReduce isn't directly used anymore, but Hive is written on top of it.
    - Hive can only process **structured data**.
    - Hive is not a database. It points to the data in HDFS
- Stores metadata information of data stored in HDFS, in a relational database called **Hive metastore**, which is outside HDFS
    - HiveQL: Hive query language (close to SQL queries)
- When specifying a location of the table, it automatically reads all CSV files in the specified directory
- External table: storage not managed by Hive
    - When dropping table, metadata dropped from Hive metastore, but data still intact in HDFS
- Filter, aggregate, transform, then store in another Hive table --> BI Tool/Reporting
### HiveQL Queries
- `SHOW databases;`
- `CREATE DATABASE IF NOT EXISTS camilledunning;`
- `USE camilledunning;`
- `SHOW tables;`
- `CREATE TABLE retailcust (age INT, salary FLOAT, gender STRING, country STRING, purchased STRING) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LOCATION '/user/camilledunning/data' TBLPROPERTIES ("skip.header.line.count="="1");`

To run a HiveQL query from a file:
- `hive -f file.sql`

# Spark
- Most popular technology for large-scale data processing
    - Can be used with HDFS, S3/Glue, Azure, file system, relational DB and NoSQL
- MapReduce stores intermediate data to disk <-- Spark does in-memory processing with this data
- **Batch** and **streaming** data
- YARN started to give Hadoop ability to run **non-MapReduce** jobs such as Spark
    - Alternative to Hive + MapReduce: Spark computational tasks instead of Map and Reduce tasks
- Data processing on multiple **Spark nodes**
- Spark has its own cluster manager, or Mesos cluster manager
- Popular **Spark-Hadoop architecture**: Hadoop solves distributed storage, Spark does large-scale processing in a distributed manner
- Written in Scala, supports Scala, Python, and Java
    - For ML, Python + PySpark is preferred

## Spark Framework
- **Storage**: HDFS, Local, NoSQL
- **Management**: YARN, Mesos, Spark Scheduler
- **Engine**: Spark Core
- **Library**: Spark SQL, Spark MLLib, GraphX, Streaming

## Driver Program
- Runs on master node
- Creates **Spark session** --> several workers on **worker nodes**