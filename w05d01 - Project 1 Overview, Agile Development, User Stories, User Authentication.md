	# Day 21 Notes | Week 5 Day 1

March 24, 2014

---

### Learning Objectives

**Agile Development & User Stories**

* Explain Agile Development
* Describe a user story
* Describe the benefits of user stories
* Pivotal Tracker

**User Authentication**

* Explain the difference between authentication and authorization
* Understand how passwords are stored on a server
* Understand has_secure_password works in Rails
* Implement user auth in Rails
* Use before_action to require authentication in parts of our app


---

### First Project Overview

* Topic
	* Something we're passionate about
* Scope
	* Moderately complex
		* 3 models
		* 3 ERDs
	* No Simple CRUD Apps
		* Should have specific domain-specific funcationality 
* Misc
	* Gem
		* At least one gem that isn't stock
	* JavaScript
		* Only build your own CSS
		* No bootstrap
	* API
		* At least one API
		
** Coming Up: To Review Before Projects **

* User Authorization
* APIs
* Many-to-Many Relationships
* Migrations
* Validations
* Partials	

---


### Agile Development

* More error checking built-in
	* testing project throughout the process
* Break project into smaller features
	* Have a large group of people working on separate stories communicating all the way through
* Waterfall Development
	* Cons:
		* Harder to find errors
* Tools
	* RSpec/Testing
	* ERDs
		* Figure out models you want to have, their attributes, and how they relate to each other before you even begin coding
		* Helps us plan out out thoughts and how the entities of our app will work together
	* REPL (Pry)
		* Allows us to debug our app while we're building it and not once everything is starting to fail
	* user stories
* Problems
	* Syntax
	* Architecture related to UX/UI
	* blank page problem
	* order, how to build it

### User Stories

* First Step of Agile Development
	 * Always have a plan. Never start coding without having a plan first.
	 	* User stories help us solve a lot of these problems.
* Feature
	* Particular functionality of application
	* What types of users?
		* Power Users
		* Small-time user
		* Admin
	* Make a user story for each *type of user* you may have on your site
* Structure of a User Story
	* As a (blank), I want to (blank) so that (blank).
		* A *who* a *what* and a *why*. Who are they, what do they want to do and why do they want to do it?
	* Power User Example
		* As a power user I want to make a catalogue of all the movies in the world so I can always have info about movies. 
	* If you ever find yourself with a feature with no user benefit, you may not need that feature. 
	
** Parts of a User Story (Class Presentations) **

* Scrum
	* Analogy for agile development
	* 3 core components (pigs)
		1. Product Owner
			* Stakeholders
		2. Development Team
			* Group that does actual work
			* 3-9 people
			* Engineers, developers, designers, analyzers, etc.
		3. Scrum Master
			* Not necessarily a developer, but is a buffer between developers and potential distractions that would stop developers from building product on time
			* Support role
				* Example: this person keeps coming to my cubicle and asking me questions so I can't focus on my work
				* Another example: I can't build this part until the other part is built and the person building that is on vacation
			* Deals with project as a whole where project manager deals with people on the project
	* ancillary roles (chickens)
		* auxiliary roles: consultants
		* shouldn't have as much say
	* sprint cycles
		* entire team runs on sprint cycles (days - months)
		* take product backlog (features that need to be built), pick next features to build, then go through sprint of actually building it out
		* during these cycles you have daily scrum meetings (usually standing in start-ups)
			* to reiterate we team is doing each day
		* end of sprint cycle you have iteration or feature of product that is fully functional or tested
	* Very collaborative and team oriented
* User Story
	* A way to determine project scope using functionality request by the users
	* 3 models:
		* card
			* a piece of paper describing functionality user would like
		* conversion
			* conversaion of that user story into a feature
		* confirmation
			* build feature that meets those demands
* Minimum Viable Product (MVP)
* Acceptance Testing
	* Part of the BDD Model
	* Test you can write based off the user story
		* makes sure that client or customer when they see the product, it is doing the thing they requested it to do
	* Before you're done, put the product in the hands of someone to make sure it's doing what you want
	* Bottom Line: Make sure your user stories are being fulfilled
