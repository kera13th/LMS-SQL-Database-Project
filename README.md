#### DSA SQL MASTERCLASS PROJECT BY KAREN SANTIAGO
## Library Management System Database Documentation

### 1. Introduction
This document provides an overview and documentation of the Library Management System (LMS) database. The LMS is designed to manage the library's collection of books, patrons, book issuance, and related activities.

### 2. Database Schema

The LMS database follows a relational database model and is composed of several tables connected through relationships. The primary entities include:
+	Book
+	Patron
+	Membership
+	Book Issuance (checkouts, returns)
+	Genre (book genres)

```
-- Create Database
CREATE DATABASE DSALibrary;

USE DSALibrary;

-- Create Book Table
CREATE TABLE Book (
BookID BIGINT IDENTITY(1,1), --will automatically generate numbering for bookID
Title NVARCHAR(350) NOT NULL,
Author NVARCHAR(300) NOT NULL,
isbn NVARCHAR(30) NOT NULL,
GenreID BIGINT NOT NULL,
LangCode NVARCHAR(30),
PageNum BIGINT,
PublicationDate NVARCHAR(300),
Publisher NVARCHAR(300),
Copies INT
);

-- Create Genre Table
CREATE TABLE Genre (
GenreID BIGINT NOT NULL,
GenreName NVARCHAR(60)
);

CREATE TABLE Membership (
MembershipID NVARCHAR(100) NOT NULL,
MembershipType NVARCHAR(50) NOT NULL,
);

-- Create Patron Table
CREATE TABLE Patron (
PatronID BIGINT IDENTITY(1,1) NOT NULL,
MembershipID INT DEFAULT 1,
FirstName NVARCHAR(50) NOT NULL,
Middlename NVARCHAR(50),
LastName NVARCHAR(50) NOT NULL,
BirthDate DATETIME,
Gender NVARCHAR(1),
EmailAddress NVARCHAR(40),
Education NVARCHAR(40),
Occupation NVARCHAR(100),
AddressLine1 NVARCHAR(120),
AddressLine2 NVARCHAR(120), 
);

-- Create BookIssuance Table
CREATE TABLE BookIssuance (
TransactionID BIGINT IDENTITY(1,1) NOT NULL,
CheckoutOrderID NVARCHAR(20) NOT NULL,
BookID BIGINT,
PatronID BIGINT,
CheckoutDate DATETIME NULL,
DueDate DATETIME NULL,
ReturnDate DATETIME NULL,
);

-- SETTING PRIMARY KEYS FOR BOOK TABLE --
ALTER TABLE Book
ADD CONSTRAINT PK_Book_BookID PRIMARY KEY (BookID);

ALTER TABLE BookIssuance
ADD CONSTRAINT PK_BookIssuance_TransactionID PRIMARY KEY (TransactionID);

ALTER TABLE Genre
ADD CONSTRAINT PK_Genre_GenreID  PRIMARY KEY (GenreID);

ALTER TABLE Membership
ADD CONSTRAINT PK_Membership_MembershipID PRIMARY KEY (MembershipID);

ALTER TABLE Patron
ADD CONSTRAINT PK_Patron_PatronID PRIMARY KEY (PatronID);

-- SETTING FOREIGN KEYS --

ALTER TABLE Book
ADD CONSTRAINT FK_Book_GenreID
FOREIGN KEY (GenreID)
REFERENCES Genre(GenreID);

ALTER TABLE BookIssuance
ADD CONSTRAINT FK_BookIssuance_PatronID
FOREIGN KEY (PatronID)
REFERENCES Patron (PatronID);

ALTER TABLE BookIssuance
ADD CONSTRAINT FK_BookIssuance_BookID
FOREIGN KEY (BookID)
REFERENCES Book (BookID);

ALTER TABLE Patron
ADD CONSTRAINT FK_Patron_MembershipID
FOREIGN KEY (MembershipID)
REFERENCES Membership (MembershipID);
```

### 3. Entity-Relationship Diagram (ERD)

