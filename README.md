# Using Federated Queries With Athena

Federated queries in modern data warehouses refer to the capability to query and retrieve data from multiple, disparate sources, both within and outside the data warehouse, in a unified and seamless manner.

Federated queries have expanded to facilitate the querying of data hosted in various cloud provider services. For example, it is now feasible to query data stored in Google Cloud Platform (GCP) Cloud Storage or BigQuery by utilizing AWS Athena and vice versa. This inter-cloud querying capability is now supported across multiple cloud vendors.

## Objective

Develop and execute a multiple source federated query using Athena, Redshift, AWS Glue, Lambda Functions, Secrets Manager, and GCP Cloud Storage(GCS).

Prerequisite:
•	You'll need to have two S3 buckets created, one with S3 data file(NaturalDisaster.Txt used for this demo), and
  one bucket required for Athena results.  
•	You'll need to have a redshift database populated with sample tickit data(tickit used for this demo). Also make a note of JDBC URL, security group and subnets of your redshift instance.
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

2. Search for Glue services in AWS console. Find Crawlers in left window pane. Click on create crawler.
3. Input the name for your crawler, choose "Data Store" as source type, Choose S3 as data source and provide awsweatherdata bucket path in the include path variable.
4. You'll need to choose an IAM role in next step. Create a new role and attach AmazonS3FullAccess policy.
5. Schedule crawler to run on demand.
6. In the output section, provide a name for new database, and finally create crawler.
7. Once created, your crawler properties will look something like this:

<img width="468" alt="image" src="https://github.com/visheshwalia/AWS-ETL-Pipeline/assets/49346509/9a97349b-d743-497e-a557-476e03081e68">

8. Run your newly created crawler. Once successfully executed, you'll find a new table created in the "Tables" section in left window pane.
9. In the schema section of the table, you can edit schema and change the column names and datatypes according to the screenshot below:
    
<img width="479" alt="image" src="https://github.com/visheshwalia/AWS-ETL-Pipeline/assets/49346509/8e7802ad-8146-47f0-9e7f-5c087632e709">

10. Click on "Preview Table" button, it'll prompt you to open Athena for query execution.
11. To query table in Athena, you'll first need to edit "Settings" and add spill bucket path to "Query Result Location" bar in Athena settings.
12. Try and run your query again. You should be able to see an output like this. Note: For data bein queried in S3, you'll find data source as AwsDataCatalog and Database that you created during your crawler configuration.:

<img width="468" alt="image" src="https://github.com/visheshwalia/AWS-Athena-FederatedQueries/assets/49346509/abf73859-1c14-482d-afd3-ed745a2f1f17">

13. Now for setting up federated queries for Redshift in Athena. Note: You must have a redshift database created with tickit data populated.
You can refer to this link to check how can you upload sample data in redshift from S3 : https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-create-sample-db.html

14. In Athena console, choose "Data Sources" from the left window pane. Create a new data source, choose Amazon Redshift as a data source.
15. Fill out a name for data source, in the connection section choose to create a new lambda function, it'll take you to another tab for configuring your new lambda function.
16. Provide an application name, secret prefix (you can choose to write anything), spill bucket path, security group and subnets.
17. Create a default connection string from this format: "redshift://<JDBC URL>?user=<redshift username>&password=<redshift password>" and provide it in the lambda function as well.
18. Once created, you'll find your lambda function and application in Lambda console. If it doesn't deploy successfully, you can delete the stack from CloudFormation, check all the values are correct and try to create it again.
19. Come back to Athena data source configuration, choose newly created lambda function and finally create your data source.
20. Check 2 things:
    •	Does your associated IAM role in redshift has access to S3 buckets. If no role is attached, attach one and provide sufficient access.
    •	Go to your lambda function configurations and check if environment variables has redshift default connection string present in a key-value pair. 
22. You'll need to create a VPC endpoint as well. In VPC service, choose the VPC associated with Redshift and Lambda function. In the left window pane choose "Endpoints".
23. Create a new endpoint, Service Category: AWS Services, Services: S3 (choose the region of your bucket), Endpoint type: Gateway. It'll allow your lambda function to access S3 bucket.

<img width="468" alt="image" src="https://github.com/visheshwalia/AWS-Athena-FederatedQueries/assets/49346509/017dd9aa-1252-4378-b0d7-3bac940a6129">

24. Once all of this is done, you can query your redshift data using Athena.

<img width="468" alt="image" src="https://github.com/visheshwalia/AWS-Athena-FederatedQueries/assets/49346509/9052f606-5a72-41bd-b7a6-b38750d10b0d">

Note that we are only querying data from redshift, we can choose data source as the one we created for redshift in athena and database name as in your redshift instance.

25. When we'll query data from multiple source in a single query, we'll need to use AWSDataCatalog as your data source. You'll need to add "lambda:<lambda function name>" to query data like shown in screenshot below.

<img width="468" alt="image" src="https://github.com/visheshwalia/AWS-Athena-FederatedQueries/assets/49346509/3fa7c252-3f84-453e-bed1-0be5ac906005">

26. Let's see how a federated query using both data sources(AWS Glue data catalog and Redshift) would look like.
    
<img width="468" alt="image" src="https://github.com/visheshwalia/AWS-Athena-FederatedQueries/assets/49346509/94015555-2339-440e-8a85-7cac014c01d7">

27. 

29. 