* BDD vs TDD
	* TDD
		* Concentrates on short development cycles. 
		* Write a test, it fails, then write the method to make it pass. 
			* We do this throughout the writing of the app
		* For someone developing an app by himself, or for a very tight group that can understand what's written
	* Behavior Driven Development
		* More verbose
		* Easier for non-developers (or people not on the team) to understand what's going on in the tests
		* BDD takes into account functionality for the end-user
			* What the user wants to use the function for on the actual web-app
	* Problems that BDD solves
		1. Where to start?
			* TDD solves the blank-page problem. Write a small test to get started.
			* BDD not only helps you start, but helps you be mindful of where to start for the user. You're thinking of the business-value for the project (the end-user)
		2. What to test
			* Looking at business-value it brings to the project and thinkin about that when thinkin about what to test first
		3. How much to test in one go
			* what's do-able, i.e. how much you should be testing
			* you're using the user-story approach
		4. What to call test
			* Going back to the perspective you're giving to the rest of the team, BDD is more verbose so people outside of your team can understand where you're going with your test
		5. Understand why test failed
			* Helps to know why a test failed, but also looking at how the failure would affect the end-user
	* Focuses more on the outcomes of writing a test and not so much the implementation details of how you're going to perform an action
		* User login
			* TDD - we'd test that every step along the way works (can add, email, phone, etc). But if we want to add a different category, all of our tests would fail because they would only test certain attributes
			* BDD - checking that a user enters the login page and the result comes out. 
* Domain Modeling vs Problem Modeling
 	* Takes place in early stages of app
 	* Domain Modeling is just one part of Problem Modeling
 	* Domain Modeling
 		* Laying out entities working with attributes and relationship that those entities have
 	* Problem Modeling
 		* Laying out problem and thinking through approach
 		* Modeling the overarching issue: this is what I want to build, what steps do I need to take to build that. 
 		
**User Story Recap**

* Every user story has a *who*, *what*, and a *why*. 
	* If there's no *why* then there may not be a need for that feature
	
---
	
### Pivotal Tracker

