# Data Warehouse and ETL Offload Code Samples Overview and Install Guide

_Copyright © 2018, Oracle and/or its affiliates. All rights reserved.
The Universal Permissive License (UPL), Version 1.0_

The Data Warehouse and ETL Offload Code Samples support data warehousing and ETL offload solution patterns on Oracle Cloud and on Oracle Cloud at Customer.   

## Contents
  * [Prerequisites](#prerequisites)
  * [Sample Source Files](#sample-source-files)
  * [Loading Autonomous Data Warehouse](#loading-autonomous-data-warehouse)
    + [Oracle GoldenGate Parameter Samples](#oracle-goldengate-parameter-samples)
    + [Create and Load Star Schema](#create-and-load-star-schema)
  * [Oracle Big Data Cloud Apache Spark Notebook](#oracle-big-data-cloud-apache-spark-notebook)
  * [Oracle Cloud at Customer ETL Offload](#oracle-cloud-at-customer-etl-offload)



## Prerequisites
You should be familiar with provisioning and using the recommended services and technologies:

* Oracle Autonomous Data Warehouse Cloud
* Oracle Data Integration Platform Cloud
* Oracle Analytics Cloud
* Oracle Big Data Cloud (for Oracle Cloud ETL Offload)
* Oracle Big Data Cloud at Customer (for Cloud at Customer ETL Offload)

Use the sample code with the solutions: 

* [Load the data warehouse for business analytics on Oracle Cloud](https://www.oracle.com/pls/topic/lookup?ctx=en/solutions/load-data-warehouse-for-business-analytics-oracle-cloud&id=ADWBA)
* [Load the data warehouse for business analytics on Cloud at Customer](https://www.oracle.com/pls/topic/lookup?ctx=en/solutions/load-data-warehouse-for-business-analytics-oracle-cloud&id=ADWBC)

## Sample Source Files
The files in the `SampleSourceFiles` folder are comma separate value (.csv) files that are used throughout the solution. The files are used in the Oracle Data Integrator Smart export samples (loading Oracle Autonomous Data Warehouse Cloud and ETL Offload for Oracle Big Data Cloud at Customer). They are also used in the sample Oracle Big Data Cloud Notebook for ETL Offload with Apache Spark processing in Oracle Cloud.

`CUSTOMER_SRC_FILE.csv` contains randomized customer names and region data that is used for the ETL offload sample.

| Column Ordinal | Column Name | Data Type |
| --- | --- | --- |
| 1 | CUSTOMER\_ID | NUMERIC |
| 2 | FIRST\_NAME | VARCHAR |
| 3 | LAST\_NAME | VARCHAR |
| 4 | REGION | VARCHAR |
| 5 | CITY | VARCHAR |


`SRC_PRODUCT.csv` contains product data that is used in Oracle Autonomous Data Warehouse Cloud to provide product category and family data for the ETL offload sample.

| Column Ordinal | Column Name | Data Type |
| --- | --- | --- |
| 1 | PRODUCT\_ID | NUMERIC |
| 2 | PRODUCT\_NAME | VARCHAR |
| 3 | PRICE | NUMERIC |
| 4 | FAMILY | VARCHAR |

`ORDERS_FILE.csv` contains order data that is used to load the SALES\_FACT and SALES\_ANALYSIS tables in Oracle Autonomous Data Warehouse Cloud.  It provides the data set for the SALES_ANALYSIS ETL Offload Apache Spark Notebook and Oracle Data Integrator project.


## Loading Autonomous Data Warehouse
The artifacts in the `LoadingAutonomousDataWarehouse` folder replicate a source table to Oracle Autonomous Data Warehouse Cloud for reporting with Oracle GoldenGate. They also load a star schema data warehouse with Oracle Data Integrator. 

### Oracle GoldenGate Parameter Samples
The artifacts in the `GoldenGateParameterSamples` folder represent sample Oracle GoldenGate parameter files for real-time replication of data from an Oracle database source (on-premise or Oracle Database Cloud Service) to an Oracle Autonomous Data Warehouse Cloud target. These files can be used with any Oracle GoldenGate for Oracle 12.3 implementation including the Data Integration Platform Cloud’s remote agent.

`exadwc.prm` is a sample Oracle GoldenGate EXTRACT parameter file to capture data from an Oracle database source.   To configure this file,  replace the \<SID> entry with your Oracle database SID or CDB name.     This file is configured to capture from the schema `ADWC_SRC`.
  
`padwc.prm` is a sample Oracle GoldenGate EXTRACT PUMP parameter file to pump transactions captured with the extract to the Data Integration Platform Cloud instance for delivery to Autonomous Data Warehouse Cloud.  To configure this file, replace the \<remote\_host_ipt> and \<SID> entries.
  
`radwc.prm` is a sample Oracle GoldenGate REPLICAT parameter file for a Classic replicat.   This applies the transactions to the Autonomous Data Warehouse Cloud instance.  To configure this file, replace the \<SID> entry with your SID or CDB name.  This applies data from the `ADWC_SRC` schema to the `adwc_repl` schema in your Autonomous Data Warehouse Cloud instance.  

Place these files in the dirprm directory for the Oracle GoldenGate installation: put `exadwc.prm` and `padwc.prm` in the source and put `radwc.prm` in your Oracle Data Integration Platform Cloud host or Oracle Data Integration Platform Cloud remote agent installation. 


### Create and Load Star Schema
The artifacts in the `ODISampleAutonomousDataWarehouseCloudLoad` folder provide scripts to create a small star schema in the Autonomous Data Warehouse Cloud and an Oracle Data Integrator Smart Export to load the Autonomous Data Warehouse Cloud star schema.  

To import and execute these scripts:  

1. Connect to your Oracle Autonomous Data Warehouse Cloud instance using SQL Developer.
1. Run the `CreateODI_USER_and_TABLES.sql` script to create the `ODI_USER` and the target tables for the Oracle Data Integrator project.  
 
   Replace the `<PASSWORD>` tag in the first line of the script with the password you want  to use for the `ODI_User`.
1. Use Secure Shell (SSH) to access your Data Integration Platform Cloud instance as the OPC user.
1. Create a directory in your Oracle Data Integration Platform Cloud instance:

        Mk dir /tmp/ADWCSample
        

1. Copy the sample source files `ORDERS_FILE.csv`, `SRC_PRODUCT.csv` and `CUSTOMER_SRC_FILE.csv` and the `ODIADWCSample.xml` file into the directory you just created.
1. Change permissions on the directory and files so that the Oracle user can read the files:

        Sudo chmod 755 -R /tmp/ADWCSample

1. Use virtual network connectivity (VNC) to connect to your Oracle Data Integration Platform Cloud instance.
1. Run Oracle Data Integrator Studio.
1. Use Smart Import to import the Oracle Data Integrator processes.
1. Go to the Topology Manager in Oracle Data Integrator Studio and alter the Oracle Autonomous Data Warehouse Cloud connection strings. See the solution documentation for more details on how to obtain and configure your Oracle Data Integrator / Oracle Autonomous Data Warehouse Cloud connection.
1. Open the Oracle Data Integrator project and review the mappings.
1. To run the processes, execute the Oracle Data Integrator package `PKG ODI_User DW Sample Load`.
1. Use SQL Developer to query the tables in the Oracle Autonomous Data Warehouse Cloud `ODI_USER` schema.
	  - `CUST_DIM`
	  - `DATE_DIM`
	  - `PRODUCT_DIM`
	  - `SALES_FACT`
	  - `SALES_FACT_ANALYSIS`
  
1. Review the Oracle Data Integrator mappings.  Note that the two folders provide two methods for executing dimension and fact loads in Oracle Autonomous Data Warehouse Cloud:

    **Loads Using Mappings and KMs** provides mappings the leverage certified Knowledge Modules for Oracle Autonomous Data Warehouse Cloud.   These are appropriate for Type 1 dimension style loads.

    **Loads using Dimensions and Cubes objects** provides mappings that leverage the Dimensions and Cubes objects within Oracle Data Integrator to load the same set of tables into the Oracle Autonomous Data Warehouse Cloud `ODI_USER` schema. The Dimensions objects are appropriate for Type 2 or Type 3 dimension loads.

## Oracle Big Data Cloud Apache Spark Notebook
`BigDataCloud - ETL Offload Sample Notebook.json` is a sample Oracle Big Data Cloud Notebook that uses Apache Spark to load data from files stored in Oracle Object Storage. It performs an ETL routine leveraging SparkSQL and then stores the result in multiple file formats back in Object Storage.

To configure and execute the Apache Spark notebook:

1. Login to your Oracle Cloud account.
1. Create a container within your Oracle Cloud Infrastructure Object Storage service; for example, `BDCETL`.
1. Navigate to your Oracle Big Data Cloud instance and the Notebook tab.
1. Click Import to import the notebook.
1. Open the notebook and click  **Run / Play** to execute the Apache Spark script. 
1. The Apache Spark notebook generates a SALES\_FACT_ANALYSIS that is similar to the results of the Oracle Data Integrator Oracle Autonomous Data Warehouse Cloud processes. 

To review the results, execute the %SQL section of the notebook which queries the resulting SALES_ANALYSIS table in Oracle Big Data Cloud.

## Oracle Cloud at Customer ETL Offload
`ODISmartExport\_ETLOffload\_BigDataCloud@Customer.zip` contains artifacts that demonstrate Oracle Data Integrator performing ETL Offload in an Oracle Big Data Cloud at Customer (Cloudera) environment. SQOOP or Apache Spark use the artifacts to load the sample source file data set into the Cloudera Cluster and then Oracle Data Integrator executes Apache Spark workloads to generate the SALES_ANALYSIS data set. 

To configure and execute the Oracle Data Integrator jobs: 

1. Follow the documentation on configuring your Oracle Big Data Cloud at Customer machine for Oracle Data Integrator workload.  Do not create your topology objects yet.
1. Use Ssh / vnc to access the Oracle Data Integrator Agent node in your cluster.
1. Create a directory in your cluster's local file system:

        Mkdir /tmp/sourcefiles

1. Copy the file `ODISmartExport_ETLOffload_BigDataCloud@Customer.zip` to your Oracle Data Integrator agent node in your cluster.
1. Copy the sample source files `ORDERS_FILE.csv`, `SRC_PRODUCT.csv` and `CUSTOMER_SRC_FILE.csv` to the directory you just created.
1. Run Oracle Data Integrator Studio.
1. Smart Import the Oracle Data Integrator project into you environment.
1. Configure your topology connections. See the solution documentation.
1. Edit the `ORCL_SRC` connection to point to an Oracle database. The ingestion mappings pull from an Oracle database to demonstrate how to leverage either SQOOP or Apache Spark to ingest data to your cluster.
1. Configure your Apache Hadoop credential store for the ORCL\_SRC connection if you plan on using Apache Spark to ingest the data. See the solution documentation.
1. Review the Oracle Data Integrator project.
1. Create Oracle Source Objects.  

      The PKG Create Oracle Objects and Load Data will create tables in your source Oracle database and load the sample source files to that database.
1. Ingest Data to Oracle Big Data Cloud HDFS/SQOOP and Apache Spark.  

    The PKG Ingest Data to Hive leverages Oracle Data Integrator mappings to import data into your Oracle Big Data Cloud cluster leveraging SQOOP.  

      Open one of the mappings and navigate to the physical tab.  Note that there are physical designs for both SQOOP and Apache Spark ingestion.  Either can be leveraged to move data into the cluster.
1. ETL Offload – Apache Spark.  

    The Mapping MAP Apache Spark ETL Offload…   demonstrates how to create a mapping in Oracle Data Integrator that leverages Apache Spark to perform ETL offload to the cluster and create a SALES\_ANALYSIS data set.
1. Execute the packages / mappings listed above to
	  - Create Oracle source objects
	  - Ingest data into your cluster
	  - Generate a SALES\_ANALYSIS data set leveraging Oracle Data Integrator&#39;s Apache Spark capabilities.
