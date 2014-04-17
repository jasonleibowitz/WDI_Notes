# Day 39 Notes | Week 8 Day 4

April 17, 2014

---

### Learning Objectives

* Set up Jasmine testing environment
* Perform test-driven JS development using Jasmine

---


### Inheritance

* Constructor is not native own property of object, it is defined on prototype
	* Object Model in JS
		* Constructor property defined on prototype
	* Own Property
		* If property is not "own" property of object
		* ```a1.__proto__```
* Inheritance 
	* If Doge inherits from Animal, shibe's property should delegate up to next object. If it's defined on Doge, then great. If it's not defined there, it should look at Animal prototype.
	* Delegation is just a link between an object an another object
* Delegation link
	* The only way to set up a delegation link is by using ```new```
	
![img](w08d03_inheritance.jpg)

* Creating class that inherits from another
	* use ```call``` and run parent constructor first
	
	```
	function Animal(){
		this.location = "home";
		this.carbonBased = true;
	}
	
	Animal.prototype.move = function(newLocation){
		this.location = newLocation;
	};
	
	function Doge(name, level){
		Animal.call(this);
		this.name = name;
		this.amazementLevel = level;
	}
	
	Doge.prototype = new Animal();
	
	var shibe = new Doge("shibe", 10000);
	```
	
* **Shadowing**
	*  changes to a child object are always recorded in the child object itself and never in parents
	* the child object shadow's the the parent's value rather than changing it.
	
### Test JavaScript

* [Jasmine](http://jasmine.github.io/)
	* Behavior-Driven JavaScript 
	* Very similar to RSpec
* Syntax
	* describe is a function that accepts a string and an it as a function
	
	```
	describe("A suite", function() {
  		it("contains spec with an expectation", function() {
    	expect(true).toBe(true);
  		});
	});
	```
* Set up testing environment
	* Standalone Distribution
		* Downloads all the files you need
		* just put the files you need someone
		* Jasmine takes care of the rest
	* lib
		* jasmine's files
	* spec folder
		* put spec files in there
		* files must end in ```**Spec.js```
	* src
		* put js we want to test in ther
* Run tests
	* just open ```spec_runner.html```
	* interface via browser
	* To rerun test, just refresh page. 
	* to run just one test, click it
	
* Create our own tests
	* Circle.js
		
		```
		function Circle(options) {
  			this.radius = options.radius;
		}

		Circle.prototype.area = function() {
  			var area = (3 * this.radius * this.radius);
  			return area;
		};

		Circle.prototype.perimeter = function(){
  			return 2 * 3 * this.radius;
		};
		```
	
	* CircleSpec.js
	
		```
		describe("Circle", function() {
  			it("should have a radius", function() {
    			var circle = new Circle({radius: 5});
    			expect(circle.radius).toBe(5);
  			});
  			
  			describe("#area", function(){
    			it("returns the area of the circle", function() {
      				var circle = new Circle({radius: 2});
      				expect(circle.area()).toBe(12);
   	 			});
  			});
  			
  			describe("#perimeter", function() {
    			it("returns the perimeter of the circle", function() {
      				var circle = new Circle({radius: 8});
      				expect(circle.perimeter()).toBe(48);
    			});
  			});
		});

		```
		
### Hoisting

* Any variable declarations that exist anywhere in the file gets "hoisted" to the top of its scope
	* looks through particular scope and any variable declarations defined within that scope get hoised to the top of that scope.
	* unless variable was set before it was called, you'll get undefined 