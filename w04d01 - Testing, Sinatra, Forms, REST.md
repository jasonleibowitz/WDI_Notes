# Day 16 Notes | Week 4 Day 1

March 17, 2014

---

### Learning Objectives

* Understand how data can be placed in params
* Write forms that formats data as expected by the server
* User Sinatra as a CRUD interface to AR models
* Understand basics of REST

* This week
	* Forms
	* REST
	* Sessions
	* Rails Basics
		* Model V Controllers (MVC)
* Next week
	* Rails (cont)
	* Wed - Tue: Project 1


---

### Testing

![table](http://imgur.com/FMMze6N.png =400x)

|  Message | Query  |  Command |
|---|---|---|
| Incoming  | ASSERT RESPONSE == Expected  | ASSERT public side effects change as expected  |
|  Sent to Self |   |   |
|  Outgoing |   |   |

* Incoming Messages
	* Query
		* Asking for value and response, but don't change state
	* Command
		* Like behavior, but usually has side effect like changing state or puts to screen. Doesn't usually return response. 
		* Examples
			* ```cat.judge_person```
				* happiness increased by 1
			* ```cat.add_riend(animal)```
				* ```.friends_with?(animal)``` returns true or false
					* doesn't rely on whether ```@friends``` is an array or hash
				* because this is the *public* method
				* when designing tests and methods, think about "what do I want my public method to be"
			* ```.friends_list``` that returns array
				* public query that converts @friends to array
	* Private Methods
		* A method you can call inside your method definitions:
	
				class Cat
			
					def judge_person(p)
						self.determine_worth(p)
					end
				
				private
			
					def determine_worth(p)
				
					end
				
		* You can call the judge_person method publically elsewhere in your program, but you can only call the private determine_worth method somewhere in your class. 
			* It's a private method that your Class needs to do in order to do its job. 
			* You can never publically ask a cat to ```determine_worth```. You ask it to judge a person, and that ```determines the worth```. 
		* private methods are methods that help a class do its job.
* Outgoing Messages
	* Anytime the Cat object sends a message to another class. 
	
			class Cat
		
				def add_friend(animal)
					@friends << animal
					if animal.class == Dog
						animal.add_friend(self)
					end
				end
			
			end
			
	* There are 2 outgoing messages here:
		* animal.class == Dog is a query
		* animal.add_friend(self) is a command
	* Testing outgoing messages
		* Don't test the behavior of the command on another class, test that the command was sent 
		* **Mock** - object that barely behaves like a dog, just enough for our test
		
				animal = DogMock.new
				cat.add_friend(animal)
				animal.expect(add_friend, cat)
				animal.verify
				
		* Setting up expectation that animal.add_friend(self) method is called on animal object. 
			* somewhere inside cat.add_friend method the animal.add_friend(self) will happen
			
* Make tests as independent of other parts of application as possible
	* Make them fast
	* Don't make them too fragile
* **coupling**
	* when two classes interact with each other by sending messages back-and-forth they are connected via coupling
	* the more messages they send to each other, the tighter their coupling is
		* grows exponentially, not linearly. 
		
---

### Forms

* Params
	* Params is a hash (with indifferent access)
		* means a string or a symbol can be used interchangeably 
	* Two ways to get data into params
		1. URL - pattern matching
			* params["id"]
			* params[:id]
		2. Query String
			* http://ex.com/users/12?name=adam&age=47
			* scheme | host | path | query string
	* A way for stuff to get into params for you: **Forms**
		* All of these ways are ways to get data from user. 
		* The first two are usually generated by the links the user clicks on, which are generated by the developer in the erb. 
		* Forms is **really** where we get data from users in a controlled way. 
* Forms
	* Forms are an HTML feature. 
	* Basic Structure
	
			<form action="" method="">
			
			</form>
			
		* method can be GET or POST
			* Search forms would be a GET form
			* POST is used for adding anything to the server
		* action
			* our form needs to know where to send the form, i.e,			 what route will process the statement
	* Forms are comprised of inputs
		* Text boxes, radio buttons, etc.
		* name of input corresponds to params hash key
	* GET forms
		* Get data from form
		* adds to query string
		* server parses it into params
	* POST forms
		* we have to change the method on the form and the server.rb file also has to say 
		
				post '/show_madlib' do
					erb(:show_madlib)
				end
		* POST is for something that changes thing on the server
			* Semantic difference: Creates new things on database
			* Technical difference: query is in body of the HTTP request rather than the header
			
---

### Sinatra Blog

* You make establich connection to ActiveRecord in spec_helper as well as require it there.
* created_at
	* if you have a column called ```created_at``` active record will automatically set the time for you
* Pending in RSpec
	* just put the letter 'x' before the it
* Index
	* By convention call the index file index.erb
* Redirect

		post '/blog_posts' do
  			@blog_post = BlogPost.create(params[:blog_posts])
  			redirect to("/blog_posts/#{@blog_post.id}")
		end


---
	
### The Seven RESTful Actions

* If we want to map the CRUD activities (basic things we want to do with objects), if we want to do it on the web, we can do it with REST:
	* Representative State Transfer - theory about how to make things accessible on the web as resources
	* 7 routes we can commonly define for objects so they can be represented on the web:
		1. Index - list all
		2. Show - shows one specific
		3. *New* - form for new object
		4. *Create* - takes form submission and creates object on server-side
		5. Edit - generates form
		6. Update - updates based on form data 
		7. Destroy - destroys 
* Examples for a blog
	* GET /blog-posts
		* index
	* GET /blog-posts/:id
		* show
	* GET /blog-posts/new
		* new
	* POST /blog-posts
		* Create new blog post
	* GET blog-posts/edit
		* Edit, generates edit form
	* PUT /blog-posts/:id
		* Updates edited post 
	* DELETE /blog-posts/:id
		* destroy