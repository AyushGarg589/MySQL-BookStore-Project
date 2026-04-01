## 📚 Bookstore Data Analysis Project
## 📖 Overview
This project focuses on extracting vital business insights from a Bookstore database using MySQL. By writing complex queries, I analyzed sales trends, inventory management, and customer behavior to simulate real-world data-driven decision-making.

## 🛠️ Tech Stack
Database: MySQL

Language: SQL (Structured Query Language)

Tools: MySQL Workbench / Command Line Interface

## 🗂️ Database Structure
The project utilizes a relational database named bookstore. The schema includes tables designed to handle:

Books: Title, Author, Genre, Price, and Stock details.

Customers: Profiles and contact information.

Orders: Transaction history and linking books to buyers.

## 🚀 Key Insights Explored
Sales Performance: Identifying top-selling genres and authors.

Inventory Tracking: Monitoring stock levels to prevent shortages.

Customer Analytics: Understanding purchasing patterns and high-value clients.

## ❓ Business Questions & Solutions
  
### CREATE THE DATABASE
CREATE DATABASE OnlineBookstore;

### USE THE DATABASE 
USE  OnlineBookstore;

### DROP THE TABLE IF EXISTS PREVIOUSLY 
DROP TABLE IF EXISTS Books;

### CREATING THE TABLE --> BOOKS
CREATE TABLE Books (
    Book_ID SERIAL PRIMARY KEY,
    Title VARCHAR(100),
    Author VARCHAR(100),
    Genre VARCHAR(50),
    Published_Year INT,
    Price NUMERIC(10, 2),
    Stock INT
);

### CREATING THE TABLE CUSTOMERS 
DROP TABLE IF EXISTS customers;
CREATE TABLE Customers (
    Customer_ID SERIAL PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100),
    Phone VARCHAR(15),
    City VARCHAR(50),
    Country VARCHAR(150)
);

### CREATE A TABLE ORDERS 
DROP TABLE IF EXISTS orders;
CREATE TABLE Orders (
    Order_ID SERIAL PRIMARY KEY,
    Customer_ID INT REFERENCES Customers(Customer_ID),
    Book_ID INT REFERENCES Books(Book_ID),
    Order_Date DATE,
    Quantity INT,
    Total_Amount NUMERIC(10, 2)
);


SELECT * FROM Books;
SELECT * FROM Customers;
SELECT * FROM Orders;



### 1) Retrieve all books in the "Fiction" genre:

SELECT * 

FROM books 

WHERE genre = 'Fiction' ;

### 2) Find books published after the year 1950:

SELECT * 

FROM books 

WHERE Published_year > 1950 ;

### 3) List all customers from the Canada:

SELECT * 

FROM customers 

WHERE Country = 'Canada';

### 4) Show orders placed in November 2023:

SELECT *

FROM orders

WHERE Order_Date BETWEEN '2023-11-01' AND '2023-11-30'

ORDER BY Quantity DESC;

### 5) Retrieve the total stock of books available:

SELECT SUM(Stock) AS Book_Stock 

FROM books 

### 6) Find the details of the most expensive book:

SELECT * 

FROM books 

ORDER BY price DESC 

LIMIT 5 ;

### 7) Show all customers who ordered more than 1 quantity of a book:

SELECT c.Name , o.Quantity

FROM customers c 

INNER JOIN orders o 

USING (Customer_ID)

WHERE o.Quantity > 1 

ORDER BY Quantity DESC ; 

### 8) Retrieve all orders where the total amount exceeds $20:

SELECT * 

FROM orders

WHERE Total_Amount >= 200

ORDER BY Total_Amount DESC ,  Quantity DESC ;

### 9) List all genres available in the Books table:

SELECT DISTINCT(genre) 

FROM books ; 

### 10) Find the book with the lowest stock:

SELECT *

FROM books

ORDER BY Stock ASC

LIMIT 10 ;

### 11) Calculate the total revenue generated from all orders:

SELECT SUM(Total_Amount) AS Revenue

FROM orders ; 

## Advance Questions : 

### 1) Retrieve the total number of books sold for each genre:

SELECT b.genre , SUM(o.Quantity) AS Book_Count

FROM orders o 

JOIN books b 

USING (Book_ID)

GROUP BY b.genre

ORDER BY Book_Count DESC;


### 2) Find the average price of books in the "Fantasy" genre:

SELECT ROUND(AVG(Price),2) AS Avg_Fiction_Price

FROM books 

WHERE genre = 'Fiction';

### 3) List customers who have placed at least 2 orders:

SELECT c.Name , COUNT(o.Order_ID) AS Books 

FROM customers c 

JOIN orders o 

USING (Customer_ID) 

GROUP BY Customer_ID , c.Name

HAVING COUNT(o.Order_ID) >= 2

ORDER BY COUNT(o.Order_ID) DESC;

### 4) Find the most frequently ordered book:

SELECT o.Book_ID , b.Title , COUNT(o.Order_ID) AS Order_Count 

FROM orders o

JOIN books b

USING (Book_ID)

GROUP BY o.Book_ID , b.Title

ORDER BY Order_Count DESC ; 

### 5) Show the top 3 most expensive books of 'Fantasy' Genre :

SELECT * 

FROM books 

WHERE genre = "Fantasy" 

ORDER BY Price DESC 

LIMIT 3 ; 

### 6) Retrieve the total quantity of books sold by each author:

SELECT b.Author , COUNT(o.Order_ID) AS Order_Count 

FROM books b 

JOIN orders o 

USING (Book_ID) 

GROUP BY Author 

ORDER BY Order_Count DESC ; 

### 7) List the cities where customers who spent over $30 are located:

SELECT c.Name , c.city AS City , SUM(o.Total_Amount) AS Total_Spend 

FROM customers c 

LEFT JOIN orders o

USING (Customer_ID) 

GROUP BY c.Name , c.Customer_ID , c.City

HAVING Total_Spend >= 30 

ORDER BY Total_Spend DESC ; 

### 8) Find the customer who spent the most on orders:

SELECT c.Name , c.city AS City , SUM(o.Total_Amount) AS Total_Spend 

FROM customers c 

LEFT JOIN orders o 

USING (Customer_ID)

GROUP BY c.Name , c.Customer_ID , c.City

ORDER BY Total_Spend DESC

LIMIT 10 ; 

### 9) Calculate the stock remaining after fulfilling all orders:

SELECT b.Book_ID , b.Title , (MAX(b.Stock) - COALESCE(SUM(o.Quantity), 0)) AS Stock_Available 

FROM books b 

LEFT JOIN orders o

USING (Book_ID) 

GROUP BY b.Book_ID 

ORDER BY Stock_Available DESC; 


### 10) Check for the Outputs of Ques 9 

SELECT SUM(Quantity) 

FROM orders 

WHERE Book_ID = 40 ; 


## 📂 How to Use
### Clone the repository:
https://github.com/AyushGarg589/MySQL-BookStore-Project.git

### Import the Database:
Open MySQL Workbench. 

Run the MySQL BookStore Project.sql file to recreate the schema and data.

## Run Queries:

Copy the queries from the "Question Area" above to test the insights yourself!
