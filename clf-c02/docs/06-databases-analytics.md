Databases & Analytics
=====================

Storing data on disk (e.g., EFS, EBS, EC2 instance store or S3) have their limits. Sometimes you will need to have a structure in the data to build **indexes** and enable **queries** or **search** through the data. For this use case, it is better store the data in a database. Here you can define relationships between your datasets.

Database are optimized for a purpose and come with different feature, shapes and constraints.

You will find to types of databases:

- SQL (i.e., Structured Query Language) databases.
- No-SQL databases.

SQL databases (a.k.a relational databases) looks like excel spreadsheets with links between them. They have a key feature and is that you can use the SQL language to preform queries or lookups over the database. The next image is an example of a relational database:

![SQL Database](../assets/images/06A-sql-databases.png)

No-SQL databases (a.k.a non-relational databases) have a purpose build specific data models with flexible schemas for building modern applications. An schema is the shape of the data. They offer benefits like flexibility, scalability, high-performance and highly functional. As example we have a JSON model as shown below:

![No-SQL Database](../assets/images/06B-no-sql-database.png)

They can be key-value pairs, documents, graphs, in-memory or search databases.

AWS offers use to manage different databases with the next benefits:

- Quick provisioning.
- High availability.
- Vertical and horizontal scaling
- Automated backup and restore.
- Operations and upgrades
- Operating system patching
- Monitoring and alerting.

> Note: many database technologies could run on EC2, but you must handle yourself the resiliency, backup, patching, availability, and other features of your database.

Amazon RDS
----------

RDS stands for Relational Database Service. So this is a managed database service for databases that use SQL as query language. It supports: Postgres, MySQL, MariaDB, Oracle, Microsoft SQL Server, IBM DB2 and Aurora. Aurora is proprietary of AWS and we will review more about it later. So, with this service we can:

- Automated provisioning.
- OS patching.
- Continuos backups and restore to specific timestamp (i.e., point in time restore).
- Monitoring dashboards.
- Read replicas for improved read performance.
- Multi AZ setup for disaster recovery.
- Maintenance windows for upgrades.
- Scaling capability.
- Storage backed by EBS.

> Note: You cannot use SSH (Secure Shell Protocol) into your RDS instances.

The next image summarizes the RDS solution architecture:

![RDS Solution Architecture](../assets/images/06C-rds-architecture.png)

Now, let's talk about **Aurora**, that is a proprietary not open sourced technology from AWS that only supports Postgres and MySQL for offer a AWS cloud optimized service and claims 5x performance improvement over MySQL on RDS and 3x of performance on RDS. Aurora is not in the free tier and storage automatically grows in increment of 10GB, up to 128TB. It cost 20% more than RDS, but it is mor efficient.

There is also **Aurora Serverless** that is and automated database instantiation and auto-scaling based on actual usage. No capacity planning is needed and it is least management overhead. You can pay per second to be more cost effective.

The use cases are: good for infrequent, intermittent or unpredictable workloads

RDS Deployment
--------------

We have three types of deployments in RDS:

1. Read replicas.
2. Multi-AZ.
3. Multi-Region.

**Read replicas** help us to scale the read workload of the database. You can create up to 15 read replicas of the main RDS. Keep in mind that the data is only written to the main database. The next image illustrates the distribution of a read replicas deployment:

![Read Replicas](../assets/images/06D-read-replicas.png)

**Multi-AZ** is used to _failover- in case of availability zone outage, guarantee high availability in the database. The data is only read/written to the main database and you can only have 1 other AZ as failover. Below an example of a multi-AZ deployment:

![Multi-AZ](../assets/images/06E-multi-az.png)

Finally, we have the **multi-region** deployment. His use case is for _disaster recovery_ in case of a region issue. Due to his distribution you will have _local performance_ for global reads, but, you will pay with replication cost. The following image show a map of a multi-region deployment.

![Multi-Region](../assets/images/06F-multi-region.png)

ElastiCache
-----------

ElastiCache is the option to handle _in-memory databases_. In the same way RDS is to get managed relational database, ElastiCache is to get manage Redis or Memcached. These caches are in-memory databases with high performance and low latency to reduce load off databases for read intensive workloads.

