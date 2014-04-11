# Day 34 Notes | Week 7 Day 4

April 10, 2014

---

### Learning Objectives

* Describe the characteristics of a closure in programming
* List the 3 (main) closures in Ruby
* Construct a method that accepts a block as an argument
* Construct a method that accepts a proc as an argument

* Scopes to DRY our code
* Understand the ActiveRecord Callback lifecycle


---

### Closures

**What is closure**

* Closure (are code)
	1. Needs to be "pass-around-able"
		* Needs to be chunck of code that our program can pass around to other methods and methods inside methods, just as you would pass an argument, or a number, or a string, etc. 
	2. Remembers all variables that existed (or still exist) in the scope of its creation
		
```
a = [1, 2, 3, 4, 5]
a.each do |el|
	puts el
end
```

* This code runs through each element of the array and puts it
* But what's really happening? 
	* In Ruby there is an Array class
	* Inside that class is a method *each*, which steps through each element in the arry and 
	
		```
		a.each { |el| b << el }
		```

	* the code inside the block is called **closure**
	
* scope
	
	``` 
		def yo(arg)
			arg.length
		end
	```
* Blocks
	* A block is a type of closure in Ruby
	
###Procs

* Stands for **procedure**
	* set of instructions, lines of code
* How to make one

	```
	p = Proc.new { }
	```
	
* How to run a Proc

	```
	p.call
	```
* Create a method to take a proc

	```
	def my_sweet_proccer(code)
		code.call
	end
	
	p = Proc.new { puts "hi there" }
	
	my_sweet_proccer(p)
	```
**Running proc on object**

* Create class with number attribute used in method

	```
	class ProcRunner
		
		attr_accessor :number
		
		def proccer(code)
			@number.times do 
				code.call
			end
		end
	end
	
	p1 = Proc.new { puts "hi there" }
	p2 = Proce.new { puts "Howdeedoo" }
	p3 = Proc.new { puts "cowabunga" }
	
	pr = ProcRunner.new
	pr.number = 2
	pr.proccer(p1)
	# will print 2 hi theres
	
	pr.number = 5
	pr.proccer(p2)
	# will print 5 "howdeedoos"
	```
* add message attribute to class

```
	class ProcRunner
		
		attr_accessor :number
		attr_accessor :message
		
		def proccer(code)
			@number.times do 
				code.call(@message)
			end
		end
	end
	
	p = Proc.new { |msg| puts msg }
	pr = ProcRunner.new
	pr.message = "Hi There"
	pr.number = 4
	pr.proccer(p)
	# will print 4 hi theres
```


**Proc Remembers Variables**

```
	class ProcRunner
		
		attr_accessor :message
		
		def proccer(code)
			code.call(@message)
		end
	end
	
	a = 5
	b = 10 
	c = 15
	
	p = Proc.new { |msg| puts msg*a }
	pr = ProcRunner.new
	pr.message = "Hi There"
	pr.proccer(p)
	# will print 4 hi theres
```

* We can secretly pass *a* to that message and print the message *a* number of times

###Blocks

* Procs are a type of closure
	* We can save Procs in a variable
* But what if we didn't want to go through the process of creating a Proc, saving it in a variable, and then calling it. What if we just want it to exist for a moment, then be destroyed? 

	```
		pr.proccer() { puts "This closures lesson is NOT giving me closure on closures." }
	```
	
	* We can't pass the Proc to any other functions because it's not stored in a variable
	* This momentary instance of the Proc is called a **Block**
		* A block is just a Proc you can't hold on to
* When you pass a block to a method, the syntax of the method has to change

	```
		class ProcRunner
		
			attr_accessor :message
			
			def proccer(num, &code)
				num.times do
					code.call
				end
			end
		end
	```
	
	* The method expects:
		* 1 regular arguments
		* 1 block argument
	* If the method expect some number of normal arguments and a block arguments, the block argument **must** go last
	
* *yield*
	* when you want to call the block, you can just use ```yield```
	
	```
			class ProcRunner
		
			attr_accessor :message
			
			def proccer(num, &code)
				num.times do
					yield
				end
			end
		end
		
	```
	
	* yield means, "I want you to run the block that you find"
	
* Taking off the trianing wheels

```
		class ProcRunner
		
		attr_accessor :message
			
		def proccer(num, &code)
			num.times do
				yield
			end
		end
		
		a = 5
		b = 10
		c = 15
		
		pr = ProcRunner.new
		
		pr.proccer do
			puts "This closures lesson is NOT giving me closure on closures."
		end
```


**Create an each method for a block**

```
class MyArray < Array

  def my_each(&code)
    i = 0
    while i < self.length do
      code.call(self[i])
    i += 1
    end
  end
end

my_arr = MyArray.new

my_arr << 3 << 6 << 4 << 7 << 2

my_arr.my_each do |el|
  puts el
end
```

**Procs vs Blocks**

* Block called outside parenthesis
* Proc called inside

---

### Scope

* Scope - like a saved search
	* takes two arguments
		* Lambdas - like a block
* stabby lambda syntax

	```
	scope(:upvote, -> { where(value: 1) })
	```
	* Any active record finding things can go in the scope lambda 
* This is an active record feature, not a Ruby feature

**Pass argument into scope**

* Vote where post is post object

	```
	 scope(:for_post, -> (post_obj) { where(post: post_obj) })
	```
	
* Scope for finding admin:

	```
	 scope(:admin, -> { where(admin: true) })
	```
	* Can also preset properties on new creation
		* ```User.admin.new``` will create a new user object where admin is set to true
		
### Cacheing

* We have to calculate the score every time we render the show or index page by hitting the database
	* This slows down our program
* So store the result and only change it when it needs to be changed
	* Put it in the database, basically an attribute of the post
		* Add attribute to post called "score"
		
		```
		rails generate migration AddScoreToPosts score:integer
		```
		* Will have to change method "score" later
	* Only change score for specific post whenever someone votes on that post

### ActiveRecord Callbacks

* There are a couple of things that happen in the lifecycle of an ActiveRecord object
	* creating
	* saving
	* updating
	* destroying
* ActiveRecord allows us to request that certain methods be run during those processes
	* In this example we want to say that whenever a vote is created update the score attribute
	
	```
	after_create {  }
	```

		