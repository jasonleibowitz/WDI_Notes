#Day 11 Notes | Week 3 Day 1

March 10, 2013

---

### Learning Objectives

* Use and Explain what a join is in SQL
* Create SQL queries that use a JOIN
* Use and Explain what join tables are an how they can represent many-to-many relationships
* Write SQL queries that use a join table

* Explain what an ORM is
* Explain what ActiveRecord is
* Explain what "convention over configuration" with respect to AR
* Distinguish between data in memory and database
* Use CRUD-related AR commands to interface with a database 

---


### Databases

** Joins **


* CRUD
	* C - Create (Insert)
	* R - Read (Select) 
	* U - Update 
	* D - Destroy (Delete)
* Tables


	*Books*

	| id  | name  | pub_year  | author_id  |
|---|---|---|---|
| 1  | H.G.T.T.G  | 1985  | 3  |
| 2  | Light in August  | 1958  | 1  |
| 3  | Game of Thrones  | 2001  | 2  |
| 4  | Sound & Fury  | 1967  | 1  |
| 5  | A.S.O.I.A.F.  | 2007  | 2  |

	*Authors*

	| id  | status  | name  | 
|---|---|---|---|
| 1  | inactive  | William Faulkner  |
| 2  | active  | G.R.R. Marting  |
| 3  | inactive  | Douglas Adams  |

* How to run a query that uses data from both tables in the same query
	* This is where **joins** come in
		* Joins take two or more tables and *temporarily* (for the life of the query) sticks them together based on some criteria 
	* ```SELECT * FROM books JOIN authors ON books.author_id = authors.id;```
	* Joined Table:
	
		| id  | name  | pub_year  | author_id  | id  | status  |  name |
	|---|---|---|---|---|---|---|
	| 1  | HGTTG  | 1985  | 3  | 3  | inactive  | Douglas Adams  |
	| 2  | Light in August  | 1959  | 1  | 1  | inactive  | W. Faulkner  |
	| 4  | Sound & Fury  | 1967  |  1 | 1  | inactive  | W. Faulkner  |
	| 3  | AGOT  | 2001  |  2| 2 | active  | George R.R. Martin  |
	| 5  | ASOIAF  | 2007  |  2 | 2  | active  | George R.R. Martin  |
	
	* If we wanted to update query to only view books where author is inactive:
		* ```SELECT * FROM books JOIN authors ON books.author_id = authors.id WHERE authors.status = 'inactive';```
			* This would build the whole table and then only pick the ones where the status column is 'inactive'. 
	* **Note**: You shouldn't use UPDATE or DELTE on Joined tables. Joined tables are almost exclusively for SELECT statements. 
	
** Join Tables **

* If you have a table that indirectly references another, for instance tenants have an apartment, and apartments have a building, when we want to join tenants to buildings the apartments table acts as a **join table**. 
	* It traverses a relationship that is more complicated than just a single join. 
	
		```
		SELECT tenants.name, apartmetns.name, buildings.name FROM tenants JOIN 	
		apartments ON tenants.apartmetn_id = apartments.id JOIN buildings ON 	
		apartments.building_id = buildings.id;
		```
	
** Representing Many-to-Many Relationships **

Books

| id  | title  |  author_id |
|---|---|---|
| 1  | AGOT  | 1  |
| 2  | Sound & Fury  |  2 |
| 3  |  Light in August |  2 |

Authors

| id  | name  |   
|---|---|---|
| 1  |  GRRM |   
| 2  | Faulkin  |   

* Ex, say George R.R. Martin and Faulkin co-authored book 3. How to represent that a book has many authors and authors have many books? 
* The relationship line between author and book represents authorship. To have a many-to-many relationship, we createa  new authorship table that represents the connections between authors and books. 
	* It doesn't store any data other than the relationships. 
	* It's common to call it authors_books rather than authorships. 
	* This talbe is called a **join table**. 