Similarly, to RDS with ElastiCache AWS takes care of OS maintenance / patching, optimizations, setup, configuration, monitoring, failure recovery and backups.

Below and image of an architecture with a cache databes:

![Cache Architecture](../assets/images/06G-cache-arch.png)

DynamoDB
--------

DynamoDB is a fully managed highly available NoSQL database with replication across 3 AZ. It scales to massive workloads, distributed _serverless_ database, processing millions of requests per second, trillions of row around 100 second of TB storage. Guaranties fast and consistent performance with a single-digit millisecond latency (i.e., low latency retrieval). Also it is integrated with IAM for security, authorization and administration.

Below is an image with the type of data that handle a DynamoDB:

![Dynamo Type of Data](../assets/images/06H-dynamo-type-of-data.png)

Additionally, you can add the **DynamoDB Accelerator (a.k.a. DAX)**, that is a fully managed in-memory cache for DynamoDB, offering **10x performance improvement**, converting a single-digit millisecond latency to microseconds latency when accessing your DynamoDB tables. This offers a secure and highly scalable/available database. The difference with ElastiCache at cloud computing platform level is that DAX is only used for and is integrated with DynamoDB, while ElastiCache can be used for other databases.

The next image is an architecture that use DAX in the middle of the application and a DynamoDB.

![DAX](../assets/images/06I-dax.png)

Finally, lets review the **DynamoDB global tables**. The purpose of a global table is make it accessible with **low latency** in multiple regions. They use the **active-active** replications that implies the the read and write operations could be done in any AWS region. The next images is an example of a global table that is replicate in N. Virginia and Pairs:

![Global Tables](../assets/images/06J-global-tables.png)

Redshift
--------

Redshift is based on PostgreSQL used for **online analytical processing (OLAP in short)** for execute analytics and data warehousing. It loads data once every hour and have 10x better performance than other data warehouses scaling to petabytes of data. Use columnar storage of data instead of row based offering massively parallel query execution (MPP in short) and highly availability. Pay as you go based on the instances provisioned and has a SQL interface for perming the queries. Lastly, business intelligence tools such AWS Quicksight or Tableu integrate with it.

Complementary you can setup automatically provisions and scales data warehouse unrlying capacity with **Redshift Serverless**. Moreover, you can run analytics workloads without managing the data warehouse infrastructure, paying only for what you use.

The common use cases of Redshift Serverless are: reporting, manage dashboard applications, and realtime analytics.

Below it is a diagram with the sequence to setup Redshift Serverless:

![Redshift Serverless](../assets/images/06K-redshift-serverless.png)

Amazon Elastic MapReduce
------------------------

EMR stands for Elastic MapReduce, and it helps creating **Hadoop Clusters** to analyze and process vast amount of data. Keep in mind the Apache Hadoop is a collection of open-source software utilities that facilitates using a network of many computers to solve problems involving massive amounts of data and computation.

The clusters can be made of hundreds of EC2 instances, and also supports Apache Spark, HBase, Presto and Flink. EMR takes care of all the provisioning and configuration in AWS offering auto-scaling and integrations with Spot instances.

The use cases are: data processing, machine learning, web indexing and big data.

Amazon Athena
-------------

Athena is a serverless query service to perform analytics against S3 objects via SQL language to query the files. It supports CSVm JSON, ORC, AVRO and Parquet,

The pricing is $5.00 per terabyte of data scanned and it use compressed or columnar data for cost saving.

The use cases are: business intelligence, analytics, reporting, analyze and query VPC flow logs, ELB logs and cloud trail.

The next image illustrate the responsibilities of each service according and specific action:

![Athena](../assets/images/06L-amazon-athena.png)

Amazon QuickSight
-----------------

Quicksight is a serverless machine learning powered business intelligence service to create interactive dashboard like the next one:

![QuickSight](../assets/images/06M-quicksight.png)

It is fast, automatically scalable, embeddable with a price per session. It can be integrated with RDS, Aurora, Athena, Redshigt and S3.

Their use cases are: business analytics, building visualization, perform ad-hoc analysis and get business insight using data.

DocumentDB
----------

