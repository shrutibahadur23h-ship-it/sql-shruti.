# sql-shruti.
Project Goal & Objective

This project consists of 10 key tasks that collectively provide a comprehensive understanding of SQL. It covers the fundamental concepts such as DDL, DML, and DQL and also demonstrates how SQL is applied in real-world business scenarios

Req1
Generate a random data input files a) Feed-1 which has 10 columns with 10 rows, b) Feed-2 which has 15 columns with 15 rows , c) Feed-3 which has 20 columns with 20 rows

Req2 Automate the Req 1 input file generation using SQL scripts and the parameter will be "Feed name" & Number of Rows to populate Data

Req3 Write SQL script to identify the duplicate (rows) in each of the table Feed-1, 2, 3

Req4 Write the duplicate records in output file - "duplicates"

Req5 Create a script to replace all the duplicates with Unique rows and update back to respective Feed table

Req6 Execute the duplicate script and check the output is zero

Req7 Create SQL script to compare data from Feed-2,3 to Feed-1 and write in output file on the compared results

Req8 Create Test plan with all kinds of manual test cases in order to test this End to End functionality

Req9 Automate the test cases (if possible) using any method but should be automated…

Req 10 Document everything in word with all screen grabs as proper Project Document

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------Below are the codes for the assignment given:-
USE FEED1 ;
CREATE TABLE Feed_1 (
    ID INT,
    Name VARCHAR(50),
    Age INT,
    Email VARCHAR(100),
    Salary DECIMAL(10,2),
    Dept VARCHAR(20),
    JoiningDate DATE,
    City VARCHAR(50),
    Status CHAR(1),
    Code VARCHAR(10)
);



INSERT INTO Feed1 (Name, Age, Email, Salary, Dept, JoiningDate, City, Status, Code)
SELECT 
    SUBSTRING(MD5(RAND()), 1, 8), 
    FLOOR(RAND() * 50) + 20,
    CONCAT(SUBSTRING(MD5(RAND()), 1, 5), '@mail.com'),
    ROUND(RAND() * 100000, 2),
    'IT',
    CURDATE(),
    'CityA',
    'A',
    SUBSTRING(MD5(RAND()), 1, 5)
FROM information_schema.tables
WHERE table_name IS NOT NULL
LIMIT 100;

CREATE TABLE Feed2 (
    ID INT,
    Name VARCHAR(50),
    Age INT,
    Email VARCHAR(100),
    Salary DECIMAL(10,2),
    Dept VARCHAR(20),
    JoiningDate DATE,
    City VARCHAR(50),
    Status CHAR(1),
    Code VARCHAR(10)
);


INSERT INTO Feed2 (Name, Age, Email, Salary, Dept, JoiningDate, City, Status, Code)
SELECT 
    SUBSTRING(MD5(RAND()), 1, 8), 
    FLOOR(RAND() * 50) + 20,
    CONCAT(SUBSTRING(MD5(RAND()), 1, 5), '@mail.com'),
    ROUND(RAND() * 100000, 2),
    'IT',
    CURDATE(),
    'CityA',
    'A',
    SUBSTRING(MD5(RAND()), 1, 5)
FROM information_schema.tables
WHERE table_name IS NOT NULL
LIMIT 100;

CREATE TABLE Feed3 (
    ID INT,
    Name VARCHAR(50),
    Age INT,
    Email VARCHAR(100),
    Salary DECIMAL(10,2),
    Dept VARCHAR(20),
    JoiningDate DATE,
    City VARCHAR(50),
    Status CHAR(1),
    Code VARCHAR(10)
);



INSERT INTO Feed3 (Name, Age, Email, Salary, Dept, JoiningDate, City, Status, Code)
SELECT 
    SUBSTRING(MD5(RAND()), 1, 8), 
    FLOOR(RAND() * 50) + 20,
    CONCAT(SUBSTRING(MD5(RAND()), 1, 5), '@mail.com'),
    ROUND(RAND() * 100000, 2),
    'IT',
    CURDATE(),
    'CityA',
    'A',
    SUBSTRING(MD5(RAND()), 1, 5)
FROM information_schema.tables
WHERE table_name IS NOT NULL
LIMIT 100;


DELIMITER $$

CREATE PROCEDURE GenerateFeedData (
    IN FeedName VARCHAR(20),
    IN RowCount INT
)
BEGIN
    DECLARE SQLQuery LONGTEXT;

    IF FeedName = 'Feed1' THEN
        SET SQLQuery = CONCAT(
            'INSERT INTO Feed1 (Name, Age, Email, Salary, Dept, JoiningDate, City, Status, Code) ',
            'SELECT Name, Age, Email, Salary, Dept, JoiningDate, City, Status, Code FROM Feed2 ',
            'ORDER BY JoiningDate DESC LIMIT ', RowCount
        );
    ELSEIF FeedName = 'Feed2' THEN
        SET SQLQuery = CONCAT(
            'INSERT INTO Feed2 (Name, Age, Email, Salary, Dept, JoiningDate, City, Status, Code) ',
            'SELECT Name, Age, Email, Salary, Dept, JoiningDate, City, Status, Code FROM Feed3 ',
            'ORDER BY JoiningDate DESC LIMIT ', RowCount
        );
    ELSEIF FeedName = 'Feed3' THEN
        SET SQLQuery = CONCAT(
            'INSERT INTO Feed3 (Name, Age, Email, Salary, Dept, JoiningDate, City, Status, Code) ',
            'SELECT Name, Age, Email, Salary, Dept, JoiningDate, City, Status, Code FROM Feed1 ',
            'ORDER BY JoiningDate DESC LIMIT ', RowCount
        );
    ELSE
        BEGIN
            SIGNAL SQLSTATE '45000'
            SET MESSAGE_TEXT = 'Invalid FeedName provided.';
        END;
    END IF;

    IF SQLQuery IS NOT NULL THEN
        PREPARE stmt FROM SQLQuery;
        EXECUTE stmt;
        DEALLOCATE PREPARE stmt;
    END IF;
END$$

DELIMITER ;



 




