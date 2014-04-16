# Day 37 Notes | Week 8 Day 2

April 15, 2014

---

### Learning Objectives

* List the different types of function definitions in JS
* Differentiate between referring to a function and calling it in JS
* Attach functions to objects to act as "methods" in JS
* Use & Explain the concept of contexts and describe what 'this' is in JS
* Use & Explain hoisting in JS
* Use & Explain how scope works in JS

---

## Functions

* How to write a function in JS
	* **Named Functions**
	
		```
		function myFunc() {
  			console.log("Hey there, from inside a function!");
		}
		```
	* Call a named function
	
		```
		myFunc();
		```
		* parenthesis required
	* **Unnamed Functions** (aka Expressive Function Declaration)
	
		```
		var secondFunc = function() {
  			console.log("Hello from an unnamed function!");
		};
		```
		
		* doesn't run the code
		* creates a function and sticks it into a variable
	* Call unnamed function
	
		```
		secondFunc();
		```
		* parenthesis very important
* Importance of parenthesis in functions
	* ```secondFunc``` refers to the code block that is the function
	* ```secondFunc()``` will run the function
* **First-Class Functions**
	* JS functions can be stored in variabes and passed around just like any other objects
	* Called first-class objects for this reason
* Implicit Return
	* No such thing in JS. You have to **always** explicityly state return
	
### Objects

* How to create a blank object

	```
	var car = {};
	```
* Set object attributes

	```
	car.model = "Buick";
	car.color = "Ugly Brown";
	```

### This

* every function is javascript is run in the context of some object 
	* if we call this inside of a function, it refers back to that functions objects
	
### Scope

* The scope of a variable is limited to within the scope of the function within which it was created
	* really the only rule for scope in JS
* This doesn't count for if, while or for loops
* **global scopes in js**
	* in browser global scope is "window"
* **Polluting the Global Scope**
	* ```var``` does more than just define a variable, it also sets the scope of the variable
		* inside a function if you don't set a variable with var, it will exist everywhere
		
### Functions in JS are Closures 

1. **Pass function as arguments**

	```
	var positiveRemark = function() {
  		console.log("What a wonderful day!");
	};

	var negativeRemark = function() {
  		console.log("There's a snake in my boot.");
	};

	var functionRunner = function(func) {
  		for(var i = 0; i< 5; i++){
    		func();
  		}
	};

	functionRunner(negativeRemark);

	```
	
2. **Functions remember all variables that existed (or still exist) in the scope of its creation**

	```
	var functionGenerator = function() {
  		var name = "Billy Bob";
  		var func = function() {
    		console.log("Hi there, " + name);
  		};
  		return func;
	};
	
	var billyMessage = functionGenerator();
	billyMessage();
	```
	
	* functions that are nested inside a particular scope have access to all the variables that exist in that scope
	* those variables that existed in the scope of the creation of this function are remembered by this function whenever it's called
	
### More on Passing Arguments into Functions

* If you have a function that accepts two arguments, and only pass in one, it will run the function for the first, and return undefined for the second.
* If you pass extra arguments, it will ignore anything beyond two

### Fickle 'this'

* if used in a function doesn't always refer to parent function. Refers instead to whatever it is wrapped in
* **this** depends on where you are
* functions in javascript are closures
	* closures:
		1. passed around
		2. called from different area
	* every function in js is called in a particular context
		* ```mult3``` is pointing to a particular function that is being called *in the context of secondObj*
		* ```multiplier3``` is pointing to the same line of code, but it is being run in the *global context*
		
	```
	var creator = {
  		factor: 5,
  		genMultiplier: function() {
    		var func = function(a) {
      			return a * creator.factor;
    		};
    		return func;
  		}
	};
	
	creator.factor = 3;
	var multiplier3 = creator.genMultiplier();
	// should take a number, multiplies it by 3 and gives it back to me
	creator.factor = 7;
	var multiplier7 = creator.genMultiplier();
	// should take a number, multiplies it by 7 and gives it back to me

	console.log(multiplier3(6));
	console.log(multiplier7(4));
	console.log(multiplier3(6));
	
	var secondObj = {
  		factor: 99,
  		mult3: multiplier3
	};
	
	secondObj.mult3(5);
	```