* Keep track of user stories
	* [Pivotal Tracker](www.pivotaltracker.com) or [Trello](https://trello.com/) or [Asana](https://asana.com/)
* Parts
	* Current
		* current user stories
	* Backlog
	* Icebox
		* to be worked on later
	* Done
		* all finished stories
* Story Types:
	* Feature
	* Bug
	* Chore - housekeeping, something that needs to be added on to an existing user story
	* Release - final release of feature
* User Story Breakdown
	* Epic
		* Start with a large user story
	* Breakdown user story into smaller and smaller parts
	
---

### User Stories in Action

**Building out a User Story for an App called *Tunr***

* App Idea:
	* Users that can create accounts. 
	* Library of music users can look at. 
	* Users can listen to music or add to their own library only after they purchase it. 
	* Share playlist with friends, but friend can only listen to songs they already own otherwise they can hear a snippet unless they buy it. (notification that they need to buy song)
	* *Note*: Don't actually have to play song. Just mock out with little button.
		* envision list of song with actions, and not every action works
* User Stories:
	* As a user, I want to: 
		* create an account so I have access to all tunr features
		* delete an account because I no longer require the services 
		* be able to login with my email and password so I have full access to the site
		* view a list of personal playlists vs shared so i can listen to what i want
		* search and find songs by title and artist so I can be more efficient at finding songs I like
		* make playlists of songs that I have not yet purchased/purchased so I can organize my music
		* be able to edit my playlist so I can better organize my music
		* be able to buy a song so I can listen to it when I want
		* have an account balance so I can buy the songs I love
		* be able to refill my balance so I can keep buying music
		* gift a song to my friend so i can share the gift
		* randomly generate a playlist so i can expand my musical horizon
		* select a playlist to share with another user to share my music with my friends
		* 
	* As an admin, I want to:
		* access user accounts so I can reset a user password in case they forget it
		* track purchases to see how much money I make
		* edit the music catalogue to keep my tunes fresh (set price, add, remove)
		* set prices on songs respond more effectively to the market

---

### User Authentication 

* Overview
	* Authentication
		* Who someone is
	* Authorization
		* What they can do
* How do we authenticate people?
	* username (email) and authentication with a password (key, token, etc)
	* identifying info (username) is unique
		* if it weren't we would have problem associating a username with a unique user in our system
* Authentication Process
	* User comes to site and logs in
	* To authentiate:
		1. Ask for info + password
		2. Find username by name/email
		3. Compare password given to password in database
			a. If they match, authenticate and sign-them in. Keep track of their username in sessions
				* Just store session id in cookie and store user_id on server side to prevent fraud
			b. If they don't match, ask again
	* Major issue here
		* The password database is not secure - stored as plain text
		* Solution is to encrypt the password
	* Encryption
		* One-way function
			* You can calculate the encrypted function to get password code
			* But if you have the encrypted password, you can figure out the unencrypted password
* User Model
	* Atttributes
		* id
		* email - in case of forgets to reset password
		* password_digest 
			* the helper funcation *has_secure_password* compares hash of password given with the password hash in the database for us
			
**Run Through Building User Auth**

* Find User Auth Gem
	* You can google "User Authenticatino in Rails" to find options for Rails
	* Always try to evaluate whether you should do it yourself or do it with a Gem
		* Gem
			* work mostly done for you, if it's popular it's well documented and it's tested to be secure
			* Dependent on external gem. What happens if it stops being maintained. What happens if it doesn't work on Rails 5? 
* [Built-In Rails Method](http://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html)
	* ```has_secure_password```
	* Part of active record
		* anything that inherits form ```ActiveRecord::Base``` can use this
	* Include the gem file ```gem 'bcrypt'```
	* database should have column called ```password_digest```
	* virtual attributes
		* attribute ```.password``` is an attribute, but it's not stored in memory. Only saves the hashed form to the database. We can never retrieve the .password text form.
* Steps
	1. Create a new app called tunr with a postgresql database
	2. Create db
	3. Create and rake migration
	4. Create user model
	5. Create user_controller
		* RESTful methods (index, show, new, create, edit, update, destroy)
		* Permit (:name, :email, :password, :password_confirmation)
			* password & pass conf are not attributes we're collecting, but they are attributes that will be coming in via params
	6. Create views
	7. Create form
		* ```f.password_field :password```
		* puts dots as you type
		* field has password and password confirmation fields
	8. Load bcrypt gem file into Gemfile
	9. Bundle install
		* Everytime you add a new gem you need to run this to install new gems
	10. Create new controller to handle logins
		* ```sessions_controller.rb``` because logging in is just creating a new session
		* what actions do we need in the sessions controller?
			* new session = form to log in
			* create (where we do authentication)
			* end a session = destroy
			
		```
		class SessionsController < ApplicationController
			def new
			
			end
			
			def create
				@user = User.find_by(email: params[:email])
				if @user.authenticate(params[:password]) 
					session[:user_id] = @user.id
					redirect_to @user
				else
					render 'new'
				end
			end
			
			def destroy
				session[:user_id] = nil
				redirect_to root_path
			end
		end
		```
		
	11. set up routes - not resourceful
		
		```
		get '/login' => 'sessions#new'
		get '/logout' => 'sessions#destroy'
		post '/sessions' => 'sessions#create'
		```
			
		* destroy isn't super-destructive so it doesn't have to be destroy request
		
	12. Create sessions views
		* ```views/new.html.erb```
		
				<h1>Login!</h1>
				<%= form_tag sessions_path do %>
					<%= label_tag :email %>
					<%= text_field_tag :email %>
					
					<%= label_tag :password %>
					<%= password_field_tag :password %>
					
					<%= submit_tag :login %>
				<% end %>
				
		* Note: ```form_tag``` is for when you're building a form for something that isn't a model
 	13. Method that tells us about our current user application-wide should go into the **application controller**
 	
 			helper_method :curent_user
 			def current_user
 				if session[:user_id]
 					User.find(session[:user_id])
 				else
 					return nil
 				end
 			end
 			
 		* Can be used in any controller now. But to get it to be used in any view as well, you have to add the helper_method
	
* **Note**: To see error on create, save, or delte add bang and it will show you error
* **Note**: How to show error - something called a flash, which displays things to the user

**What to do with user auth**

* Can't edit user unless you are that user
	
		def edit
			@user = User.find(params[:id])
			if current_user == @user
				render 'edit'
			else
				redirect_to root_path
			end
		end
		
	* You should duplicate the above if statement in update for added security
* Say there are parts of the app you want to require login, if you're not logged in you will be redirected to the login page, otherwise you will be served the page:

	* In application controller:
	
			def authenticate
				if current_user == nil
					redirect_to login_path
				end
			end

	* put this method in the User_Controller before new, create, edit and update and now no one can edit users without themselves first logging in. 
	
			before_action :authenticate, only: [:new, :create, :edit, :update, :destroy]
			
		* the above code will do the same thing as putting this method in each of new, create, edit, update and destroy, but won't give us a multiple redirect error. 
			* before action knows that if action return something false it will halt right then and there
		* runs method we specify before every action by default (with only you can specify which) AND if the method returns something false then it will halt and return whatever the method says
		
---

### Tunr Homework

* ERD
	* Users
		* id
		* name (string)
		* email (string)
		* password_digest (string)
		* admin (boolean)
		* balance (integer)
	* Songs (iTunes API)
		* id
		* title (string)
		* album (string)
		* preview link (string)
		* artwork (string)
		* genre (string)
		* artist_id (FK)
		* Relationships: 
			* Many Songs to One Artist
	* Artists
		* id
		* name (string)
		* photo_url (string)
		* nationality (string)
		* Relationships:
			* One Artist to Many Songs
* Songs
	* Arranged in nested arrays and hashes
		* Main collection is a hash with an array of hashes inside
* Seed Artists via McKenneth
	* He'll provide five artists to seed