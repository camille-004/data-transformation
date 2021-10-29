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
