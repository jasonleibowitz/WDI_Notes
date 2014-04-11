# Day 35 Notes | Week 7 Day 5

April 11, 2014

---

### Learning Objectives

* Understand and Explain Acceptance tests
* Think critically about the requirements of a user story
* User Capybara to write acceptance tests

### Testing

* Testing we've done
	* **Unit Tests** (Models/Method-level)
		* Types
			* RSpec
			* MiniTest
		* Helps devs implement low-level functionality
			* Necessary to make high-level tests pass
	* Functional (for Ruby: Controllers)
	* **Acceptance** (features)
		* Built around user stories 
		* Drive web interface by clicking links, fill in forms, etc
			* Automated forms of the testing we're already doing
		* For these to run, we're exercising our controllers
			* So in essence our controllers are being tested
		* Types of acceptance tests 
			1. Proof to client that app does what it should
			2. Guides devs at high level
		* Work only on one acceptance test at a time
			* Prevents feature creep
	* Integration
* Acceptance Tests
	* Feature.specs
		* Test a swath of testing functionality in our test
		
* Types of Tests

![Types of Tests](/images/w07d05_typesoftests.jpg)


### Capybara
	
* Another testing framework that can integrate with either RSpec or MiniTest.
	* We will be using Capybara + RSpece
* Drivers
	* Capybara accepts drivers, which is what drives a web page
		* Selenium 
			* Based off Firefox and is one way Capybara can interact with webpages
		* RackTest
			* Headless, means there's no GUI (all happens behind the scenes)
* DOM (Document Object Model)
	* Browser's internal programmatic representation of the document's HTML
	* Analogy
		* Equivalent to JSON parsed into Ruby and turned into a hash. A hash is a programmatic object that we can manipulate (call methods on)
* BDD (Behavior Driven Development) vs TDD
	* TDD done right
	* But basically the same thing
		
### Setting Up Capybara

* Gemfile
	
	```
	group :development, :test do
  		gem 'rspec-rails', '~> 3.0.0.beta'
  		gem 'shoulda-matchers'
 		gem 'capybara'
	end
	```
* spec_helper

	```
	require 'rspec/rails'
	require 'capybara/rails'
	require 'capybara/rspec'
	```
	
* Create the test file in ```spec/features```
	* Every feature spec should correspond to a user story
	* Can do a test based on grouping
		* User auth set ```authorization_spec.rb```
* Write test descriptions for this user story

	```
	Given that I have a registered account
	When I visit the login page
	and fill in my email and password
	and I click on the login button
	then I should be signed in an taken to my profile page
	```

* Test DSL
	* Start with RSpec syntax

		```
		require 'spec_helper'

		describe "A user can sign in" do
  			it "should sign in the user given a valid username and password" do
    	    	User.create(username: 'abray', email: 'adam@ga.co', 
    	    	password: 'qwerty', password_confirmation: 'qwerty')
				# capybara dsl goes here
  			end
		end
	
		```
	
	* Then include Capybara DSL
	
		```
		visit("/login")
		fill_in("Email", with: "adam@ga.co")
    	fill_in("Password", with: "qwerty")
		click_button("Login")
		expect(page).to have_content("Successfully logged in")
		```
		
* Run Test
	* Just run ```rspec``` in command line
		* Will run both RSpec and Capybara
		
* DRY Up Test Code with Methods
	* We're signing in a lot, so create a sign in method:
	
		```
		def sign_in(email, password)
  			visit("/login")
  			fill_in("Email", with: email)
  			fill_in("Password", with: password)
  			click_button("login")
		end
		```
		
* Capybara Language
	* Capybara uses feature instead of describe and scenario instead of ir
	
	
		```
		feature "a user can sign in" do
		
			background { User.create(username: "adam@ga.co", password: "qwerty") }
		
  			scenario "should description" do
    			sign_in("adam@ga.co", "qwerty")
    			expect(page).to have_content("successfully logged in")
  			end

  			scenario "should not sign in a user with an invalid email or password" do
    		sign_in("adam@ga.co", "password")
    			expect(page).to have_content("Invalid username or password")
  			end
		end
		```
	* Find method
	
		```
		find(".vote_score").text.to_i
		```
	* dom_id
		* rails method that shows post_id in css
		
		```
		<div class="post" id="<%= dom_id(post) %>">
		```