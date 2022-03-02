# Relational Database Management System

# Overview
With exponential increase of data in the day to day, the need for sophisticated and fully functional databases is greater than ever. There has been a great demand in not just storing the data but instead storing data in ways that would make sense by creating appropriate relationships. These kinds of databases are nothing but relational databases. Now, we as a team intend to work on creating a database that aims to deliver a light- weight relational database management system that uses custom data structures with the help of arrays, trees, linked lists, hash maps etc., while also providing means to establish a file system that has persistent storage and holds ACID properties.

# Features

## 1. User Authentication and Registration
To enhance the Database security and to allow access of only registered users within the database a user authentication layer is created. Before proceeding with the user authentication, the user must first register on to the system.  
For registration purposes we are taking an input from the user seeking the desired username and password. These details, once fetched from the user will be stored in a text file “userLoginDetails.txt” located in the root directory of the project.  
For further security the password is stored in an encrypted format using **AES encryption algorithm**. This will ensure that anyone prying into the system will have no idea about what the password is. Below is a sample picture for the registration process and the snippet of the “userLoginDetails.txt” file.
<table>
  <tr>
    <td><b>a. Registration Flow</b></td>
    <td><b>b. Password Encryption</b></td>
  </tr>
  <tr>
    <td><img src="https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/1.png" width=470 height=400></td>
    <td><img src="https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/2.png" width=470 height=400></td>
  </tr>
</table>

Once the user is successfully registered, the user is automatically logged into the system. If the user chooses to login later, the user can simply exit out of the system and proceed to login again with the details given at the time of registration. At the time of login, the user is prompted to enter the username and password after which the password is then decrypted. if the details are a match, then the user is signed into the system and is greeted with various options to perform on the database. Below is a snippet showing the successful login case as well as an unsuccessful login case.
<table>
  <tr>
    <td><b>a. Successful Login</b></td>
    <td><b>b. Unsuccessful Login</b></td>
  </tr>
  <tr>
    <td><img src="https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/3.png" width=470 height=400></td>
    <td><img src="https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/4.png" width=470 height=400></td>
  </tr>
</table>

## 2. Database Operations
### a. Global Data Dictionary
Global data dictionary will store the meta data of all the tables. It will store information like Table name, Column name, column data type, constraints of a column, primary key field, Foreign key Constraint. Data stored inside it will help us in performing **Primary Key validation** as we will fetch primary key of a particular table from data dictionary while inserting data inside that table. It will further help us in building **ERD (Entity Relation Diagram)** as it holds all the information of a table like, which one is primary key, what all constraints are there of each column, and foreign key is referenced from which table.  
Global data dictionary will be created for the first time only i.e., when we will create our first table. For subsequent tables new data dictionary will not be created rather the meta data of new tables will be appended inside the same data dictionary. The format in which we are writing meta data of tables inside Data dictionary is written inside createDataDictionary() function inside Create.java class inside queryParser Package. We are also maintaining query logs along with this operation.
![image](https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/5.png)

### b. CREATE
Create operation will help us in Creating Tables in our Database. First, we will write SQL query for CREATE operation which will be passed to CreateValidator class where we have REGEX for create query, which will check if query is correct or not. If entered query is correct it will be passed to createParser() method where we will fetch table name, column names, data types, and constraint from groups of REGEX. Once we will get them, we will pass them to create() method which will Create a new tablename.txt file of that table and metadata of that table will also be appended to Data Dictionary. This method will also validate whether, if a table is already present and user is second time creating table with same name. In such a case new table will not be created.
![image](https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/6.png)

### c. INSERT 
Insert operation will help us in writing data inside tables which we created using CREATE operation. First it will validate if table exists in our database or not. If table does not exist than it will throw an error and will not allow us to write data in any other table present in our database. Once it finds a table, we are fetching primary key of that table from Meta Data (Data dictionary) file to perform primary key validation. INSERT operation won’t allow insertion of duplicate primary key. The format in which we are displaying inserted values in our table is written inside insertParser() method of Insert.java class. The flow of Insertion is also same as to Create operation. First, query will be sent to validator method of Insert operation. It will validate query by checking REGEX, thereafter that query will be parsed to insertParser method to fetch column names and column values, and thereafter we are Inserting data in table by performing primary key validation. Parallelly, we are maintaining query logs as well for this transaction.
![image](https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/7.png)

