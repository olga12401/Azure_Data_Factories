# Azure Data Factories

Data Set for example: https://mavenanalytics.io/data-playground?accessType=open&tags=NXBNqbCUPNOBzgZYNeH6x  Restaurant Orders .

# Goal

1. Creation of a pipeline to merge multiple files with the same structure: This pipeline will enable the merging of data from different files into a single file for further processing.
2. Storing data in storage and a SQL database: We plan to save the merged data in Azure storage and transfer it for storage and processing in a SQL database.
3. Development of a pipeline to transform files in Azure Data Factory: Creating this pipeline in Azure Data Factory will facilitate the process of transforming data from files and saving the obtained results in the database for further use.

# Preparatory steps
1. Create resourse group
2. Create storage account
3. Create SQL database
4. Click Data Factories -> Create

# Work with storage

1. Create container for orders files
2. Create conteiner for other files
3. Input files for conteners

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/123f30ac-28f2-4f64-99df-7fbc53f9df50)

# SQL Database
1. Open database
2. Query editor
3. Create tables for data

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/16a06bce-236c-43e4-a810-abefb31a43b3)

# Work with Data Factory

We need to create a connection for all the resources we'll use in the data factory-

## Create connection to the blob storage

1. Open data factory

2. Click ti Author  ![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/a52767da-4224-4e59-97b7-ecce3e1785d6)

3. Go to Manage and create new -> find blob storage
   
![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/bf1d4d11-6102-4887-9a0c-af56c9e9af87)

4. Write name cor connection
5. Add storage
6. Test connection
7. Create

## Create connection to the data base

1. Add new connection
2. Choose Sql database
3. Add information for connection

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/e719d4d3-b2f5-4834-abd8-84ed763cfae1)

4. Test connection Ok
5. Create

## Pipeline for merge files

When working with the Azure portal and processing multiple files of the same structure stored in a specific folder in storage, there are several options for efficiently transferring this information to a SQL database:

Individual loading: One option is to load each file separately. However, this method is extremely time-consuming because a separate loading process needs to be created for each dataset.

Parameter optimization: It's possible to optimize the process using parameters to speed up the loading process. But even with this optimization, when dealing with thousands of files, the loading time will still be significant.

File merging: Instead, you can retrieve all the files from the folder and merge them into one file. This approach reduces the number of data loading requests, significantly speeding up the data transfer process to the SQL database.

It's critically important to consider the data volume, required update frequency, and the complexity of file processing. The optimal choice of method will depend on these factors and project requirements.

In this example, let's consider a scenario where we have many files all stored in one folder in storage. Our goal is to create a pipeline that allows us to merge the files into one and store them in a container. 


