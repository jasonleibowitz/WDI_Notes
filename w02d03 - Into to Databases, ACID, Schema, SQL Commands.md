#Day 8 | Week 2, Day 3 Notes

######March 5, 2014

<br />

---

###Learning Objectives

* Explain what a database is
* State the advantages of a DB
* Explain the relation between a database, a DBMS, and SQL
* State the principle of ACID in layman's terms
* Describe the components of a schema
* Set up a schema using PSQL
	* CREATE / DROP
* Manipulate and retrieve data in your schema
	* SELECT
	* WHERE
	* INSERT INTO...VALUES
	* UPDATE
	* DELETE FROM


---

###HappiTails HW Review


1. Step 1 - Planning (Mental Model of Classes)
	* Animal Classes
		* Attributes
			* Name
			* Species
			* Toys - Collection. Make a hash. 
		* Behaviors
			* ```get_toys```
			* ```info_string```
	* Client Class
		* Attributes
			* Name
			* Age
			* Pets - Can have many, but doesn't start with any. Make a hash because it allows us to have easy access to the values inside. 
		* Behaviors
			* ```info_string```
	* Shelter Class
		* Attributes
			* Name
			* Clients - hash list
			* Animals - hash list
		* Behaviors
			* ```add_animals```
			* ```add_clients```
			* ```facilitate_adoption```
			* ```facilitate_return```
			* ```list_clients```
			* ```list_animals```
2. Priorities
	* Assign priorities to behaviors
		* 1 - ```add_animals``` and ```add_clients```
		* 3 - ```facilitate_adoption``` and ```facilitate_return```
3. Main.rb
	* Create a method that displays the menu that returns the user input
	
	<br />
	
	```
	return gets.chomp.downcase.to_sym
	```
	* Make a shelter at the top of main.rb
	
	<br />
	
	```
	our_shelter = Shelter.new("Agraba's Best")
	```
	* case statements
	
	<br />
	
	```
	choice = menu()
	while choice != :q
		case choice
		
		when :p #make a pet
			puts "What's their name?"
			name = gets.chomp.downcase
			puts "What's the species?"
			species = gets.chomp.downcase
			
			new_animal_obj = Animal.new(name, species)
			
			our_shelter.animals[new_animal_obj.name] = new_animal_obj
			
		when :c #make a client
			puts "What's their name?"
			name = gets.chomp.downcase
			puts "What's their age?"
			age = gets.chomp.to_i
			
			new_client_obj = Client.new(name, age)
			
			our_shelter.clients[new_client_obj.name] = new_client_obj
		
		when :d #list pets
			our_shelter.animals.values.each { |animals_obj| puts 
			  animal_obj.animal_info }
		
		when :l #list client
			our_shelter.clients.values.each { |client_obj| puts 
			  client_obj.client_info }
		
		when :t #adopt
			our_shelter.clients.values do |client_obj| puts client_obj.client_info
			end
			
			puts "Choose a client by name"
			client_key = gets.chomp.downcase
			
			our_shelter.clients.values do |animal_obj| puts animal_obj.animal_info
			end
			
			puts "Choose an animal by name"
			animal_key = gets.chomp.downcase
			
			client_obj = our_shelter.clients[client_key]
			animal_obj = our_shelter.animals[animal_key]
			
			client_obj.pets[animal_key] = animal_obj
			our_shelter.animals.delete(animal_key)
		
		when :r #return
			our_shelter.clients.value do |client_obj| puts client_obj.client_info
			end
			
			puts "Choose a client by name"
			client_key = gets.chomp.downcase
			
			unadopt_client_obj = our_shelter.clients[client_key]
			
			unadopt_client_obj.pets.values.each do |pet_obj| puts 
			  pet_obj.animal_info
			end
			
			puts "What pet do you want to give back?"
			animal_key = gets.chomp.downcase"
			
			animal_obj = unadopt_client_obj.pets[animal_key]
			
			our_shelter.animals[animal_key] = animal_obj
			unadopt_client_obj.pets.delete(animal_key)
		
		end 
		choice = menu()
	end
	
	```		
	* For adoptions we are working with client objects and animals objects
		* Use client's name as key to get client back from clients hash 
			* Now we've listed all clients, gotten key. Listed all animals, and gotten keyt that will be animals name. 
		* To facilitate adopt we need to get animal and take it out of shelter's animals hash and put it inside the pets hash inside client object
			* First get client. 
		* After we put animal in client's pets hash, remove it from shelter's animals hash
