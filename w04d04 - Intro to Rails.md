# Day 19 Notes | Week 4 Day 4

March 20, 2014

---

### Learning Objectives

* Understand and Explain what Ruby on Rails is and its architecture
* Descrie the lifecycle of an HTTP request/response in Rails
* Explain convention over configuration in Rails
* Create a new Rails app and understand its structure
* Explain bundler and how a "gemfile" works
* Follow Rails conventions in creating Ms, Vs, Cs
* Create a Rails app to handle CRUD RESTfully

---

### Rails

**Intro**

* What is the point of Rails?
	* Framework that enables you to build web applications
* Difference between Rails and Sinatra
	* Rails does more for you
		* More advanced features
	* For many applications it lets you build things faster (gives you more tools) and you also have to make fewer decisions
		* This stems from the idea of convention over configuration 
			* As your Sinatra app got more complex, that server file will get long and gnarly 
			* You had to make separates files/folders for each route in Sinatra
		* Rails forces you to structure your app in a certain way, which frees you from certain decisions
	* Has built-in support for TDD and ActiveRecord
	* Minimizes Decisions
	* Serving static assets
		* CSS
		* Rails has a more advanced static asset pipeline
		
**HTTP Requests in Sinatra**

* What happens when HTTP Request comes into Sinatra?
	1. HTTP Request
	2. HTTP Server
		* webrick, thin, etc
		* listens for HTTP requests and understands it
	3. Rack
		* known as "middleware"
		* handles http features that apps use in common: chacing, cookies, logging
	4. Sinatra
		* Where the magic happens
		* Looks at route (path & verb)
		* **parses** route and runs appropriate block of code:
					
					get '/' do
			
					end ```
					
		* sets up the **params hash** for us
					
	5. Your App Code
		* This block of code will return a Ruby string, either by using a method like ```redirect to``` or you hardcode a Ruby string, or most likely you're using the erb method to render a view template
	6. String gets sent back to Sinatra, who then talks to the Rack and the HTTP server that constructs a response, usually HTML, but can also be a command to the browser (redirect)
* One big app file, not easy to break it down.

**HTTP Requests in Rails**

* Rails is much more modular than Sinatra
	1. Web Browser makes an HTTP request
	2. HTTP Server
	3. Rack
	4. **Rails**
		1. Router
			* equivelent to Sinatra parsing our route
			* looks at the request that comes in
			* basically a pattern matching, just like in Sinatra we list ``` get '/article'```, etc.
			* Decides which controller to send it to
		2. Controller
			* More than one controller for modularity
				* Different controllers for different parts of the application
					* *You may have one controller for artists, one for paintings, one for museums, etc*
			* Set of methods that correspond to set of methods for that source
				* Just like we have multiple blocks in Sinatra for each resource
		3. Models
			* Uses ActiveRecord
			* Just classes that inherit from ActiveRecord
			1. Talks to the **Database**, which is outside of RAILS, then returns data back to model
		4. Models returns data to controller
		5. Controller sends data to views
			* Multiple views
		6. Views sends string back to controller
		7. Controller sends string data back to rack
		8. Rack sends string back to http server
		9. HTTP server sends string back to web browser
		
** Making a New Rails App **

* Terminal command that creates all necessary files and folders:

		rails new superheros --database=postgresql
		
* Folders
	* App is the main folder where 90% of our diagram lives
		* Controllers
		* Assets
			* JavaScript
			* CSS
			* Images
		* Helpers
		* Mailers
			* How you send email from app
		* Models
			* ActiveRecord models
		* Views
			* ERB templates
		* MVC
			* Fundamental design paradigm of Rails
				* Models
				* Views
				* Controllers
	* Config
		* routes.rb
			* This is the router file where we define all of our rotues
		* A log of configuration for the app lives in the config folder
			* What db to use? 
		* Modes
			* Ruby changes configs based on what mode your app is in:
				* Dev Mode
					* running on own comp to develop app and paly with it
				* Test Mode
					* running app under tests
					* deleted every time you run your tests
				* Production Mode
					* app is live on server and used by people
			* We will have three databases that can live independent of each other
				* usually production lives on a separate server
	* bin
	* db
		* database
		* has seeds file and where can write a seeds script here
		* migrations
			* takes database from one schema to another. 
			* has a nice syntax for this
			* we don't have to manually writes schemas in rails
			* migraints are just steps to change database schema
			* maintain history because we save individually steps
	* lib
		* if you have code that you've written that might belong in more than one rails app (social dating site for sea turtles and biz asset manager for sea turtles), if we had any classes that know how to deal with sea turtles specifically we could put it in lib
		* common code that works in more than one rails app
	* log
		* rails keeps a log of everything it does and that will be put into log
		* we'll review to keep track of what went wrong with app
	* public
		* we won't use too much
		* equivelent to public folder of sinatra, except most of that stuff will actually go into app/assets
	* tests
		* there are lots of different types of testing
		* /models
			* unit testing we've done
		* integration
			* testing views
		* whole support system in place to test system
	* Gemfile
		* so far we haven't had to worry about what gems our app needed. if it needed a gem, we just made sure it was installed on our system, then somewhere say ```require 'active_record```
		* in the real-world we'll have to manage that, and that's what gemfiles are
		* list of all gems your app needs
			* you specify a version (or approx version) to make sure you don't use a newer or older version that might break your app
		* notes
			* pg is how we talk to our postgres database
		* this file is used by a tool called "bundler" and it will install all them gems for you
		* it insures that when we run our apps it installs gems in the correct version
		* we don't have to use ```require```
	