### d. SELECT
The SELECT statement in SQL is used to retrieve rows from our database. Whenever we insert rows into our database, we can retreive them using the SELECT command. It offers various conditional retreival options so that our user would be able to get only the desired data based on a confition. Our java program will first validate the SELECT query based on regex and then will call different four functions based on the query as follows:

#### i. If the query contains * (select all rows) without any condition.
It simply calls the function getAllRows() with a “None” for the where condition query. The function prints all rows from the table.
![image](https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/9.png)

#### ii. If the query contains * (select all rows) with a where condition.
It calls the function getAllRows() with the where condition and the code executes accordingly, which would be explained in the pseudo code below. This gets all the columns with matched rows.
![image](https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/10.png)

#### iii. If the query contains specific column names without a where condition.
It calls the function getSpecificRows() with passing the where condition as “None”. It gets all the matching columns in all rows.
![image](https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/11.png)

#### iv. If the query contains specific column names with a where condition.
It calls the function getSpecificRows() but now it does pass the where condition as a parameter to the function.
![image](https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/12.png)

### e. UPDATE
The UPDATE statement in SQL is used to update record/records in our SQL database. It allows us to update any record by passing a condition that can identify the record which is to be updated. Our java program imitates this update statement by traversing through the database for finding the record to be updated. Once it is found it updates the value and stores it. We are storing the content in a two dimensional arraylist and once all records are updated we write back the arraylist into the file.
<table>
  <tr>
    <td><b>a. Original Records</b></td>
    <td><b>b. Query Execution</b></td>
    <td><b>c. Records after Query Execution</b></td>
  </tr>
  <tr>
    <td><img src="https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/13.png" width=400 height=300></td>
    <td><img src="https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/14.png" width=400 height=300></td>
    <td><img src="https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/15.png" width=400 height=300></td>
  </tr>
</table>

### f. DELETE
The DELETE statement is used to delete specific records from our database. It works same as update statement, its just that it deletes the record while the other one updates a record. So the working of our java program is very much similar to update statement. It finds the record , deletes the record and replaces the content in the file.
<table>
  <tr>
    <td><b>a. Original Records</b></td>
    <td><b>b. Query Execution</b></td>
    <td><b>c. Records after Query Execution</b></td>
  </tr>
  <tr>
    <td><img src="https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/15.png" width=400 height=300></td>
    <td><img src="https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/16.png" width=400 height=300></td>
    <td><img src="https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/17.png" width=400 height=300></td>
  </tr>
</table>

### g. DROP
Drop operation will help us in deleting entire table from our database. It will also delete meta data of that table from our Data Dictionary. First it will validate if table exists in our database or not. If table does not exist than it will throw an error saying “selected table cannot be dropped” else, it will delete that table completely from our database. The flow of Drop operation is as follows, it will first, send the query to validator method of DROP operation. It will validate query by checking REGEX, thereafter that query will be passed to insertParser method to fetch table name which needs to be deleted. Upon fetching table name we will call dropDDTable() and dropTable() methods inside Drop.java class to drop table from database and its details from meta data file.
![image](https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/8.png)

## 3. Transactions
To perform a set of Database queries in a single go we have implemented Transaction module. In main class we have an option to perform transaction. Inside transaction module we have created an ArrayList which will hold number of SQL queries which will be executed in that transaction. Once all the queries are added in ArrayList, we will iterate through array list and hit a particular case mentioned in DBOperations class for each query. When at last user will write COMMIT keyword all the queries within that transaction will get executed. Parallelly, we have also maintained logs for all these SQL queries executed in a single Transaction.
![image](https://github.com/ManjinderSingh3/Relational-Database-Management-System/blob/master/screenshots/18.png)

## 4. Locking
For all the Write operations within our Database we have maintained locks on respective Table where operation takes place. As soon as user start performing an operation on a table, it gets locked, and when operation is completed, lock gets released from that table so that other operations can be performed on that table. We have created a separate Locks.java class in which there are three methods. First method, checkLock() will return a Boolean value if a table is locked or not. If table is not locked, then we will lock it so that someone else should not have access to same table on which user is performing operation. In this case, we will call setLock() method to impose lock on that table. Once lock gets imposed than user will perform operation. Upon successful operation at last we will call removeLock() method to release lock from that table. The structure of our Table_Locks.txt file is Username TableName and LockStatus. If lock status is false than it means an operation can be performed on that table, else, if it is true than no other operations can be performed on that table as it is currently locked.