3. Making a seed file
	* Wrap the seed.rb file in a method that will be able to create animals, client objects, put those animal and client objects in a shelter, then return the shelter so we can use it inside our application. 
	Set the method in seeds.rb equal to the shelter variable ```happitails = seed_me_baby ```
5. Start Basic
	* Define Classes
	* Think about logic to get classes to talk about each other
	* Think about the most important/hardest methods and do that
	* Once you have working code and thinks work, then start complicating things:
		* When did I repeat myself? 
		
---
		
###Where we Are

1. Pieces to a Web Developer
	1. OOP
	2. TDD
	3. Data Persistence
		* File's are the best way to handle data when we have a lot of it. Databases are better. 
	4. Interwebz
		* How's the internet work? 
		
---

###Databases

*What are we trying to do? Find the information we need, quickly and efficiently.*

1. Files
	* If we store data in files we would have rows and rows of data: name, species, etc. 
		* This is great when we're talking about five or ten animals that we want to keep track of. 
		* When I want to go lookup an animal I look at the first row, is that the anmimal I'm look for. Is that the one, no? Go to the next one. 
		* But when you have 10,000 animals in a text file that is incredibly inefficient. 
2. What goes on in a database?
	* A **database** is essentially a way of storing information in some way and allows us to access this information that is realiable and quick, even when working with very large data sets. 
3. Vocab
	1. **Database**
		* **tables**.
			* **entity**.
		* **rows** 
			* Known as **row**, **record**, and sometimes a **tuple**.
		* **column**:		
			* has multiple fields: **column**, **field**, **attribute**. 
		* **Primary key**
		* **DBMS** - Database Management System
			* **PostgreSQL**
	2. **Table**
	3. **SQL**
