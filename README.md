# Azure Data Factory

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

### Steps 

1. Create dataset for input files ( Orders) -> Factory Resource -> Datasets -> create new
2. Find blob storage -> Continue
3. Choose the format type of your data ( I choose csv )
4. Add set properties: Name, choose storage, file push. I selected only the 'input' folder containing my 'Orders details' files because I want to merge all these files into one. All files have the same structure. Import schema = from connection/storage
5. Ok

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/8d043a91-edd3-4ef0-ae6f-3cf7034567e2)

6. Test connection
7. Open Activities -> Move and transform -> Copy data
8. Create name for pipeline
9. Go to Source

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/6235eefd-1ef4-4ea1-9924-9f5dee5c1c7d)

10. Choose datasets (this dataset consists of orders files from the 'input' folder in the storage).
11. File path type: Wildcard file path = OK.
12. Wildcard paths to write '*' because we will use all files from the 'input' container.
13. Go to Sink

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/b49bce5b-bcba-41e2-84a7-5156d5f1700f) 

14. Sink dataset = my dataset with Orders files
15. Copy behavior = Merge files. This is the command to combine my files into one.
16. In File extension to choose .csv (tipe new file in container 'input')
17. Debug
18. Check result in the storage

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/d10e86cd-3519-4461-ab96-5d160fc11651)

## Create a pipeline to transform the information and then transfer it to the database

Scheme pipelene

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/477b4360-71f5-4d51-8cc1-043bda2d98df)

On the Azure portal, a pipeline consisting of three key stages was created. The first two stages are aimed at extracting data from the 'items' and 'orders' files. 'Orders' is obtained as a result of executing the pipeline named 'MergeOrdersCSV'. After loading this data into the SQL database, our goal is to apply data flows and perform a join operation to create a new dataset, which will also be loaded into the SQL database table.

All three stages are structured with arrows indicating the sequential execution of all operations. Each action logically continues the previous one, ensuring a clear sequence of operations in the data processing workflow on the Azure platform.

Scheme data flows

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/efb56de2-63c5-40c1-8e52-62c749caf8af) 

### Steps

1. We need to create datasets for input and output data from the 'items' and 'orders' files. For each file, we'll create one dataset for input data from a CSV file and another dataset for output data into an SQL table
2. Factory Resources -> Data sets -> create new -> Azure Blob Storage -> CSV file (type)
3. Add set properties

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/ceee32da-0b33-4fe0-b0e0-49086477b646)

4. Test connection
5. Data sets -> create new ->Azure SQL DataBase - > choose database and write Name - >Continue

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/61b554d5-6032-44fc-94de-c95a2ae890b7)

6. Choose table in sql database for. Test conection.

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/76498605-e43b-4e1d-b77d-4a6f1a8dfa84)

7. Import schema data

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/6de8c983-664f-4704-a4f2-06470bae56fd)

8. Complete steps 2 through 8 for the 'Orders' file (create two datasets).
9. Factory Resources -> Pipeline - > Create new -> Activities -> Move and transform -> Copy data
10. select element. General tab -> write name for element

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/0cb8ac47-7710-43f1-a41c-9a68563ed30b)

11. Click 'Source' tab
12. Source dataset -> choose datasets for input file from the storage
13. File path type -> File path in dataset
14. We can make 'oreview date' and look our table.
15. Click Sink and choose 'Sink dataset' with the table in the data base

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/2a51c047-39c7-40bc-959a-67938670f9b7)

