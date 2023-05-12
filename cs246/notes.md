# CS246 Mining Massive Data Sets

This repo contains my notes on [CS246 Mining Massive Data Sets](https://web.stanford.edu/class/cs246/).

## What is this course about?

The course covers data mining and machine learning algorithms for analyzing very large amounts of data. The focus is on
MapReduce and Spark as tools for creating parallel algorithms that can process very large amounts of data.

**Topics include**: Frequent itemsets and Association rules, Near Neighbor Search in High Dimensional Data, 
Locality Sensitive Hashing (LSH), Dimensionality reduction, Recommendation Systems, Clustering, Link Analysis, 
Large-scale Supervised Machine Learning, Data streams, Mining the Web for Structured Data, Web Advertising.

*CS246 is one of the most useful classes at you'll take at Stanford if you want to become a Data Scientist or an ML Engineer.*

CS246 is fast paced! Requires programming maturity and strong math skills.

## Introduction - MapReduce and Spark (Tue Apr 4)

### Slides

See https://web.stanford.edu/class/cs246/slides/01-intro.pdf

* Course website: http://cs246.stanford.edu
* Class textbook: Mining of Massive Datasets by A. Rajaraman, J. Ullman, and J. Leskovec. http://mmds.org
* MOOC: www.youtube.com /channel/UC_Oao2FYkLAUlUVkBfze4jg/videos

#### Large Scale Computing

Large scale computing generally refers to the capability of hardware and software systems to dynamically 
adapt to an increasing load typically employing multiple, distributed nodes to complete a given processing task.

* **Issue**: Copying data over a network takes time.
* **Idea**: Bring computation to data and store files multiple times for reliability.
* Spark/Hadoop address these problems
  * Storage Infrastructure – File system: Google GFS. Hadoop HDFS)
  * Programming model: MapReduce and Spark.

#### Distributed File System

A Distributed File System (DFS) as the name suggests, is a file system that is distributed on multiple file 
servers or multiple locations. It allows programs to access or store isolated files as they do with the local ones, 
allowing programmers to access files from any network or computer.  See 
https://www.geeksforgeeks.org/what-is-dfsdistributed-file-system/.

#### Hadoop and MapReduce

Hadoop is a group of open-source software services. It gives a software framework for distributed storage 
and operating of big data using the MapReduce programming model. The core of Hadoop contains a storage part, 
known as Hadoop Distributed File System (HDFS), and an operating part which is a MapReduce programming model.

MapReduce is a style of programming designed for:

1. Easy parallel programming
2. Invisible management of hardware and software failures
3. Easy management of very-large-scale data

It has several implementations, including Hadoop, Spark (used in this class), Flink, and
the original Google implementation just called "MapReduce".

Generally MapReduce paradigm is based on sending the computer to where the data resides!

* MapReduce program executes in three stages, namely map stage, shuffle stage, and reduce stage.
* Map stage − The map or mapper’s job is to process the input data. Generally the input data is in the form of file 
directory and is stored in the Hadoop file system (HDFS). The input file is passed to the mapper function line by line. 
The mapper processes the data and creates several small chunks of data.
* Reduce stage − This stage is the combination of the Shuffle stage and the Reduce stage. The Reducer’s job is to process the
data that comes from the mapper. After processing, it produces a new set of output, which will be stored in the HDFS. 

See [Hadoop - MapReduce](https://tinyurl.com/tn387zyu) on tutorialspoint.

MapReduce environment takes care of:

* Partitioning the input data
* Scheduling the program’s execution across a set of machines
* Performing the group by key step (in practice this is the bottleneck)
* Handling machine failures
* Managing required inter-machine communication

#### Spark extends MapReduce

[Apache Spark](https://spark.apache.org/) is an open-source unified analytics engine for large-scale data processing. 
Spark provides an interface for programming clusters with implicit data parallelism and fault tolerance.

* Expressive computing system, not limited to the map-reduce model
* Additions to MapReduce model
  * Fast data sharing
    * Avoids saving intermediate results to disk  
    * Caches data for repetitive queries (e.g. for machine learning)
  * General execution graphs (DAGs)
  * Richer functions than just map and reduce
* Compatible with Hadoop
* Key construct/idea: Resilient Distributed Dataset (RDD)
* Higher-level APIs: DataFrames & DataSets
  * Introduced in more recent versions of Spark
  * Different APIs for aggregate data, which allowed to introduce SQL support

Spark RDD Operations

* Transformations build RDDs through deterministic operations on other RDDs
  * Transformations include `map`, `filter`, `join`, `union`, `intersection`, `distinct`.
  * Lazy evaluation: Nothing computed until an action requires it.
* Actions to return value or export data
  * Actions include `count`, `collect`, `reduce`, `save`.
  * Actions can be applied to RDDs; actions force calculations and return values.

Higher-Leval API: DataFrame & Dataset

* DataFrame:
  * Unlike an RDD, data organized into named columns, e.g. a table in a relational database.
  * Imposes a structure onto a distributed collection of data, allowing higher-level abstraction
* Dataset:
  * Extension of DataFrame API which provides typesafe, object-oriented programming interface (compile-time error detection)
* Both built on Spark SQL engine. Both can be converted back to an RDD.

Useful Librairies for Spark

* Spark SQL
  * [What is Apache Spark SQL?](https://www.databricks.com/glossary/what-is-spark-sql)
  * [Spark SQL Explained with Examples](https://sparkbyexamples.com/spark/spark-sql-explained/)
* Spark Streaming – stream processing of live datastreams
  * [What is Spark Streaming?](https://www.databricks.com/glossary/what-is-spark-streaming)
  * [Spark Streaming Programming Guide](https://spark.apache.org/docs/latest/streaming-programming-guide.html)
* MLlib – scalable machine learning
* GraphX – graph manipulation

### Youtube

See [Distributed File Systems](https://www.youtube.com/watch?v=jDlrfBLAIuQ) on Youtube.

Cluster Computing Challenges

* Node failures
  * A single server can stay up for 3 years (1000 days)
  * 1000 servers in cluster = 1 failure/day
  * 1M servers in cluster = 1000 failures/day
  * How to store data persistently and keep it available if nodes can fail?
  * How to deal with node failures during a long-running computiation?
* Network bottleneck
  * Network bandwith = 1 Gbps
  * Moving 10 TB tages approximatly 1 day
* Distributed programming is hard!
  * Need a simple model that hides most of the complexity

MapReduce addresses the challenges of cluster computing

* Store data redundantly on multiple nodes for persistence and availability
* Move computation close to data to minimize data movement
* Simple programing model to hide the complexity of all this magic

Redundant Storage Infrastructure

* Distributed File System
  * Provides global file namespace, redundancy, and availability
  * E.g. Google GFS; Hadoop HDFS
* Typical usage pattern
  * Huge files (100s of GB to TB)
  * Data is rarely updated in place
  * Reads and appends are commun
* Data kept in "chunks" spread across machines
* Each chunk replicated on differents machines
* Chunk servers
  * File is split into contiguous chunks (16-64 MB)
  * Each chunk replicated (usually 2x ou 3x)
  * Try to keep replicas in different racks
  * 


### Chapter 1 Data Mining

See http://infolab.stanford.edu/~ullman/mmds/ch1n.pdf

### Chapter 2 MapReduce and the New Software Stack

See http://infolab.stanford.edu/~ullman/mmds/ch2n.pdf

## Frequent Itemsets Mining (Thu Apr 6)

### Slides 

### Colab

See https://stackoverflow.com/questions/55874956/how-to-open-spark-ui-when-working-on-google-colab

