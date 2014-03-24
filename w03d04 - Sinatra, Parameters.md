# Day 14 Notes | Week 3 Day 4

March 13, 2014

---

### Learning Objectives

* Distinguish between route & path
* Use and explain what Sinatra is and why we use it
* Use and explain what WEBrick & Rack are
* Create a Sinatra app that responds to HTTP requests
* Define static and dynamic routes in Sinatra
* Use ERB to render dynamic HTML pages (views)
* Include an asset (eg CSS file) in a view

---


### Morning Exercise

* Ternary Operator
	* a nifty way to write an if else statement
	
			trainer1_score > trainer2_score ? "#{trainer1.name} is the winner!" : "#{trainer2.name} is the winner!"
			
---

### Review

* URL
	* ```http://www.google.com/search?keyword=cats&mode=img#relsearch```
	* has multiple parts to serve unique functions:
		* scheme
			* how to internet, or how to transfer files from one place to another
		* host
			* where are we going
		* path
			* file vs resource 
			* in *file-mode* the path represents a directory or a file that lives on the actual file-system present on the server
				* more traditional approach
				* called the **common mode**
			* in *resource* mode a path represents a resource
				* eg.
					* Posts on a blog
					* Pictures on instagram
				* depending on the path (resource)user requests the application will construct the approprite response and shoot it back to whoever requested it
					* if Adam requests the google calendar resource it will build a calendar for Adam and send it back to him. If someone else requests it, it will build that person's page. 
		* query string
			* one way for client to pass information to server
		* fragment
* Soft links
	* Verbs (modes of HTTP requests)
		* GET
		* POST
		* PUT
		* DELETE
	* Paths
		* /
		* /search
		* /calendar 
			* GET
			* POST
	* Route
		* Verb + Path = route
		
---

### Sinatra

** Intro **

* We use Sinatra to make web apps
* An application does different things upon receiving requests to different routes
	* Sinatra is a Ruby framework that allows us to create an application
		1. Listen to HTTP requests
		2. Identity route
		3. Parse the info that came with the request (query string)
		4. If this route, do this; if that route, do that. 
			* This entails constructing and sending a response back to whoever made the request

** Parts of Sinatra **
			
* WEBrick
	* listens for HTTP requests
	* receive requests
	* pass requests along
* Rack
	* Middle-Man who parses request
* Sinatra
	* Recognizes information and then creates response based on data
	* the heart of our application - where we execute what our app is supposed to do
	
** Why do we use Sinatra? **

* Ruby web related framework with web-related tools to make the following easier:
	* We have to differentiate routes
		* it gives us tools to do that easily		* we have to receive HTTP requests
		* It gives us tools to do that easily
		
** On the Doc Page **

* Conceptual Understanding
	* What is Sinatra
	* What it can do
		* Before you even download something, understand what it is and what it can do.
* Download/Install
	* This is how you install Sinatra
* Examples
	* Most nice developers have an example of how to use their app. This is a really nice place for us to start. 
	
** How It Works **

* Port Number
	* Sinatra listens on a port number
		* **Ports** are to gates as IP addresses are to airports
		* An IP address identifies your machine. A port is an additional specification (you can have thousands ranging from 0 - 9999). At a specific IP address you can have multiple gates for your planes to land at. 
			* You can run multiple servers or programs at one IP addresses at different ports. 
			* Allows you to have multiple things listening for requests on a single machine.
* Server
	* When we run a Sinatra ruby app our computer becomes a server and listens for requests. 
	* Whenever it GETs a request for '/' it will return "Hello World!" as our Ruby app specified. 
* Client
	 * When you run ```http://localhost:4567``` your client browser is making requests to a server
	 * localhost is semantic speak for a particular, *very special* IP address - 127.0.0.1
	 	* that IP address *always* refers to yourself
	 * The default port is 80
	 	* If you don't specify a port, the client will default to port 80
	 	
** Parameters **

* What are placeholders?
	* get '/jokes'
		* returns the joke page
	* get '/jokes/*placeholder*
		* gives you a value to do something
	
				get '/jokes/:id do
			
					return "Joke #{}"
			
				end
			
	* we can specify URL parameters with the placeholder
* How do we use it?
	* ```params``` is a hash that is available to us within every action pertaining to a route. 
		* Inside every route block we have access to a variable called params. 
	* Multiple things go into params:
		1. URL parameters
			* When you say 'I want to set up a route get ```/joks/:id```, if you say ```return params[:id]``` then that will be whatever was in the URL'
			* in other words, we specify a URL parameter by using a colon (but not a symbol)
			* when the :id gets put into params, it gets put in where the key is the URL placeholder (in this case :id) and the value is whatever the client typed.  
			* Using ActiveRecord
				* ```return Joke.find(params[:id]).content```
				
* news.rb
	* GET '/'
	* GET '/articles'
		* All dem articles
	* GET '/articles'
		* One of the articles
	* GET '/articles/new'
		* A page where you can make a new article
	* GET 'articles/:id/edit'
		* APWYC edit an existing article
* Sinatra reads GETs from top to bottom. 
	* The pickier route should be listed first. 
* HTML
	* Hyper Text Markup Language
	* Uses tags
		* opening tag - ```<tag>```
		* closing tag - ```</tag>```
	* Terms
		* HTML tag
		* head tag
		* body tag
		* h1 tag 
		* p tag
		* a tag
		* img tag
	* Trio (separation of concerns)
		* HTML
			* skeleton
			* structure
		* CSS
			* the skin (make up)
			* appearance
			* styling the structure defined by HTML
		* JavaScript
			* interactivity
			* manipulating the structure or the style of the structure
* erb files
	* means *embedded* ruby file
	* we don't put an HTML file in our folder because an HTML file is *static*. 
		* our app is meant to be dynamic, not static
	* embedded ruby is how we can dynamicize our webpages 
	* sinatra returns the erb file as HTML
* clown hat (%)
	* you can write ruby within an erb HTML file by using a clown hat
	
			<% a = 5+3 %>
	* with a clown hat and an equal sign it will retun what's inside to the page
			
			<%= a %> 
			
* layout.erb
	* The only thing on webpages that change is things in the body after the heading. 
	* This file includes a ruble line ```<%= yield %>```, which grabs content from other file and plugs it in.
	* Makes it easy to include same font, navbar, etc in all pages
* Variables
	* local variables we define in our news.rb are not accessible in our erb files
	* You should turn them into instance variables of the route handler class
		* both the news.rb file and the erb files are objects of the route handler class
* Memory-less
	* HTTP client-server relationship is stateless, i.e. memoryless
	* There is no way for the server to track that two consecutive requests came from the same person or the same user account 
	* This means that once a request comes into this route, we create a route handler object. We assign it an instance variable. It does some stuff, then it gets destroyed. 
	
* Review
	* Make dynamic HTML pages based on incoming requests
	* Pass information from routes action to the erb file so that we can do things like put article content in a page.
	* Layout erb file DRYs up our code
		* Whenever we tell news.rb to erb(:article) layout.erb and articles.erb are both invovled. 
			* articles.erb sits in layout's yield. 
	* URL parameters are a concept that only exist in Sinatra
	* URL queries go into the body of a request, but act the same as URL parameters