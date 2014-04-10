# Day 32 Notes | Week 7 Day 2

April 8, 2014

---

###Learning Objectives

* Utilize [shoulda matchers](https://github.com/thoughtbot/shoulda-matchers) to test
	* model validations
	* model associations
	
--- 

###Testing ActiveRecord Validations

**Test Presence Of**

* In song class of Tunr app, we want to test that every song in our database should ahve a title
	* Validations happen when we save or create, etc. and object. We can test that ```song.save``` returns true, but that is a bad test.
		* It would work if validations were not in place
	* So test that a song without a title should return false
		* Also a bad test. What if we write a validation in the future that requires genre not to have any spaces in it. This test will still pass though. 
	* How to test that a song should have a title then?
		1. Generate a new song, that is completely valid in all respects except TITLE
		2. Try to save that song, expect false
		3. Give that song a title
   		4. Try to save the song now, expect true
* This causes many problems. The **shoulda matchers** gem will do step one above depending on the current state of the model. 

**How Shoulda Matchers Work**

* Validate presence of title

		 it "should have a title" { should validate_presence_of(:title) }
		
	* That one line does all four steps listed above without us having to do them explicitly. 
	* Syntactic Sugar
		* Don't need a do/end block
		* Don't need the description after it because this method does it for us
		
		```
		it { should validate_presence_of(:title) }
		```
* [Other validation tests](https://github.com/thoughtbot/shoulda-matchers)
	* should have_secure_password
	* should validate_uniqueness_of

**Uniqueness Validation**

* Error with Password Digest for User
	* There's a mismatch between how shoulda matchers should been creating and saving objecst and how it thinks it should be saving objects, which is with a password_digest. 
	* How do we solve this problem? 
		* The problem is with uniqueness shoulda matchers will look for a pre-existing record in the db, and if it can't find one it will create one. The problem is that it doesn't know how to create one. 
		* The solution is to create a record before we run the test
		
		```
		 it do
    		patrick = User.create!({
        		name: 'Patrick',
        		email: 'patrick@star.com',
        		password: 'spongebob',
        		password_confirmation: 'spongebob',
        		admin: false,
        		balance: 500
      			})
    		should validate_uniqueness_of (:email)
  		end
		```
		
**Test Associations**

* [ActiveRecord Matchers](https://github.com/thoughtbot/shoulda-matchers#activerecord-matchers)
	* Example for Songs
	
	```
	it { should belong_to :artist }
	it { should have_many :purchases }
	it { should have_many(:users).through(:purchases) }
	it { should have_and_belong_to_many(:playlists) }
	```
	
**In Class Exercise**
	
* Practice Making a Rails App
* Practice TDD Skillz
	* Red
		* Test first
	* Green
		* then code
	* Refactor
* Reps with shoulda matchers

---

### Reflect - Where We Are

* What we've done so far
	* Ruby - Programming Fundamentals 
		* Variables
		* Scope
		* Methods
		* OOP
	* Command Line Interface
	* Git
	* TDD
		* RSpec
		* Unit Testing
	* Web/Internet
	* SQL
	* Sinatra
	* Rails
* What's Left?
	* JavaScript
	* JavaScript + Rails
		* JQuery
	* Advanced Rails Techniques 
		* Optimization
		* Caching 
			* Make app serve pages faster and handle large loads easily
	* Computer Science
	* JS Frontend Frameworks
		* Backbone
		* Angular 
		* Where entire app is written in JavaScript
	* How to work in a group
		* Project 2
**This week's schedule**
* Monday
	* JSON
	* Asset Pipeline
* Tuesday
	* Job Search Standup
* Wendesday
	* You Day - Halfway point
		* Do whatever you need to get ready for JavaScript
* Thursday
	* Closures
	* Callbacks 
* Friday
	* Capybara
	* Lab
* Weekend
	* Review session (2 hours)
* Monday & Tuesday
	* JavaScript
	
---

# Lab