authors_books (Authorship)

| author_id  | book_id  |
|---|---|
|  1 | 1  |
| 2  | 2  |
| 3  | 3  |
|  2 | 3  |

* How to view columns in this view:
	* ```Select * FROM books JOIN authors_books ON books.id = authors_book_id JOIN authors ON authors_books.author_id = authors.id```
	
| id  |  title | author_id  | book_id  | id  | name  |
|---|---|---|---|---|---|
|  1 | AGOT  | 1  | 1  | 1  | GRRM  |
| 1  | S&F  | 2  | 2  | 2  | WF  |
| 3  | Light in August  | 1  | 3  |  1 | GRRM  |
|  3 | Light in August  | 2  | 3  | 2  | WF  |

* **Note**: Join Tables usually do not have a primary key

---
### Object Relational Mapping (ORM)

**Auto Incrementing** 

* You do this with the *serial* datatype:

		id SERIAL PRIMARY KEY
		

**ActiveRecord**


* An awesome wrapper for writing tedious SQL
* Talk to rows of a database as if they were objects in a class
* Mechanics
	1. Requre active record
	
			require 'active_record'
	2. Connect to database
	
			ActiveRecord::Base.establish_connection(
				database: "library"
				adapter: "postgres"
			)
			
	3. Create a class and link it to the right table
	
			class Book < ActiveRecord::Base
			end

		* this class is a child of ActiveRecord
	
	4. **Convention over configuration**

		* ActiveRecord uses the convention to try to figure out what we're trying to do. 
		* Two associations are made via convention:
			1. Since we named the class Book, it's already looking for a books table in the database
				* Specifically a downcase, snakecase, pluralized version of Book
			2. The 'id' column is a convention. It assumes that the primary key is in a column called 'id'. 
	5. Access data in database:
	
			Book.all.each do |book|
				puts book.title
			end
			
		* For Book.new our ORM will create a query in the language we told it in the database we told it to run on, get back some form of results, which are basically rows and columns of data, put those results in a convinient data structure for us, i.e. an array of objects, and give us that array so we can do whatever we want with it. 
		* Books.new is like ```SELECT * FROM books;```
		* ActiveRecord automatically adds a getter and setter for each column name in our table: title, author, etc. 
		
** Create a New Record via Active Record **

		new_book = Book.new({:title => "Hyperspace", :author => "Michio Kaku", :hardcover => false)
		
		
* This will create a new object in our Ruby program, however it doesn't save it in our database. 
* How do we save it?

		new_book.save
		
* But if we use ```Book.create``` it will create the new book and automatically put it in our database at the same time!


** View Data from Database **

* Find a book by the id number

		Book.find(20)
		
	* The argument we give must be the **primary key** of the books table
* Find a book by a different column

		Book.find_by(:title => "A Game of Thrones")
		
	* This is *case-sensitive*

** Update **

* Set variable to item in database:

		feynman_book = Book.find(22)
		
	* Change author name now to just Richard Feynman
	
			feynman_book.author = "Richard Feynman"
			feynman_book.save
			
	* Instead of a two-step process, use the .update method to change something and save at the same time:
	
			feynman_book.update({:author => "Richard Feyndman"})
			

** Destroy **

		feynman_book.destroy
	
* It will return the book information, but automatically delete it from the database. 

** Terms **


* persisted?
	* tells you if the variable is saved in your database
			
			new_book2.persisted?
			false
* all
	* Class Method
		* We call ```.all``` on the class, not on an instance. 
		* This is a class method because it has to do with the class itself, not an instance of the class. 
* new
	* Class Method
		* Doesn't pertain to an instance of an object, even though it does create an instance of this object. 
* save
	* Instance Method
		* The instance is already create and we want to save that particular instance 
* create
	* Class Method 
* find
	* Class Method 
* find_by
	* Class Method
* update 
	* Instance Method
* destroy
	* Instance Method
* where
* order


	