As Aurora is an AWS implementation of PostgreSQL and MySQL, DocumentDB is the same for MongoDB that is popular a NoSQL database. MongoDB is used to store query and index JSON data. DocumentDB has the similar deployments concepts as Aurora so it is fully managed, highly available with replication across 3 availability zones. Automatically the store grows in increments of 10 gigabytes and it scales workloads with millions of request per seconds.

Amazon Neptune
--------------

Amazon Neptune is a fully managed **graph** database. Maybe the most popular graph dataset would be a **social network** where you have a lor of action and relations across users a post as show the next image:

![Social Network](../assets/images/06N-social-network.png)

Neptune is available across 3 availability zones with up 15 read replicas. It builds and run applications working with highly connected datasets optimizing these complex and hard queries. Also, it can store up to billions of relations and query the graph with milliseconds of latency.

So the graph database are great for knowledge graphs as Wikipedia, fraud detections, recommendation engines and social networking.

Amazon TimeStream
-----------------

TimeStream is a fully managed, fast and scalable serverless **time series database**. So what is a time series data? Well, it's data that is evolving over time. So, for example, if you have on the vertical axis a number, and on the horizontal axis we have the year that is evolving from an older date to a newer date. Like the date is evolving over time, we are talking about a time series datasets.

TimeStream automatically scales up/down to adjust capacity. It stores and analyze trillions of events per day 1000 seconds faster and 1/10 of the cost of relational databases. It is a built-in series analytics function that help you identify patterns in your data in near real-time.

Amazon QLDB
-----------

QLDB standos for _Quantum Ledger Database_. A ledger is a book to **recording financial transactions**. This database is fully managed, serverless, high available and with replication in 3 availability zones. It is used to review the history of all the changes made to your application data over time via and immutable system (i.e., no entry can be removed or modified, and it is cryptographically verifiable). The next image is and example of a QLDB journal:

![QLDB](../assets/images/06O-qldb.png)

It has a 2-3x bettern performance than common ledge blockchain frameworks, and the data in manipulated usign SQL. There is another amazon service called Amazon Managed Blockchain and the difference with it is that QLDB is use a centralized component while Amazon Manage Blockchain use a decentralized component.

Amazon Managed Blockchain
-------------------------

Blockchain makes it possible to build applications where multiple parties can execute transaction **without the need for a trusted, central authority**/. This service is use to: join public blockchain networks or create your own scalable private network. It is compatible with the frameworks Hyperledger Fabric and Ethereum.

AWS Glue
--------

Glue is a fully serverless managed service to extract, transform and load (in short ETL) data. It is useful to prepare and transform data for analytics. The next diagram is a common map of the use of a ETL service in AWS:

![Glue ETL](../assets/images/06P-glue-etl.png)

> Note: There is a Glue Data Catalog of dataset that can ve used with Athena, Redshift and EMR.

AWS Database Migration Service
------------------------------

Database Migration Service (in short DMS) is the quickly and securely way to migrate database to AWS, keeping guarantees over resilient and a self healing process. The source database remains available during the migration. It support two types of migrations:

- Homogeneous migrations: form Oracle to Oracle.
- Heterogeneous migrations: form Microsoft SQL Server to Aurora.

Then next image summarize a process that use DMS:

![DMS](../assets/images/06Q-dms.png)

Summary
-------

- _Relational Database - OLTP_: RDS & Aurora.
- _Difference between Multi-AZ, Read Replica and Multi-Region._
- _In-memory Database_: ElastiCache.
- _Key-Value Database_: DynamoDB and DAX for Dynamo Cache.
- _Warehouse - OLAP_: Redshift.
- _Hadoop Cluster_: EMR.
- _Athena_: Query data on Amazon S3.
- _QuickSight_: Dashboard on your data.
- _DocumentDB_: Aurora for MongoDB.
- _Amazon QLDB_: Financial transaction ledger.
- _Amazon Managed Blockchain_: Managed Hyperledger Fabric and Ethereum blockchains.
- _Glue_: Managed ETL and data catalog service.
- _Database Migration_: DMS.
- _Neptune_: Graph database.
- _TimeStream: Time-series database.
