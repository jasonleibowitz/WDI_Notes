# Day 23 Notes | Week 5 Day 3

March 26, 2014

---

### [Ruby Style Guide](https://github.com/styleguide/ruby)

* What is a style guide
	* Guide to how to style your code
* Why are they important
	* Helps different developers understand each others code
	* Consistency
* Ternary operator (?:)

	```
	# bad
	result = if some_condition then something else something_else end
	
	# good
	result = some_condition ? something : something_else
	```
	
	* Do not nest ternary operators
	* Use only one expression per branch
	
		```
		# bad
		  some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else
		  
		# good
		if some_condition
			nested_condition ? nested_something : nested_something_else
		else
			something_else
		end

		```
* Favor ```if/unless``` usage when you have single-line body

	```
	# bad
	if some_condition
		do_something
	end
	
	# good
	do_something if some_condition
	```
	
---

# API

**Intro**

* User Story
	* As an admin I want to be able to browse or search for songs on iTunes so that I can get inspired to create new songs on my website
	* Requirements
		* Rails server needs to be able to communicate with iTunes and get info from it
		* It's important to know what questions to ask and how to ask them to get the info we want
* API
	* Set of questions we're allowed to ask iTunes and responses iTunes will send back (that iTunes has created)
	* Defines valid requests that will be responded to
* How does it work?
	* Send a GET request to a path ```http://pokeapi.co/api/v1/pokemon/1```
	* The response for this request is what you get back in JSON format
		* It's a hash-like thing that contains the key-ability pointing to the value, which is a value, etc. 
* RESTful APIs
	* Follow the RESTful convention we've been working with:
		* ```resource/:id``` => ```pokemon/1```
		
**How to Access APIs**

* Install HTTParty Gem
	* ```gem install HTTparty```
	* Allows our Rails server to construct HTTP Requests in Ruby and receive responses
* Install json gem
	* ```gem install json```
* Playing in pry

		require 'HTTParty'
		require 'JSON'
		
		type5 = HTTParty.get('http://pokeapi.co/api/v1/type/5')
		
	* We get back the API data as a response and automatically converts it into Ruby format
	* Put data in mental picture (useful for large responses)
		* Response is a hash. What are the keys? 
			* ```type5.keys``` = array of keys
		* What's in name?
			* ```type5["name"]``` => string
		* What's in super_effective?
			* ```type5["super_effective"]``` => array with 5 elements
			
**Bad APIs - iTunes**

* URL
	* ```https://itunes.apple.com/search?term=jack+johnson&limit=10```
		* ```itunes_response = HTTParty('https://itunes.apple.com/search?term=jack+johnson&limit=10')```
		* The datatype of the response is one huge string.
	* Parse data with json
		* ```JSON.parse(itunes_response)```
		* Returns Rubified and organized data
		
---
		
### Creating an API in Tun.r	
	
**Tun.r User Story**

* As an admin I want to be able to search songs on iTunes so I can be inspired to add songs to my database
	* Admin should have a search bar where he can search iTunes info
* Things to decide:
	* What method do we use?
		* GET
			* HTTParty lives in tunr and talks to iTunes
			* the path get 'songs/search' handles the client => tunr communication
			* we can add a search form on the create song page
			* that form will submit its data via the GET method to the '/songs/search' path. 
				* our URL should like like ```localhose:3000/songs/search?keyword=blue```
	* Where should the functionality live
	* Where should it route to
		* Search route
			* ```get '/songs/search'```
	* What controller should control it
		* songs_controller => ```'songs#search'```
* Edit resources in routes
	* Add route to resources collection
		* add search action to collection (applies to all songs)
			```
			resources :songs do
				collection do
					get 'search'
				end
			end
			```
	* Add route to resources member
		* member pplies to one particular song
	* Doing this gives us helper methods, while setting route outside of resources does not
* Create method to search iTunes in tunr's song controller

		def search
			itunes_url = 'https://itunes.apple.com/search?'
			escaped_query = params[:query].downcase.split.join('+')
			itunes_path = "#{itunes_url}limit=10&entity=musicTrack&term=#{escaped_query}"
			
			raw_response = HTTParty.get(itunes_path)
    		response = JSON.parse(raw_response)
    		results = response["results"]
		end
		
	* iTunes needs spaces replaced with plus signs (+), so we escape all the spaces in the query with pluses
* HTTParty requests should live in models
	* We're creating new songs with it
	* This is what should be in the songs_controller
	
		```
		@songs = Song.itunes_search(params[:query])
		```
		
		songs model:
		
			```
		  def self.itunes_search(query)
    		itunes_url = 'https://itunes.apple.com/search?'
    		escaped_query = query.downcase.split.join('+')
    		itunes_path = "#{itunes_url}limit=10&entity=musicTrack&term=#{escaped_query}"

   			raw_response = HTTParty.get(itunes_path)
    		response = JSON.parse(raw_response)
    		# hash of what we want is:
    		return response["results"]
  		end
  		```
  		
  	songs_controller
  	
  		```
  		 def search
    		@songs = Song.itunes_search(params[:query])
 		 end
  		```
  	
* What's still ugly?
	* The iTunes URL is ugly because we keep creating a local variable and destroying it. 
	* Make the url a class variable: ```@@itunes_url = 'https://itunes.apple.com/search?'```
		* attribute that governs all attributes of the class
		* this lives outside of the method at the top of the class
		
---

### Project 1 Overview

* For this first project we should focus more on the process rather than the code
	* Process Aspects
		* User Stories
		* TDD
		* ERD
		* Scrum/Agile
		* Time Management
		* Getting Help
			* How to ask for help effectively 
		* Documenting Work & Sharing
			* Readme File
				* Overview of what project is
				* How to use it
				* Stuff to help instructors work with us
					* Link to ERD
					* Link to Pivotal Tracker
	* Code Aspects	
		* New Technology (both process & code)
			* API
			* Ruby Gem
		* Complex Project
* Timeline
	* Wed
		* Project Framing
		* Ideas Generation
	* Thur
		* Scrum
		* ERDs
		* User Stories
	* Fri
		* Scrum
		* Code
	* Weekend
	* Mon
		* Scrum
		* Code
	* Tue
		* Scrum
		* Code
	* Wed
		* Scrum
		* Presentations
	* Thur
		* Feedback
	* Fri
		* Refactor
* Requirements
	* Rails App with 3+ related models
	* Tests
		* Unit tests for Models (non AR functionality)
	* At least one new technology (API, RubyGem)
		* No JavaScript
	* Readme
		* Conceptual overview of project
		* Getting Started
		* Link to Pivotal Tracker project with User Stories
		* Link to ERD
	