# Day 58 Notes | Week 12 Day 3

May 14, 2014

---

### Learning Objectives

* Describe the use of modules
* Refactor a code base based on the principles of good code

### Modules

* Intro to Modules
	* Group code
		* If you have a class with a bunch of class methods, but you never make an instance of the class, use a module
	* Namepsacing
		* Make sure that any methods we write or classes we write or constants we write don't interfere or have the same names as methods, constants as other people wrote. 
		* Make sure ActiveRecord's Base class doesn't interfere with your base class. 
	* Mixins
* Set up a Module

	```
	module ModuleName
	
		# Constants
		SIZE_OF_UNIVERSE = 42
		
		# "Module" methods
		def TestModule.greeting
			puts "Welcome to here."
		end
	
	end
	```
	
	* You can set up **constants** inside modules
	* You can create **Module methods** (similar to class methods) inside a module
	
* Access Module in ruby file
	* First require the module
	
		```
		require_relative '../test_module.rb'
		```
	* Now you can just call methods like: 
	
		```
		TestModule.greeting
		```
	* Access constants in file:
	
		```
		TestModule::SIZE_OF_UNIVERSE
		```
		
		* Need double colon to access constants from module
* Create class in Module
	* Create Cat in module that has an initialize method and call it in the ruby file
	
		```
		class Cat
    		def initialize
      			puts "Meow, cat was born"
    		end
  		end
		```
	* Because class names are constants, call it by: 
	
		```
		TestModule::Cat.new

		```
* Mixins
	* What is it?
		* Taking a bunch of code from somewhere and stick it in somewhere else
	* Example
		* Example from notebook
		
		![From Notebook](/images/w12d03_modulemixins.jpg)
		
		* Put beep method in module and insert module where we want
	* Code
	
		```
		require_relative '../beep_module.rb'
		
		class Toaster
			include BeepModule
		end
		
		toasty = Toaster.new
		toasty.beep
		```
* Where to put modules
	* Put them in the ```/lib``` folder, but you have to tell Rails to require files in the lib folder