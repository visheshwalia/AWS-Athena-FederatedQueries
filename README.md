# Using Federated Queries With Athena

Federated queries in modern data warehouses refer to the capability to query and retrieve data from multiple, disparate sources, both within and outside the data warehouse, in a unified and seamless manner.

Federated queries have expanded to facilitate the querying of data hosted in various cloud provider services. For example, it is now feasible to query data stored in Google Cloud Platform (GCP) Cloud Storage or BigQuery by utilizing AWS Athena and vice versa. This inter-cloud querying capability is now supported across multiple cloud vendors.

## Objective

Develop and execute a multiple source federated query using Athena, Redshift, AWS Glue, Lambda Functions, Secrets Manager, and GCP Cloud Storage(GCS).

Prerequisite:
•	You'll need to have two S3 buckets created, one with S3 data file(awsweatherdata), and 		one spill bucket required for Athena results.
•	You'll need to have a redshift database populated with sample tickit data(tickit). 
•	A GCP account.

After completing this lab, you will be able to:
•	Navigate the Glue application
•	Create a data crawler using Glue
•	Create a schema and tables in Glue
•	Create a lambda function
•	Query S3 data from Athena, Redshift, and GCS using the Glue data catalog

## Execution

1. Once you have uploaded you data in S3, it'll look something like this:

<img width="468" alt="image" src="https://github.com/visheshwalia/AWS-ETL-Pipeline/assets/49346509/ac516139-0388-4a22-967c-cd9a280ed1cc">




