# Data Warehouse and ETL Offload Code Samples - Cloud at Customer Addendum

_Copyright © 2020, Oracle and/or its affiliates. All rights reserved.
The Universal Permissive License (UPL), Version 1.0_

The Data Warehouse and ETL Offload Code Samples support data warehousing and ETL offload solution patterns on Oracle Cloud and on Oracle Cloud at Customer. This addendum describes how to use the code samples described in the primary README.MD file in a Cloud at Customer environment.  

## Contents
  
   * [Prerequisites](#prerequisites)
   * [Oracle Cloud at Customer ETL Offload](#oracle-cloud-at-customer-etl-offload)



## Prerequisites
You should be familiar with provisioning and using the recommended services and technologies:

* Oracle Autonomous Data Warehouse Cloud
* Oracle Data Integration Platform Cloud
* Oracle Analytics Cloud
* Oracle Big Data Cloud at Customer

Use the information in the primary README.MD file to understand and install the necessary sample files..
 

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
