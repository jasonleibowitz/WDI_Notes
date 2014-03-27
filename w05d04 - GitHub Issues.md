# Day 24 Notes | Week 5 Day 4

March 27, 2014

---

### Filing GitHub Issues

* Title
* Context 
	* What are you trying to do
		* User Story that you're working on
		* What you're trying to do in the code
	* What do you expect to happen
	* What actually happens (error)
		* What is the error being raised (also should be in title)
		* What is the behavior that you aren't expecting
* Example
	* Title
		* Error creating has and belongs to many relationship between user class and itself	
* Code Snippits
	* If you can't checkout certain code you can post the code snippit as a [Gist](https://gist.github.com) file and past a link to the snippit to your issue
* Mentions
	* You can @ mention an instructor that helped you with something similar and may have certain insight
	* Only do this sparingly
* When to file
	* File issues as we come across them
	* Instructors will review issues at least once a day
* After filing issue
	* Let issue go and pick a different user story that doesn't depend on this issue
	
---

### RSpec Testing in Rails

* [RSpec Rails is a Ruby Gem](https://github.com/rspec/rspec-rails) that helps you use RSpec inside of a Rails app
* ```rails generate rspec:install```
	* add ```--format documentation``` to .rspec file
* Note: Mark test as pending
	* as ```x``` before ```it```
* Test Migration
	* RSpec runs in test mode, so we have to migrate our DB to the test environment
	
			rake db:migrate RAILS_ENV=test
			
* What's different about testing in Rails
	* It's hitting your test DB, which is not preserved at the end. It's wiped. 
* How to run RSpec

		bundle exec rspec

* Seeding DB
	* The test database isn't seeded so you need to create objects in your spec file
	* In the future we can use something called factories - helper methods that build different types of objects for you
	
---

### Many-to-Many Self Joins

* Many-to-Many to self
	* Needs a join table
		* can be called ```friendships```
			* table not holding friends, which is other users, it is a table of the relationship itself
		* containts
			* user_id => friend_id
		* symmetry
			* if user 1 is friends with user 2 that doesn't necessarily mean user 2 is friends with user 1. You have to create two entries of each kind. 
* Friendships Join Table - ```rails generate migration CreateFriendships```
	* needs id column because it's ```has_many through```
	
	```
	create_table :friendships do |t|
		t.references :user
		t.references :friend
		t.timestamps
	end
	```

* Friendship.rb

		class Friendship < ActiveRecord::Base
			belongs_to :user
			belongs_to :friend, class_name: "User"		end	

* User.rb

		has_many(:friendships)
		has_many(:friends, through: :friendships)

			
			
	```
	class Person < ActiveRecord::Base
		has_many :friendships, :foreign_key => "person_id",
			:class_name => "Friendship"
			
		has_many :friends, :through => :friendships
	end
	
	class Friendship < ActiveRecord::Base
		belongs_to :person, :foreign_key => "person_id", :class_name => "Person"
		belongs_to :friend, :foreign_key => "friend_id", :class_name => "Person"
	end
	
	```