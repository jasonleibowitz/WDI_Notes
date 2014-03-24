#Day 9 | Week 2 Day 4 Notes

March 6, 2014

---

### AM Learning Objectives

* Explain the concept of **CRUD**
* State the *relationships* that can exist between entities
* Describe the components of an **Entity Relationship Diagram** (ERD)
* Construct an ERD for a given domain model

---


### CRUD

* If you're working with some information stored in a database, these are really the only four things you ever do to the data.
	* Create
	* Read
	* Update
	* Destroy
* When we start making web apps, we'll be talking in terms of this. 
	* Different frameworks have different ways of doing this. 
	* There is a general convention, however, that we will follow. 
* The concept of CRUD is a generic philosophy and not limited to databases

** Example **

* Update WDI database schema to include customers
	* Fields:
		* customer_id
		* name
		* money_spent
		* color_of_shirt
	* How to add a column to an existing table:
	
		```
		ALTER TABE tablename
		ADD columnname datatype; 
		```
	* Select certain fields from table:
	
		```
		SELECT name, color_of_shirt FROM customers
		WHERE money_spent > 5;
		```
	* Find average of column:
		
		``` 
		SELECT AVG(colunmn_name) FROM tablename;
		```
---

## Relationships Between Entities

* One-to-Many Relationship
	* An author has many books, a book has one author
* One-to-One Relationship
	* An office has one manage and a manager manages one office
* Many-to-Many Relationships
	* TV Characters are relations to many episodes, and episodes are related to many characters 
* Examples:
	* Tournement : Chess Game
		* Many games in a chess tournament, but one tournament for each game: 
		* ```one-to-many```
	* Olympics : Opening Ceremonies
		* One Olympics is related to one opening ceremony and one opening ceremony is related to one Olympics: 
		* ```one-to-one```
	* Buildings : Apartments
		* An apartment can only belong to one building, but a building and belong to many apartments: 
		* ```one-to-many```
	* Shelters : Animals
		* An animal belongs to one shelter, but a shelter can belong to many animals: 
		* ```one-to-many```
	* Baseball Players : Baseball Games
		* One baseball players can belong to many games, and a baseball game can belong to many baseball players: 
		* ```many-to-many```
	* WDI Courses : Students
		* A WDI class belongs to many students, but many students belong to one WDI class
		* ```one-to-many```
* Standardized Way to diagram relationships between entities in a given domain
	* Called an **entity relationship diagram**:
		* *see diagram of entity relationships diagram in notebook*
			* one-to-many
			* one-to-one
			* many-to-many
			
---

### PM Learning Objectives

* Explain naming convetions for SQL
* Explain how we represent 1-1 & 1-many rel in SQL
* Explain what constraints are
* Explain what foreign keys are & how to use them

---

### Naming Conventions in SQL

*  Ruby Classes
	* Author = ```author.rb```
	* Book = ```book.rb```
	* PurchaseReceipt = ```purchase_receipt.rb```
*  RSpec Code *Ruby for testing*
	* ```spec/author_spec.rb```
	*``` spec/book_spec.rb```
	* ```spec/purchase_receipt.rb```
* Database Tables
	* One database per probject/application
	* Plural databases because Ruby Class is a template for create one instance of an author. Database tables hold all of our authors. 
	* ```bookstore database```
		* ```authors```
		* ```books```
		* ```purchase receipts```

** Intro to Linking Data **

* Primary Keys
	* Features of Primary Keys
		* Unique
		* Not NULL
		* Auto Incrementing
			* I don't have to tell database what the ID number is, the database will automatically create it for me when I add a record. 
		* If we delete a record, the database will **NEVER** reuse id_numbers.
* Add a column for ```author_ids``` to the books table. 
	* id numbers in this colum reference specific records in the authors table. when data is updated in the authors table, that is automatically reflected in the books table. 
	* We put primary key references in the table that s on the 'one' side of a one-to-many relationship
		* If we reference more than one unique_id in a column, it'll break the index
	* These ids in a different table are called **foreign keys** beacuse they reference a *foreign* table
* Indexes
	* Indexes are just like indexes in the back of a book. 
	* If you have something you're looking for you can either scan the book line-by-line and make a mental note whenever you find a word you need. Alternatively, you can go to the back of the book and see where the word is referenced. 
	* Databases don't index everything, because it takes extra time and space to do. 
	
** Mechanics of Linking Data **

* First set up schema
	* Includes tables, constraints (primary keys, references, etc.), etc. 
		* Also if we say that the pub_date can't be before 1923 that would be a constraint. 
		* Yon't be able to create a book with an author_id that doesn't exist. 
* How to Set a Primary Key
	
		
		CREATE TABLE authors (
		id integer PRIMARY KEY,
		name varchar(255),
		status varchar(255),
		);
		
* How to reference another table:

		CREATE TABLE books (
		id integer PRIMARY KEY,
		title varchar(255),
		pub_date date, 
		author_id integer REFERENCES authors(id)
		);
* View books by multiple authors:

		SELECT * FROM books WHERE author_id = 2 OR author_id = 3;
		
* Sort selected data:

		SELECT id, pub_date FROM books ORDER BY pub_date;
		
* How to insert data from .sql file into database

		psql -d database_name < filename_seeds.sql
		




		
