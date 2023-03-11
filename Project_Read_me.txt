Goal:
Create a Databricks Data Lakehouse on Cloud for a business using SQL Server onprem. Synch the data from onprem to Cloud Data Lakehouse in real time. Create the bronze, silver and gold layers. Create the dashboards using the Gold layer data or silver layer data. Create users and allow them to access the dashboards.

Highe Level Architecture:

SQL SERVER (CDC Enabled) ------>CDC tool (debezium) SQL Connector--> Kafka topics--->S3 sink connector---->S3 Bucket---> Databricks Tables(bronze)---> Silver(ETL) -----> Gold(ETL)--> BI(Dashboards SQL Warehouse)

Project High Level Phases and mile stones under each phase:

	phase 1 (Phase: SQL SERVER)
		1) Get a AWS Account (admin)
		2) Create a Virtual Machine in AWS and install SQL Server
		3) Adminstrate the SQL Server Enable programmatic logins
		4) Create a new user name and password with permissons on new db
		5) Create new database, sample tables and add data
		6) Read documentation and enable CDC feature on the Database and tables
		7) Validate that cdc is enabled on our sample tables
		8) Create a sample python program to read data from Databricks notebook connected to SQl Server
		9) open the port for sql server on virtual machine to be accessed by CDC Tools (debezium)
	
	Extra:
		Create a program to create the sql script for enabling cdc on a sql database and tables
		Create program to check if CDC is enabled on database and tables
		Create a program to export metadata of database and tables to csv with the following details:
		schema with column names and datatype, database name, table name, primary key,zorder column names, number of records in table
		Create a yaml (encrypted file) file with following:
		database ip,  database password, database username, database name
	
	
	phase 2 (Phase: Debezium CDC TOOL on Aiven.io)
	    1) create an account on aiven.io and create a kafka service (business service)
		with kafka-connect and kafka registry enabled
		2) Start the kafka service on Aiven
		3) Create an S3 bucket on AWS (create accesskey and secret password)
		4) Configure aiven and use Source SQL Server Debezium connector to write to kafka
		   (look at the docs on aiven)
		5) Configure aiven to take data from kafka and write to S3 (S3 Sink Connector)
		   (look at the docs on aiven)
		6) Check that the s3 bucket received the events from kafka topic
        7) Validate data is being received by Kafka topics in Aiven		
		
    Phase 3	(create tables on Databricks using DLT - bronze and silver layers)
	   1) https://www.databricks.com/blog/2022/04/25/simplifying-change-data-capture-with-databricks-delta-live-tables.html
	   2) Create a new Databricks workspace or use existing workspace 
	   3) mount the s3 bucket using accesskey and secret (use alternate security model IAM)
	   4) create a notebook to read data from s3
	      and create the bronze and silver tables using DLT
	   5) Create DLT workflow, job and run the notebook to see the data in action
       6) Extra credit: 
	      How will you deal when the source database has 1000 tables
	      How do you control and get s3 files of size 1 gb and in a particular format
		  How do you handle schema changes that are coming in JSON	
          How do you know how much backlog is there in S3 that need to processed by DLT
          How to validate if the tables in onprem and Datalake house are in synch
          How to make sure the silver table is optimized and zordered for query purposes
    Extra:
		 create dashboard to show health of the synch between source and target data lakehouse
		 create process to restart synch on a table again
	   
	phase 4 (Gold layer in Data lakehouse)
		Transoformations (join multiple tables in Silver layer and create the gold layer DLT or Batch)
		schedule those notebooks as workflows
		
	phase 5 (BI Layer in Data lakehouse)
		Business Intelligence
		Create SQL Queries to display Business metrics using Databricks SQL Warehouse
		Create a dashboard on the SQL Queries with autorefresh
		Datawarehouse end point
		Talk about Unity Catalog for security Model considerations for large teams and companies		
	
	Phase 6
		Discuss with students and think about further improvements that can be made.
		Ask the students to sink from MySQL and repeat the process.

