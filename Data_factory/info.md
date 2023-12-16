# Pipeline with parameter

## Input data from storage

1. Return to Author tab
2. Datasets -> Create new datasets
3. Find blob storage -> Continue
4. Choose the format type of your data ( I choose csv )
5.  Add set properties - OK 

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/8bf3617d-f3cf-484a-97d3-1915f2ec5693) 

7. Check you data

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/3d0c2e6f-6b32-481a-86c4-2ef62cd602d8)

## Add info to table in sql base 

1. Create new datasets for output data
2. Find sql Data base
3. Set properties

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/e3614a05-f66c-4965-a8c4-e2379ba5ba0a) 

4. Ok
5. Test Connection

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/bd3bb334-b125-4572-8646-6ec0306c0d2b) 

# Create Pipelines

1. Create new pipelines - > write name
2. First step is " Move and transform" - > Copy data . We copy the data from storage to base.

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/d6b37bfc-de71-49e3-b253-27ae583a44be) 

3. Source datasets is our InputCVS.
4. Sink datasets OutputTable.
  InputCVS and  OutputTable we created on the previos step.
5. Click here

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/d7e496de-934e-4edc-9821-24120095884b)

6. Run Debug and find error

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/5942c2fc-0f1c-4fcf-aa92-4ec5b7248fb1) 

# Add another cvs files using parametrization 

1. Click the datasets for InputCSV
2. Click "parameters" -> New -> Write Name as "fileName"
3. Go to Connections
4. Remove File name and add dynamic content

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/d6957a45-5292-4b08-9297-5c92146c091b)

5. Finish

Now a paramiter will be used in this file path to pick the file name.
6. Click the datasets for Output data (OutputTable)
7. Click "parameters" -> New -> Write Name as "tableName"
8. Go to Connection

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/445202d1-76af-4a78-b7c2-546a1cfa70d9)

# Changeing pipelines 

Now we need to match the name of the parameter and files.

1. Change a property for Source
 ![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/126cdcd2-3cff-4902-9774-e64c1cee37b1)

2. Change a property for Sink

 ![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/8246d162-08e1-4f32-bd6b-5ac62d834027) 

3. Click "Debug"
4. We must get "Pipeline status Succeeded"

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/3132c2e9-3c1d-4f74-94db-b7ce3a0b63d8)

We can now modify the 'Value' in the dataset properties and map all tables and files. There's no need to create distinct sets of input and output data for each file.

If we have a resuts "Debug" as error,  for exsample, we have a different columns name between sql table aan csv file.
Click to pipepline -> Mapping -> import schemas -> write all match for columns -> debug.

![image](https://github.com/olga12401/Azure_Data_Factories/assets/86374953/3f8717d4-ee33-4e75-9eb8-63a693f69059) 