4. How Databases work:
	* **Database**
		* Inside of a database is a collection of **tables**. We can have tables of customers, employees, foods, etc. 
			* Each table can be set to equal an **entity**.
		* Inside each table are **row**s and rows of things. 
			* Where each row corresponds to an employee. This is called a **row** or a **record**, and sometimes a **tuple**.
		* The row's **column**'s includes the name of the employee, the salary of the employee, and the days of the week they work. 
			* Each row has multiple fields: **column**, **field**, **attribute**. 
			In a table, all of the rows need to have the same set of fields.
			* The value of a field of a particular field can be null. (null is like the nil we've seen.)  
		* Any table we make needs an attriute that is a **primary key**
			* Needs to be unique for each record.
			* Every record needs one.
			* This is the fail-safe way of accessing what we want. 
5. How to interact with Databases
	* We need a DBMS (Database Management System)
		* Allows us to put info into them and get info from them. 
		* The DBMS we will use is called **PostgreSQL**, *(PostGres)*
	* The language we use to speak with them is called **SQL**. 
	* Relational Database
		* We're using a relational database. 
		* This means it's easy for us, the user, the find the name of the user, the bank of the user, and the user's pet's name (from two different tables) all in one.
6. **ACID** - *(Don't focus too much on this)*
	* When you say that *something* in your database is acidic or possesses acidity, that means it has four characteristics:
		1. **atomicity**
			* When you interact with a database, it can be said to have done a **transaction**. 
			* This means that if a particular transaction doesn't complete, it doesn't happent -- a transaction has to be all or nothing. *(see notebook diagram)*
			* So how does this work? 
				* Comp will say "I'm about to do a transaction". Then it'll remove 10 from the credit of name A and then it will add 10 to the credit of name b. If at any point it sees an unfinished transaction then it will see that something is unfinished so it goes back and reverts it. 
			* aka "All or nothing"
		2. consistency
			* Our table has certain rules: I can't have a negative balance. 
			* The interactions I perform will always take my data from one valid state to another valid state. I should not be able to go from a valid state to an invalid state. 
				* These are called **constraints** and they're build via SQL. 
		3. isolation
			* If two transactions are trying to perform an action on the same piece of data, the database should perform as if one action happened and then the other. There should not be confusion by two actions on the same piece of data.  
		4. durability
			* "Duh"
			* If I store a bunch of info in my database and then the server that had the database lost their power. Then the computers turned off. When you turned the computers turned back on, your data should still be there. 
			* Physically, you shouldn't be storing your databases in RAM. You should be storing it somewhere that is durable. 
7. **Domain Modeling**
	* Overview
		* We draw a line between the **attributes** that an object has and the **behaviors** that it portrays. 
			* As far as our database is concerned, we're capturing attributes.
		* But we also capture **relations** in the database. 
			* What animals belong to a particular client
			* What aniamls belong in the shelter?
	* When we draw a line between what attributes and behaviors a class has, that process is called **problem modeling**. 
		* Our problem was: we need to set up an animal shelter that has clients and animals and can perform adoptions and returns betwee them. 
		* In order to model our problem, we first need to identify the classes we will have (Animal, Shelter, Client). 
		* The next thing we do is say what attributes each class has. 
		* Finally, we think about what behaviors each class has - how should we be able to interact with them. 
	* Domain Modeling, roughly speaking, is everything in problem modeling, except behaviors. 
		* Modeling our domain, our domain is our classes and their attributes. 
	* Example: Facebook
		* Before you can think about what Facebook can do, think about what data Facebook should use:
			* users
			* pictures
			* posts
		* Don't worry about what happens when you click a button, just think about organizing the data we'll be working with. 
	* Another Example: Let's model the domain of our restaurant
		* Customers
			* Customer ID
			* Name
			* Spent
			* Color of Shirt
		* Employees
			* Employee ID
			* Name
			* Shift
			* Salary
		* Foods
			* Food ID
			* Course
			* Cuisine
			* Price
			* Vender Info
			* Received At
			* Lifetime

9. The Process
	1.  Setting up our schema
		* What is a schema - it's the current structure of our database
		* First we have to create our database
			* ```CREATE DATABASE name;```
				* Note: Every SQL statement HAS to end with a semicolon (;)
		* Then we have to create our tables
			* When you create a table, you need to tell it exactly what fields it's going to have. 
			* Not only do you do that, you also have to tell it what type of data is going to be sitting in that field, which means that databases have their own **datatypes** that we have to work with:
				* integer
				* varchar(255) - variable length characters. basically a string. Pass argument to set max string length. 
				* timestamp - Year Month Day Hour Minute Seconds
			* Before you can create tables, you have to connect to your database: ```\c name;```
			* Create Table
			
			```
			CREATE TABLE tablename 
			(field_name datatype,
			field_name datatype,
			);
			```
				
		* Command Line Tool to talk to Postgres: ```psql```
			* Along with this, we get ```createdb```.
			* Postgres by default will try to find a database with the username of your machine.  
		* SQL Commands
			* ```\?``` - shows help documentation
			* ```\l``` - list databases
			* ```\dt``` - list tables
			* ```\c``` - connect to a new database
				* At any given time PSQL is connected to *one* of our databases. The commands we talk to PSQL with only apply to that database. 
	2. Manipulating the data
		* How to put data in our tables row-by-row:
		
		```
		INSERT INTO tablename
		VALUES (~, ~, ~)
		```
	3. View database data
		* Selecting/Viewing Data:
		
		```
		SELECT fields FROM table
		```
		
		* Note  that fields can be a *, and that will select everything
		* View all details for food just called Kimchi
		
		```
		SELECT * FROM foods
		WHERE name = 'Kimchi'
		```
	4. Update data in a table:
	
		```
		UPDATE table
		SET price = 10
		WHERE name = kimchi
		```
	5. Delete a row in a table:
	
		```
		DELETE FROM table
		WHERE _____; (ex. name = 'Kimchi')
		```
9. Talk to Postgres using our *Ruby chops*. 
	* We'll be able to save the "seeding" that we did in the *HappiTails* example and add a bunch of animals today. Then come back tomorrow and they'll still be there. 
	
---

###Recap

* Why use databases over text files
	* More efficient, reliable, and robust
* SQL
	* The language we use to communicate with the database management system
* ACID
	* atomicity, consistency, isolation, durability
* Components of a schema
	* Schema - structure of database: what tables you have, and within those tables what fields you have (the name of each field, and its datatypes)
* Mechanics
	* CREATE - create databases and tables
	* DROP - delete tables
	* SELECT - retrieves information from a table in our database
	* WHERE - we can change what we retrieve with where, which allows us to select particular rows in our table
	* INSERT INTO... VALUES - add rows to our table
	* UPDATE - update existing rows in our table
	* DELETE FROM - get rid of rows in our table 