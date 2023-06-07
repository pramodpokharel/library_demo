# library_demo
Library Management System Design
Data Modelling:
1.    Identify the entities involved in the Library Management System, such as users, books, different copies of a book, authors, loans, fines, book reviews, etc, … Teams should make analysis and observation from similar systems and come up with their own designs.
2.  Define the attributes for each entity, ensuring that the data model captures all necessary information.
3.   Establish relationships between entities, considering factors like books and copies, loans and books and users, etc, ...
4.   Design an Entity-Relationship Diagram (ERD) to visually represent the entities, attributes, and relationships.
1.    Database Design:
1.    Identify primary and foreign key constraints to maintain data integrity and enforce relationships between entities.
2.   Select appropriate data types for each attribute to ensure efficient storage and retrieval of data.
3.   Write SQL statements to create the necessary tables in the database, reflecting your designed schema.
4.    Provide basic CRUD queries to create, read, update, delete data.
5. Populate the tables with representative sample data to simulate a functioning library
1.    Extra Functionalities:
1.    Retrieve all books by a specific author.
2.    Retrieve all books published in a specific year or range of years.
3.    Retrieve all the expired loans.
4.   Create fines for expired loans, and suspend user account when fine is not paid on time.
5.    Let users pay for their fines, and reopen their account if needed.
6.    Determine the total number of books available in the library.
7.    Find the average rating of books based on user reviews.
8.    Calculate the number of books by each author.
9.    Retrieve the top 10 most borrowed books.
10. Retrieve the latest books added to the library collection.
11. Retrieve all books with their corresponding author information.
12. Retrieve the details of top 10 users who have borrowed the most books.
13. Determine the number of books borrowed by users in a specific age range.
14. Reserve and return copies.
Deliverables:
1.  Entity-Relationship Diagram (ERD) representing the relationships between entities and attributes in the Library Management System.
2.     Normalized relational database schema for the Library Management System.
3.   SQL scripts or queries used to create tables and populate them with sample data in PostgreSQL.
4. Documentation describing the indexing strategy, including the selected columns and their impact on query performance.
5.   Documentation describing the transaction design, scenarios tested, and the observed behavior of transactions.
 



DESCRIPTION OF TABLES

books
book_id		primary key, serial
title			varchar(1000)
isbn			bigint, unique, not null
publication_year	int
publisher		varchar(200)
nr_of_pages		int
summary		varchar(2000)
category_id		smallint

NOTES:
Author is not an attribute, because books can have multiple authors. To get it in ‘normalized’ form, I think we need a separate table that links books with authors
ISBN is a 10-digit number so should fit into a bigint (varchar(10) is also an option, but probably slower in querying than bigint)

users
user_id		primary key, serial
first_name		varchar(200)
last_name		varchar(200)
phone_nr		varchar(20)
membership_expiry	date
join_date		date
email 			varchar(200)

authors
author_id		primary key, serial
first_name		varchar(200)
last_name		varchar(200)

copies
copy_id: primary key, serial
book_id: foreign key referencing books(book_id)
available: boolean
Notes:
The "copies" table is used to keep track of individual copies of books available in the library.
Each copy is uniquely identified by the "copy_id" column, which serves as the primary key.
The "book_id" column is a foreign key referencing the "book_id" column in the "books" table. It establishes a relationship between copies and books, indicating which book a specific copy belongs to.
The "available" column is a boolean field that denotes whether a copy is currently available for loan. It helps to determine the availability status of a copy in the library.

loans
loan_id			primary key, serial
user_id		foreign key, users(user_id)
book_id		foreign key, books(book_id)
loan_start_date	date
loan_end_date	date
return_date		date 

NOTES: 
I assume that this table is meant to keep a register of who is borrowing which book
Books can be returned after the due date, which is why we need separate loan_end_date and return_date

fines
fine_id			primary key, serial
loan_id			foreign key, loans(loan_id)
fine_amount		int   
issue_date		date
payment_date		date
paid			bit 

NOTES:
Each fines is associated with a specific loan; 1-to-1 relationship (each loan can have at most 1 fine; and each fine is associated with exactly 1 loan)
FineAmount is the amount of fine in pennies/cents (or whatever the smallest discrete unit of the currency is) – alternative is ‘decimal’, but that we might want to avoid whenever possible (i imagine that int queries may be a tiny little bit faster than decimal queries)
“Paid” indicates if the fine has been paid (a boolean; in MS SQL it is a ‘bit’; not sure if postgresql uses the same name)

book_reviews
review_id		primary key, serial
book_id		foreign key, books(book_id)
user_id		foreign key, users(user_id)
rating			smallint
review			varchar(max)
review_date		date
NOTES:
Each review is associated with exactly 1 book
Each book can have multiple reviews

book_author_link
book_id		int
Author_id		int
CONSTRAINT		primary key on the pair book_id, author_id 
Notes: 
Many-to-many relationship
not sure about the primary key



Entity-Relationship Diagram (ERD) representing the relationships between entities and attributes in the Library Management System.

Entity-Relationship Diagram (ERD) 1:

https://dbdiagram.io/d/646f60577764f72fcfd6a99d



Entity-Relationship Diagram (ERD) 2:

https://dbdiagram.io/d/6474027a7764f72fcffeee20



Deliverable 2.    Normalized relational database schema for the Library Management System.

The normalized schema for the library management system:

https://dbdiagram.io/d/6473f7b37764f72fcffe982e


