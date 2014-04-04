# Day 26 Notes | Week 6 Day 1 - Project Week

March 31, 2014

---

### [How to Create a Custom Rake File](http://railscasts.com/episodes/66-custom-rake-tasks)

1. Lib/Tasks/
	* Create a new rake file, i.e. ```jason.rake```
2. Add a task to your rake file:

		task :greet do
			puts "Hello World!"
		end
3. Run rake task from command line:

		rake greet

*Note*:  You can put as many tasks in your rake file as you want

**Dependency**

* How to run a task before another 
	* Supply a dependency: ```task :ask => :greet do```
		* This will call ```greet``` before ```ask``` everytime you call ```rake ask```

**Example**

* Pick a random user from the User model as the winner of a content

		task :pick_winner do
			user = User.find(:first, :order => 'RAND()')
			puts "Winner: #{user.name}"
		end
		
	* Rake doesn't automatically load your Rails environment
		* To load the environment you need to use a dependency called ```:environment```
		
				task :pick_winner => :environment do
		* This is just a rake task that Rails provides that just loads your Rails environment
		
**Group Tasks Using Namespace**

* Use the namespace method

		namespace :pick do
			task :winner do
				user = User.find(:first, :order => 'RAND()')
				puts "Winner: #{user.name}"
			end
			
			task :prize do
				product = Product.find(:first, :order => 'RAND()')
				puts "Prize: #{product.name}"
			end
		end
		
* Call Namespaced task

		rake pick:prize
		
	* use colon
	
**Create task that calls on both prize and winner so we don't have to specify both**

* Group tasks together

		task :all => [:prize, :winner]
		
**Remove Duplication**

* Create methods within your rake file

		def pick(model_class)
			model_class.find(:first, :order => 'RAND()')
		end
		
	* Edit :prize task to:
	
			task :prize	do
				puts "Prize: #{pick(Product).name}"
			end
			
**Document Tasks**

* You can do this via the ```desc``` method

		desc "Pick a random user as the winner"
		
* Advantage of doing this is that descriptions are availble to you when you run the ```rake -T``` option