** Implementing Rails app **

1. First thing to do:
	* Set up routes
	
			get '/superheroes' => 'superheroes#index'
			
2. Set up Database
	* In terminal in your rails folder run:
	
			rake db:create
	* rake means "rails create"
	* this creats all three databases for us
3. Set up Controller
	* Convention over configuration
		* We don't have to tell Rails where this controller lives
	* Create it under app/controllers
		* use pluralization: ```superheroes_controller.rb```
	* application_controller.rb
		* things that need to happen app-wide go into this controller
4. Your controller should inherit from application_controller
	* Superhero controller can have anything we define in ApplicationController, and since that inherits from ActionController, Superhero will have everything in ActionController
5. Define actions
	* We can do this via methods
	* Define a method for index because the controller is looking for ```#index```
	* Convention
		* If you don't specify in an action Rails will try to match action to method name. So if you have ```def index``` Rails will look for ```erb(:index)```
6. Create erb file for index to find
	* Create folder in app/views with the same name as your controller (plural) and make an ```index.html.erb``` file
7. Create class file in app/models
	* Classes are singular: ```superhero.rb```
8. Migration
	* Anytime we change our data we create a migration
	* Rails builds this structure for us (the filename needs to have a timestamp in it)
	
			rails generate migration CreateSuperheroes
			
	* Creates a different class in ActiveRecord whose job is to create a table
	* Rails run them in order of timestamps
		* which ones have and haven't been run yet
	* **Create Table Values**
		* Should be ERD'ed
		* DSL (Domain Specific Language)
		
				t.string :name
				
			* will create a new table of type string called ```:name```
			* string maps to varchar(255)
9. Run migrate file
	
		rake db:migrate
		
10. Write seed file
11. Run seed rake task

		rake db:seed
		
** If Rails Pluralizes Incorrectly **

* We need to manually tell Rails what table to look at. In your Superhero.rb class file write:

		self.table_name = "superheroes"

	

** Misc**

* Run Rails Server

	```rails server``` or ```rails s```
	
* All tasks we can run
	* ```rake -T```
* params.inspect in Rails
	* ```render text: params[:superhero].inspect```
	
	
1. route
2. controller
3. model behaviors
4. implement the view