![LMS Entity Relationship Diagram](https://drive.google.com/uc?export=download&id=176DjE3y2ZX6svhTsPrgruq2U-HOAlZkF)

### 4. Tables and Descriptions

#### Book Table

| **Column**       | **Data Type**       | **Description**                                     | **Constraints**                |
|------------------|---------------------|-----------------------------------------------------|--------------------------------|
| BookID           | BIGINT              | Unique identifier for books.                        | IDENTITY(1,1)                  |
| Title            | NVARCHAR(350)       | The title of the book.                             | NOT NULL                       |
| Author           | NVARCHAR(300)       | The author of the book.                            | NOT NULL                       |
| isbn             | NVARCHAR(30)        | The International Standard Book Number (ISBN).     | NOT NULL                       |
| GenreID          | BIGINT              | Unique identifier for the genre of the book.        | NOT NULL                       |
| LangCode         | NVARCHAR(30)        | The language code of the book.                     |                                |
| PageNum          | BIGINT              | The number of pages in the book.                   |                                |
| PublicationDate  | NVARCHAR(300)       | The publication date of the book.                 |                                |
| Publisher        | NVARCHAR(300)       | The publisher of the book.                        |                                |
| Copies           | INT                 | The number of copies available for the book.       |                                |

#### Genre Table

| **Column**   | **Data Type**  | **Description**                 | **Constraints**  |
|--------------|----------------|---------------------------------|------------------|
| GenreID      | BIGINT         | Unique identifier for genres.    | NOT NULL         |
| GenreName    | NVARCHAR(60)   | Name of the genre.              |                  |

#### Membership Table

| **Column**     | **Data Type**  | **Description**                     | **Constraints**  |
|----------------|----------------|-------------------------------------|------------------|
| MembershipID   | NVARCHAR(100) | Unique identifier for memberships.   | NOT NULL         |
| MembershipType | NVARCHAR(50)  | Type of membership.                 | NOT NULL         |

#### Patron Table

| **Column**    | **Data Type**   | **Description**                      | **Constraints**                |
|---------------|-----------------|--------------------------------------|--------------------------------|
| PatronID      | BIGINT          | Unique identifier for patrons.        | IDENTITY(1,1)                  |
| MembershipID  | INT             | Membership ID of the patron.         | DEFAULT 1                       |
| FirstName     | NVARCHAR(50)    | First name of the patron.            | NOT NULL                       |
| Middlename    | NVARCHAR(50)    | Middle name of the patron.           |                                |
| LastName      | NVARCHAR(50)    | Last name of the patron.             | NOT NULL                       |
| BirthDate     | DATETIME        | Birth date of the patron.            |                                |
| Gender        | NVARCHAR(1)     | Gender of the patron.               |                                |
| EmailAddress  | NVARCHAR(40)    | Email address of the patron.        |                                |
| Education     | NVARCHAR(40)    | Education level of the patron.      |                                |
| Occupation    | NVARCHAR(100)   | Occupation of the patron.           |                                |
| AddressLine1  | NVARCHAR(120)   | Address Line 1 of the patron.       |                                |
| AddressLine2  | NVARCHAR(120)   | Address Line 2 of the patron.       |                                |

#### BookIssuance Table

| **Column**        | **Data Type**  | **Description**                          | **Constraints**                |
|-------------------|----------------|------------------------------------------|--------------------------------|
| TransactionID     | BIGINT         | Unique identifier for transactions.      | IDENTITY(1,1)                  |
| CheckoutOrderID   | NVARCHAR(20)   | Order ID for checkout.                   | NOT NULL                       |
| BookID            | BIGINT         | Identifier for books.                    |                                |
| PatronID          | BIGINT         | Identifier for patrons.                  |                                |
| CheckoutDate      | DATETIME       | Date of checkout.                        |                                |
| DueDate           | DATETIME       | Due date for return.                     |                                |
| ReturnDate        | DATETIME       | Date of return.                          |                                |

Records transactions related to book checkouts and returns.
Fields include transaction ID, book ID, patron ID, checkout date, return date.


### 5. Populate Database

#### :minidisc: Populating Book Table
Dataset: [Kaggle - Book Recommendation Dataset](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset)

I accomplished the task of populating the "Book" table by retrieving data from an established dataset acquired from Kaggle. Afterward, I carefully reorganized the columns to make sure they smoothly fit into the LMS (Library Management System) Database's structure, making sure they match the needed columns accurately.

To facilitate this data transformation, I opted for MS Excel as my tool of choice. Within Excel, I meticulously edited a CSV file, making necessary adjustments to the dataset. To enhance the richness and diversity of the data, I employed Excel's formula functionality to generate randomized entries for specific columns, thereby enriching the dataset with pertinent information.

```
=INDEX(data!A:A, RANDBETWEEN(2,COUNTA(data!A:A)))
```

Testing the table if populated properly

![Book Table Screenshot](https://drive.google.com/uc?export=download&id=17H5BP9egBqctM7LvRCuB6eLlwC0s2OHm)

#### :minidisc: Populating Patron Table
Dataset: [AdventureWorks2022](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2022.bak)

To populate the Patron and Book Issuance tables, I leveraged the AdventureWorks2022 dataset for the Patron (User) entries and the FactInternetSales table for the Book Issuance Table. This approach allowed me to import and integrate relevant data from these sources, ensuring the accuracy and completeness of the information contained within these tables.

```
-- Insert data from specific columns in SourceTable into DestinationTable
INSERT INTO Patron (FirstName, MiddleName, LastName, BirthDate, Gender, EmailAddress, Education,
Occupation, AddressLine1, AddressLine2) -- Replace Column1, Column2, Column3 with your specific column names
SELECT FirstName, MiddleName, LastName, BirthDate, Gender, EmailAddress, Education,
Occupation, AddressLine1, AddressLine2 -- Replace with the actual column names you want to copy
FROM AdventureWorksDW2022.dbo.ProspectiveBuyer; -- Replace SourceTable with the name of your source table in SourceDB
```
Testing the table if populated properly

![Patron Table Screenshot](https://drive.google.com/uc?export=download&id=17H5BP9egBqctM7LvRCuB6eLlwC0s2OHm)
 
#### :minidisc: Populating BookIssuance Table
![BookIssuance Table Screenshot](https://drive.google.com/uc?export=download&id=18AzxFAjxQGtxm5Foq4nr_m7oSls1h2Ss)

#### :minidisc: Populating Membership Table

```
INSERT INTO Membership (MembershipID, MembershipType)
VALUES
(1,'Student'),
(2,'Library Staff'),
(3,'Librarian'),
(4,'Database Admin');
```
![Membership Table Screenshot](https://drive.google.com/uc?export=download&id=17Tx-EjiSkyB_yjQayyRNLyPTewwHLozU)

#### :minidisc: Populating Genre Table

```
INSERT INTO Genre (GenreID,GenreName)
VALUES
(1,'Action/adventure Fiction'),
(2,'Children’s Fiction'),
(3,'Classic Fiction')...
```
![Genre Table Screenshot](https://drive.google.com/uc?export=download&id=17Sl3Na8nCvZ2Abc-OPO3J309nHijB3BA)


##### For complete SQL Code used, please refer to the link:
[Populate Tables Script](https://github.com/kera13th/projects/edit/LMS-Database/populatetables_script)

### 6. Sample Queries
###### 1. Checking who is the librarian of DSA Library
```
SELECT
    PatronID,
    FirstName,
    LastName,
    MembershipType
FROM
    Patron p
JOIN
    Membership m ON p.MembershipID = m.MembershipID
WHERE
    MembershipType = 'Librarian';
```
###### Query Result:
```
+---------+---------+----------+----------------+
| PatronID| FirstName| LastName| MembershipType |
+---------+---------+----------+----------------+
|   101   |  Adrian |  Murphy  |   Librarian    |
+---------+---------+----------+----------------+
```

###### 2. Total Borrows Categorized by Occupation for Year 2014
```
SELECT
    Occupation,
    COUNT(*) AS CheckOutsForYr2014
FROM
    BookIssuance bi
JOIN
    Patron p ON p.PatronID = bi.PatronID
WHERE
    YEAR(CheckoutDate) = 2014 AND Occupation <> 'Database Admin'
GROUP BY
    Occupation;
```
###### Output

```
+--------------+-------------------+
|  Occupation  | CheckOutsForYr2014 |
+--------------+-------------------+
| Professional |        557        |
|   Clerical   |        320        |
|    Manual    |        247        |
|  Management  |        364        |
|  Librarian   |         1         |
| Skilled Manual|       480        |
+--------------+-------------------+
```
###### 2. Querying the TOP 5 most borrowed books
```
SELECT TOP 5
    Title,
    Author,
    GenreName,
    COUNT(*) AS TimesBorrowed
FROM
    BookIssuance bi
JOIN
    Book b ON bi.BookID = b.BookID
JOIN
    Genre g ON g.GenreID = b.GenreID
GROUP BY
    Title,
    GenreName,
    Author;
```
###### Output
```
+---------------------------------------------------+---------------------------------------------------+-----------------------+---------------+
| Title                                             | Author                                            | GenreName             | TimesBorrowed |
+---------------------------------------------------+---------------------------------------------------+-----------------------+---------------+
| 100 Years of Lynchings                            | Ralph Ginzburg                                    | Classic Fiction       | 234           |
| 1776                                              | David McCullough                                 | Biography             | 216           |
| 1776                                              | Peter Stone/Sherman Edwards                     | Biography             | 232           |
| A Changeling for All Seasons (Changeling Seasons #1) | Angela Knight/Sahara Kelly/Judy Mays/Marteeka Karland/Kate Douglas/Shelby Morgen/Lacey Savage/Kate Hill/Willa Okati | Western | 231           |
| A Killing Rain (Louis Kincaid  #6)                | P.J. Parrish                                     | Religion & Spirituality | 248           |
+---------------------------------------------------+---------------------------------------------------+-----------------------+---------------+


```



## 7. User Roles and Permissions
#### 7.1. Create Roles

Three roles are created: librarian, librarystaff, and member. These roles are often used to group users based on their roles or responsibilities within the database.

#### 7.2. Add Users

User logins are created for different users who will access the database. For example, lmsadmin, dsalibrarymember, and dsalibrarystaff logins are created. Corresponding database users (lmsadminuser, dsalibrarymemberuser, and dsalmsstaffuser) are created and associated with these logins.

```
-- Add users
USE DSALibrary;
CREATE LOGIN lmsadmin WITH PASSWORD = 'lmsadmin';
CREATE USER lmsadminuser FOR LOGIN lmsadmin;

CREATE LOGIN dsalibrarymember WITH PASSWORD = 'dsalibrarymember';
CREATE USER dsalibrarymemberuser FOR LOGIN dsalibrarymember;

CREATE LOGIN dsalibrarystaff WITH PASSWORD = 'dsalibrarystaff';
CREATE USER dsalmsstaffuser FOR LOGIN dsalibrarystaff;
```

![Screenshot of Users](https://drive.google.com/uc?export=download&id=18fW3Q_rZr9mun87yodM8db3Cd-gAdGFA)

#### 7.3. Adding Users to Role:

Users are added to the appropriate roles. For instance, lmsadminuser is added to the librarian role, dsalibrarymemberuser is added to the member role, and dsalmsstaffuser is added to the librarystaff role.
Setting Privileges for Member Role:

Members (likely students or general users) are granted SELECT privileges on the Book and Genre tables. This means they can only read data from these tables.

```
-- CREATE ROLES -- 
USE DSALibrary;
CREATE ROLE librarian;
CREATE ROLE librarystaff;
CREATE ROLE member;

-- Adding users to role
USE DSALibrary;
EXEC sp_addrolemember 'librarian', 'lmsadminuser';
EXEC sp_addrolemember 'member', 'dsalibrarymemberuser';
EXEC sp_addrolemember 'librarystaff', 'dsalmsstaffuser';
```

#### 7.4. Setting Privileges for Staff:

Library staff members are granted more extensive privileges. They are given SELECT, INSERT, UPDATE, and DELETE privileges on the Book table, SELECT, INSERT, and UPDATE privileges on the BookIssuance table, and SELECT privileges on the database schema.

#### 7.5. Setting Privileges for Librarian:

Librarians are granted extensive privileges, including SELECT, INSERT, UPDATE, and DELETE permissions on all tables in the schema, EXECUTE permissions on all stored procedures and functions in the database, VIEW DEFINITION permission to see object metadata, and additional SELECT, INSERT, UPDATE, and DELETE permissions on the entire database.
This script essentially sets up a role-based access control system for the database, allowing different levels of access and permissions based on user roles. It's a common practice to ensure that users only have the necessary privileges to perform their specific tasks within the database while maintaining security and data integrity.

```
-- Setting privilege for member role/for students
GRANT SELECT ON Book TO member;
GRANT SELECT ON Genre TO member;

-- Setting privilege for staff
GRANT SELECT, INSERT, UPDATE, DELETE ON Book TO librarystaff;
GRANT SELECT, INSERT, UPDATE ON BookIssuance TO librarystaff; 
GRANT SELECT ON SCHEMA::dbo TO librarystaff;

-- Setting privilege for librarian
-- Grant SELECT, INSERT, UPDATE, DELETE on all tables in the schema
GRANT SELECT, INSERT, UPDATE, DELETE ON SCHEMA::dbo TO librarian;
-- Grant EXECUTE permission on all stored procedures and functions
GRANT EXECUTE ON DATABASE::DSALibrary TO librarian;
-- Grant VIEW DEFINITION permission (ability to see object metadata)
GRANT VIEW DEFINITION TO librarian;
GRANT SELECT, INSERT, UPDATE, DELETE ON DATABASE::DSALibrary TO librarian;
```

Simulate User Interactions
Librarian User: 1. Adding new member in Patron table (left image), 2. Querying the newly added member
![Librarian Access](https://drive.google.com/uc?export=download&id=18Ouqan0HE3aK9iF4la9RO9FGJQ-_bNvj)

Staff User: Adding new book entry on Book Table
![Staff Access](https://drive.google.com/uc?export=download&id=18WqHJz4KUvgXuaPOOfblEuEWI-EcEuWW)

Member User: 1. Querying Patron and Book Issuance Table (left image), 2. Querying book Info (right)
![Member Access](https://drive.google.com/uc?export=download&id=18GIdbh7NDTS6DxYgnoZCJsWLdPDRbEa0)

### 8. Power BI Dashboard 

![LMS Dashboard](https://drive.google.com/uc?export=download&id=18cmhYnS_Y3P7tJQBjzyBuz7rt8EAYcXj)

This comprehensive guide provides an in-depth overview of the dashboard's functionalities and features, designed to empower library professionals and administrators with efficient tools for library management and data-driven decision-making. 
#### Data Source:

The data utilized in this dashboard is sourced directly from the SQL database meticulously crafted for our library management system. Leveraging the capabilities of Power BI, this dashboard connects to the database using the Direct Query mode, ensuring that you have access to real-time, up-to-date information.

#### Key Features:

1. Insights:

Analyze borrowing patterns and historical trends.
Explore membership trends, types, and demographics.

2. Book Analytics:
Discover insights into our library's extensive book collection.
Identify the how many books were borrowed, Identify overdue records, and total number of members.

3. Book Issuance Activity:

Monitor book issuance activities within the library.
Gain insights into user behavior and preferences.

4. Overdue Activity:

Monitor and track loaned books that are past due date.


### 9. Revision History
Version 1.0 (10/01/2023): Initial database design.

This documentation serves as a reference for those involved in managing and using the Library Management System database. It provides insights into the database's structure, relationships, and usage, helping ensure the efficient operation of the library system.
