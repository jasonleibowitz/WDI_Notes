# Day 18 Notes | Week 4 Day 3

March 19, 2014

---

### Learning Objectives

* Explain the stateless nature of HTTP requests
* Explain how sessions can be used to achieved user-wise persistence on the server
* Use the 'session' has in a Sinatra to keep track of a user's 'screen name'
* Construct RESTful routes for nested resources

---

### Sessions (cookies)

* Client-Server Relationship is stateless
	* Each request is presented as a fresh request
	* Incoming requests are treated as independent from all other requests that the server has ever received. 
		* You can't say "I want to view the third painting you gave me"
	* How to solve this problem:
		* Server gives data AND instructions that say "tell me what I gave you last time"
		* Create a separate database table for users. Each instance in this database is a "session" so that it can keep track of things like: what they put in their shopping cart, who they're logged in as right now		* All the client needs to say is "my filenumber is this: x"
			* Your client has an ID card that has maybe their name and age, but more importantly that unique number. 
			* That ID card is called a coookie
* Cookies
	* Just a little piece of info that looks like a *hash* that is sent laong with your request to the server
	* Abstraction
		* Process of Generating cookies, 
		* Process of sending cookie with requests to server, 
		* Process of identifying which session belongs to session that just made a request, that's not our job - sinatra and rails takes care of that for us
	* We just have to worry about ```session = { } ```
		* Any changes we make to that variable will only pertain to subsequent requests made by that user. 
			* *Example:* If we have client1 and client2 making a post request to Sinatra app, and clien1 makes request with some info in it (name => bob) and client2 makes a request with (name => bill). 
			* Inside Sinatra app we'll have something that looks like this:
			
			
					post '/name' do
					
						session[:name] = params[:name]
 					
					end
					
			* If we have another route called '/name' and we just want to return the user's name, we'd do:
			
					get '/name' do
					
						session[:name]
					
					end
					
		* How does it work?
			* When bob makes a post request Sinatra makes a table for bob:
			
				| session  |
				|---|
				| :name => "bob"  |
				
			* When bob makes a post request it doesn't touch the session data for bob, it makes a new hash
			
* Sessions in Sinatra
	* You need to enable sessions
	
			enable(:sessions)
			
	* Catch session[:username]
	
			if params[:username]
				session[:username] = params[:username]
			end
			
	* *Note*: If you quit the server and open it again your session is cached. 
	* Log out
			
			if session[:username]
				session[:username] = nil
			end
			
---

### RESTful Routes for Nested Resources

* Resource
	* Entity that you want to allow users to interact with
* Nested Resources
	* A resource within a resource
	* Example
		* Books
			* If authors and books had a one-to-many relationship and we could call author.books, then books would be a nested resource of author
			* Any book we want to arrive at can be arrived at, or we can interact with via the author
			* *we replace 'book' with 'the author's book' in our head*
	* Arise in one-to-many relationships
	
**RESTful Routes for Nested Resources**

	
| REST  | Route  | Nested Route |
|---|---|
| Index  | GET '/authors'  | GET '/authors/:author_id/books' |
| Show  | GET '/authors/:id'  | *GET '/authors/:author_id/books/:id'* * |
| New  | GET '/authors/new'  | GET '/authors/:author_id/books/new' |
| Create  | POST '/authors'  | POST '/authors/:author_id/books' |
| Edit  | GET '/authors/:id/edit'  | *GET '/authors/:author_id/books/:id/edit'* * |
| Update  | PUT '/authors/:id'  | *PUT '/authors/:author_id/books/:id'* * |
| Delete  | DELETE '/authors/:id'  | *DELETE '/authors/:author_id/books/:id'* * |

* How to get all books by a particular author?
	* GET '/authors/:id/books'
		* This is an index action
* Create a new book by a specific author:

		<form>
			<input "hidden" >
				<name = "author_id" value="3"
				
		</form>
		
	* This allows us to get rid of pesky drop-down menus where we choose who wrote the book when we create a new book via '/books/new'
* **Shallow Nesting**
	* If what we're trying to pull can already be accessed by it's own unique id we do not need a nested route
		* see nested route in table with asterisk
		* Some of the URLs are not as deep as they could be