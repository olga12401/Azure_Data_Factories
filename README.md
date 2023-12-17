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

16. Add a new activity 'Copy data' and complete steps 9 through 15 for the second file.
17. Let's begin creating the third table, which will be placed in the database. To do this, we need two tables from the database: 'orders' and 'item'. Go to Factory Resources -> Data flows -> create new.
18. Add the source. It will be a table from the SQL database

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/73d7c6a2-b958-408e-a4be-a7f79a96f76a)

19. When working with data flows, we need to enable 'Data flows debug' to check our flow. Remember to disable 'Data flows debug' after creating the flows because you will be charged for it
20. Check 'Projection' and 'Inspect'.
21. Data preview - refresh. We must see out table with the data, if we don't have error.
22. Add second table to source.
23. Click '+' near first table and find actions

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/bcf2f390-dc54-4c98-8b65-655c685b358a)

24. Add 'Sort'. I want to sort table by order_id

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/bf8276d6-c13f-46e4-bad1-a3c716e2fcb3) 

25. Add new actions 'Join' and choos properties fot actions. After make 'data preview' and check table.

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/daa53b38-0790-4321-a591-df94b1c8a94c)

24. I know that my 'Item' table has value NULL in column item_id, but I don't need this rows, so I create new actions 'Filter' me new table.

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/8dc94a2a-7744-4fd2-9f8d-f8579fa699cb)

25. After refreshing I see that my table has two columns with the same information about 'item_id. I need only one.

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/f103e018-18df-4172-8603-cd397b0d0181)

26. Create new action 'RemoveColumn'

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/6069d025-5052-429b-9f61-d22a4adaf249)

Find colunm that we want to delete and click 'remove mapping'. 

27. Data Preview -> Refresh .
28. If we make all transformation then last step will be always 'Sink'

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/a18216b2-8dc7-4f70-9451-8d12d8df9b68)

29. Add sink properties and agaein 'Data preview' -> Refresh.
30. Click 'Data flows debug' desable, if it doen't have error.
31. Click to pipeline -> add activity -> Data flow

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/c9e4bc1a-9da4-4fcf-8108-e4b1d03981b6)

32. In 'General' tab to choose our data flow, that we creted .

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/5515a5dd-a99b-4bc1-ace9-384281d8a6e0)

33. Click in the empty space and click 'Debug'.
34. We will carry out the steps sequentially, if there are no errors, then all the data will appear in the database.
35. Go to the sql database and check information in the tables. 
