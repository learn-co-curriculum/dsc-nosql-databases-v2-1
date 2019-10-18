
# NoSQL Databases

## Introduction

In this lesson, we'll learn about some of the various kinds of NoSQL databases, and their use cases.


## Objectives
You will be able to:
- List different NoSQL databases 
- Describe the components of the different NoSQL databases 


## Key-Value Stores and Column Stores

Although **_Key-Value Stores_** and **_Column Stores_** are two different kinds of databases, they both store their data in ways you're already familiar with, because you've seen them in Python and Pandas!  **_Key-Value Stores_** work exactly like they sound -- just like a Python dictionary! They are powered by a hash table under the hood, and to access data, you just pass in the unique key for the value that you want. 

**_Column Stores_** work a bit more like how pandas stores data under the hood. Recall that in pandas, a DataFrame is stored as a collection of `Series` objects, with each column getting it's own series. The same general principle is at work for Column Stores -- each column is stored separately, allowing for very fast reads when querying data. 

##### Popular Key-Value Store Database: Amazon DynamoDB

##### Popular Column Store Database: Google BigTable

| <img src="images/bigtable.png" height=60% width=60%>    | <img src="images/dynamodb.png"> |
|---------------------|---------------------|


## Graph Databases

**_Graph Databases_** are based on the idea of the kind of graphs you might have seen in higher-level math classes -- the kind that are made up of **_Nodes_** and **_Edges_**. These sorts of databases excel at storing data where there are multiple associations between the entities stored in the database. The most obvious use case here is social media. Think for a second about what it would take to represent all of your friends on Facebook or Twitter in a relational database. This would be a pretty gargantuan data engineering task! However, with Graph Databases, it's quite easy. In a graph database, every user is a node, and each connection between you and one of the nodes representing one of your Facebook friends is an edge. In each node, we can store an arbitrary amount of metadata, which allows us to store things like user information. These sorts of databases really shine when you need to ask questions about the connections between users or objects in a database. For instance, what if we wanted to determine who the greatest influencers are among the Data Science community on Facebook? Even thinking about how you might approach this problem in a relational database is a surefire way to get a headache, but this sort of query is quite easy in a Graph Database -- one approach might be to examine all the Nodes with a metadata tag listing their job as "Data Scientist", and then examine who has the most shared connections among this community. 

##### Popular Graph Databases: Neo4J and OrientDB


| <img src="images/neo4j-logo.png"> | <img src="images/orientdb-logo.png"> |
|---------------------|---------------------|



## RDDs and Big Data

The Term "RDD" is short for **_Resilient Distributed Dataset_**, and it's the backbone behind all the fuss surrounding "Big Data". The most popular platforms that support RDDs are Hadoop and Spark, which are related to one another. Spark is built on top of Hadoop and allows users a cleaner, faster way of dealing with distributed datasets. A _Resilient Distributed Dataset_ is a collection of an arbitrary number of server clusters that share a full dataset distributed between them. With multiple servers comes multiple opportunities for things like server failures, and our dataset is no good if we have no guarantee that part of our dataset might be missing when we go to query for it. Relational databases are stored on a single server, which means that when the server is down, there's no doing anything until it comes back up. However, what if our Hadoop cluster contains 100 servers, and 2 are down when we write our query?  Without a way of making sure our RDD is **_Fault Tolerant_**, we could get incorrect information back because we think we've queried the entire dataset, but don't realize that two servers weren't up to contribute their data to the query. The way that RDDs like Hadoop and Spark deal with this is by chopping the data into different labeled blocks, and then storing multiple different backups of each block across all the different server clusters. For example, let's say that we've divided our dataset into blocks labeled, A, B, and C. Server 1 will host block A, but will also contain a backup of block B, and a backup of Block C, in that order. Server 2 will host Block B, but will contain backups of Block B and Block C, while Server 3 will host C first and contain backups of A and B. When things are working smoothly, Server 1 will run the query passed only on block A, Server 2 on Block B, etc. If Server B goes down, then our query will run on Blocks A and C because of Servers 1 and 3 first, and then the backup will kick in and Server 1 will then run the query on Block B. 

If you've never worked with RDDs before, this probably sounds slow and unnecessarily redundant. On small or normal-sized datasets, you're absolutely right. However, in this day and age, there has been a massive explosion in the amount of data created and stored every day. When working with truly _Big Data_, the distributed architecture provides massive speedups to query times by distributing the workload across multiple servers. 

As an example, let's pretend that you're a Data Scientist for a major retailer that sells everything under the sun at discount prices -- we'll call this company WallMart. At the end of every day, your boss wants to know the total number of sales in each department. If your data was stored in a relational database, this would mean executing a query on a single, truly massive relational database, which can only work as fast as its processor allows. Each query would have to go through trillions of rows to find what it needs, aggregate the sales for each category, and then return the answer. Running this query on a traditional relational database would likely take days, or even weeks! 

However, this would be a very different story with an RDD like Hadoop or Spark. Instead of running this on a single machine, we can just put a small server in each store location to keep track of all that store's data and transactions. When we need to run a query, our distributed system will ask each store's server to get the total number of sales for each department _simultaneously_. For a single store, this is a much smaller query, so it will run quite quickly. Then, once we have the totals for each department from each store, we can just combine them together into grand totals for each, getting us the answer we needed in seconds instead of days! The idea of having each server run some query or function at the same time, and then combining the results is based on the idea of **_MapReduce_**, which is the secret sauce behind RDDs. When we ask each server to run the function or query at the same time, this is the "Map" step. When we combine the results from each server into a single aggregate, this is the "Reduce" step. 


| <img src="images/hadoop.png"> | <img src="images/spark.png" height=10% width=10%> |
|---------------------|---------------------|


## Summary

In this lesson, we learned about the various sorts of NoSQL Databases in the market today. We also dug into the similarities and differences between them all. 
