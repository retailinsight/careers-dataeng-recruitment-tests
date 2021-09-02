# Retail Insight MSSQL Case Study

## Introduction
This is an opportunity for you to demonstrate your knowledge of MSSQL.

You will need to complete tasks that will:
- create a new database
- create tables
- import some sample data and transform it

Please complete as many of the tasks as you can. Code provided has been kept to a minimum, we do not expect you to complete these task in exam conditions so please do your own research and use the search engine of your choice.
When you've completed all you can please submit a single file containing all your code, feel free to include any supplementary comments / commentary in this file.

Before moving on to the tasks please ensure you have completed the setup instructions.

## Setup

### Install MSSQL Server
If you don’t already have MSSQL server installed on your machine, please download and install MSSQL 2019 Express Edition from this link (it's free) here:

https://www.microsoft.com/en-us/Download/details.aspx?id=101064

- Download and run the `.exe`
- Choose `Basic` installation
- Make a note of the server name, when the installation completes you should see a screen like this, you can find the server name in the connection string (if you completed the installation with the default config this should be `localhost\SQLEXPRESS`).

![SQL Install](img\SQLInstall.png)

### Install SQL Server Management Studio (SSMS)
You will also need SSMS to complete this case study. If you don’t have this, please install the free version by clicking the `Install SSMS` button from the installation screen above or you can use this link:

https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?redirectedfrom=MSDN&view=sql-server-ver15

## Tasks

### Task 1 - Open SSMS and connect to installed instace of MSSQL server

Open SSMS, it should prompt you to connect to a database engine. If the server name isn't pre-populated, enter the servername you noted down in the `Install MSSQL Server` setup step.

![SQL Server Connect](img\SQLServerConnect.png)

Familiarise yourself with the SSMS environment

![SSMS](img\SSMS.png)

Highlighted are information you might find useful:
1. Object explorer – connect to a database engine to explore the databases, database objects, users etc.
2. Available databases – dropdown list used to change which database your query is connected to
3. New Query –  click to open a new tSQL query window
4. Execute – click to execute your query, either everything in the query window or a highlighted section (F5 as a shortcut for this).
5. Comment and uncomment section – use to comment out code or text that you don’t want to execute
6. Server connected to – displays the server your query is connected to
7. Session ID – your query session ID, useful for debugging or monitoring
8. Database connected to – database that your query is connected to

### Task 2 - Configure the Model database

This is one of the system databases and acts as a template for any new databases that you create.  So, for this step you will configure some defaults.
- In the object explorer window expand Databases > System databases
- Right-click on Model > Properties

**Files**

In the database properties under files you will see two items listed.
- One with file type of `ROWS Data` and file name extension `.mdf`; this is your primary database file.
- The second has file type of `LOG` and extension of `.ldf`, this is your database log file.

The `Autogrowth/ Maxsize` settings are important and tell the database engine how to increase the size of files as more data is loaded.

Generally, we don’t want to allow a percentage increase because this will not be linear.  As DB or log files grow that percentage may become unmanageable and we risk running out of disk space.
- Change the Autogrowth setting for the `LOG` file by clicking the ellipsis (…)
- Change file growth to `In Megabytes` and set that to `10`
- Click OK

**Recovery Model**

In the database properties under `Options` you will see a dropdown list for `Recovery model`.

The recovery model determines how transactions are logged.  There are various situations that require the different settings and you can read more about this here:

https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/recovery-models-sql-server?view=sql-server-ver15

For this exercise we require minimal logging and no log backups, for this we need to change the recovery model to `Simple`, make this change and click `OK`.

### Task 3 - Save RetailInsight_CaseStudy_Data.csv locally
Save the `RetailInsight_CaseStudy_Data.csv` file locally on your machine e.g. `C:\RetailInsight_CaseStudy\RetailInsight_CaseStudy_Data.csv`

### Task 4 - Create file for your code
Open a new query window in SSMS and save it as `RetailInsightCaseStudy_completedtasks.sql`, use this for all the code and comments you want to submit.

### Task 5 - Create a new databse using T-SQL
a. Create a database called `RetailInsight_Case_Study` using T-SQL, here is a simple T-SQL statement to create a database without any additional settings configured.
```SQL
CREATE DATABASE RetailInsight_Case_Study;
```
b. Right-click and refresh databases in the `Object Explorer`, you should now see your new database

c. Check the database properties (as you did for the `model` database) and make sure they are the same as the `model` database

d. Expand `Databases` > `RetailInsight_Case_Study` to see all the database objects, such as tables, views and programmability (functions and stored procedures)

e. In your query window comment out the create database statement and save

### Task 6 Create dbo.StageSalesData table
Create a stage table to import the sample data.

a. Change the database that the open query window is connected to by either selecting `RetailInsight_Case_Study` from the dropdown list in the top left of SSMS or by executing the T-SQL statement
```sql
USE RetailInsight_Case_Study;
```
b. Create a table called `dbo.StageSalesData` with the following columns and data types to match the sample data
```
TransactionDate VARCHAR(10)
StoreNumber VARCHAR(10)
StoreName VARCHAR(50)
SalesUnits VARCHAR(10)
```
c. Right click and refresh `RetailInsight_Case_Study` > `Tables` in `Object Explorer` to see your newly created table

### Task 7 Create stored procedure dbo.csp_ImportSalesData
This stored procedure will be used to import the sample data.

a. Write the stored procedure, the procedure should do the following:
- Clear all data from `dbo.StageSalesData` so that the table is empty, this could be a `DELETE` or `TRUNCATE` statement
- Bulk insert the data in `RetailInsight_CaseStudy_Data.csv` into `dbo.StageSalesData`.  You’ll need to set the file path, file name, field terminator, row terminator and the first row of data
- *Optional* - To take this one step further you could add a parameter to your stored procedure definition for the file path and file name.  Then you could build the bulk insert statement using dynamic T-SQL

b. Create the stored procedure

c.  In `Object Explorer` expand `Databases` > `RetailInsight_Case_Study` > `Progammability` > `Stored Procedures` to see your new stored procedure

d. Execute your procedure to import the csv data

e. Write a `SELECT` statement to view the data imported

### Task 8 Create dbo.DimStore
Create a table to hold store data called `dbo.DimStore` with the following specification:
- Column names and data types
```
StoreID INT – this needs to be an identity column with increment of 1 starting at 1. This column should not allow nulls.
StoreNumber SMALLINT – This column should not allow nulls
StoreName VARCHAR(50)
DateModified DATETIME
```
- Primary key clustered on `StoreID` called `PK_DimStore`

### Task 9 Create dbo.FactSales
Create a table to hold sales data called `dbo.FactSales` with the following specification:
- Column names and data types
```
DateID INT – this column should not allow nulls.
StoreID INT – this column should not allow nulls.
SalesUnits INT
```
- Primary key clustered on `DateID` and `StoreID` called `PK_FactSales`

### Task 10 Create trigger on dbo.DimStore
Create a trigger on the `dbo.DimStore` table called `tg_DimStore_Modified`, with the following specification:
- Trigger should be run on `INSERT` or `UPDATE`
- The trigger should update the `DateModified` column to `GETDATE()` for any row inserted or updated
- *hint* you will need to reference the special temporary table called `Inserted`

### Task 11 Create stored procedure dbo.InsertUpdateDimStore
Create a stored procedure called `dbo.InsertUpdateDimStore` in the `RetailInsight_Case_Study` database to insert new stores to the `dbo.DimStore` table.

a. Write the stored procedure, the procedure should do the following:
- Insert new store numbers and store names from `dbo.StageSalesData` into `dbo.DimStore`. The insert query should check if the store already exists using the column `StoreNumber`.
- *Note* that the StoreID column is an identity and will automatically be populated
- Update the `StoreName` of any existing stores, where the `StoreName` has changed

b. Create then execute the stored procedure `dbo.InsertUpdateDimStore`

c. Write a select statement on `dbo.DimStore` to see the data

### Task 12 Create stored procedure dbo.csp_InsertFactSales
Create a stored procedure called `dbo.csp_InsertFactSales` in the `RetailInsight_Case_Study` database to insert new stores to the `dbo.FactSales` table.

a. Write the stored procedure, the procedure should do the following:
- Delete any existing data in `dbo.FactSales` table for the same `TransactionDate` that you have imported into `dbo.StageSalesDataInsert`
- Insert all the sales from StageSalesData to FactSales
    - Generate the `DateID` from the `TransactionDate` as an integer in the format YYYYMMDD (e.g. 20180903)
    - Use the `dbo.DimStore` to lookup the `StoreID` for each `StoreNumber`
    - Convert the `SalesUnits` to integer

b. Create then execute the stored procedure `dbo.InsertUpdateDimStore`

c. Write a select statement on `dbo.FactSales` to see the data

## Additional tasks
If you're interested in learning more here are some additional tasks, these are provided with fewer details than in the previous tasks.

### Task 13 Create and populate dbo.DimDate
Create a `dbo.DimDate` table and a stored procedure to populate it with all 2021 dates (i.e. from 1st Jan 2021 to 31 Dec 2021)
- Needs a primary key column called `DateID` in the format YYYYMMDD
- Some additional useful attribute columns like `CalendarDate`, `DayOfWeek`, `MonthName`, `Year` etc.  Choose appropriate data types.
- For the stored procedure use a `CTE` (common table expression) or a `WHILE` loop to generate all the dates

### Task 14 De-normalise data using a view
The data in `dbo.FactSales`, `dbo.DimStore` and `dbo.DimDate` are normalised (a general practice in database design). If we were to build a report from that data we would want to show the end user the attributes that make sense to them rather than DateID and StoreID.

Create a view in the `RetailInsight_Case_Study` database that will show the `CalendarDate`, `DayOfWeek`, `StoreName`, `StoreNumber` and the `SalesUnits`.

### Task 15 Extending data loading
In task 7 you created a procedure called `dbo.csp_ImportSalesData` to import the sample data, let's now create a new stored procedure which will load all files in a given folder.

a. Create a stored procedure called `dbo.csp_CheckSalesFiles` with the following specification:
- Check a folder and create a list of files in that folder (you may need to enable `xp_cmdshell` for this, see Appendix A)
- Create a loop and execute `dbo.csp_ImportSalesData`, `dbo.csp_InsertUpdateDimStore` and `dbo.csp_InsertFactSales` for each file

b. Save `RetailInsight_CaseStudy_Data_set2.csv` to the same location you used in task 3

c. Execute `dbo.csp_CheckSalesFiles`


## Appendix A - Enable xp_cmdshell
To enable xp_cmdshell
```sql
-- To allow advanced options to be changed.
EXEC sp_configure 'show advanced options', 1;
GO
-- To update the currently configured value for advanced options.
RECONFIGURE;
GO
-- To enable the feature.
EXEC sp_configure 'xp_cmdshell', 1;
GO
-- To update the currently configured value for this feature.
RECONFIGURE;
GO
```












