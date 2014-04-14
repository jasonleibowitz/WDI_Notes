# Day 36 Notes | Week 8 Day 1

April 14, 2014

---

### Learning Objectives

* State JavaScript's initial role on th eweb
* State other applications of JavaScript
* State syntactical & conceptual differences between JavaScript and Ruby
* List the datatypes that exist in JavaScript
* Explain what JavaScript primitives are
* Utilize basic JavaScript control flow structures to write JS programs 

---

### Intro to JavaScript

**Purpose**

* Interactivity (with user)
* Event-driven
* JS allows us to **manipulate the DOM** without requesting new information
	* In Ruby if we want to have a menu show we'd have a user click a button that sends a new request to the server, which then sends back a new page with all the same content plus a menu
	* DOM - Document Object Model
		* Take structure of webpage and translates it into model that we can manipulate
		* Manipulating it allows us to change certain things within the DOM: text, styles, adding/removing stuff, etc.


**Triforce of the front-end web**

* HTML
	* Responsible for structure
* CSS
	* Style, makeup
* JS
	* Function

**What's Different between JS and Ruby?**

* Stricter
	* No training wheels in JS
		* ```;```, ```()```, ```{}``` are no longer optional. They must be put in.
		
**How to run JS on our local?**

* node
	* It's an alternative backend web server framework that runs on JavaScript and not on Ruby
	* However, we're going to take that monstrous beast and provide just one small tool it provides us, it's **REPL**
	* i.e. we're going to use node as if it were pry
	
**Writing JS**

* variable declaration
	* In Ruby we only had to do ```a = 10``` to set a variable. 
		* If ```a``` doesn't already exist, it'll create it for us. 
	* In JS, if we want to create a variable we must first declare it: ```var a = 10;```
		* We also have to end statement with a semicolon ```;```
		* If I want to use a later, I don't need to declare ```var```, because ```a``` already exists
* Datatypes
	* JS has its own datatypes:
		* *number*
			* no difference between integers and floating point
			* every number is treated as a floating point number
		* *string*
		* *boolean*
		* null
			* it's like nil
			* means "nothing"
		* undefined 
			* like a super-charged version of nothing
				* means a greater level of not existing than null
			* not like it exists and has no value, it doesn't even exit
			* we'll see this when we talk about objects
		* *arrays*
			* *no hashes*
		* objects
		* **primatives**
			* core core datatypes that are not objects
			* treated specially, not like generic objects
			* passed by value, not by reference
				* as opposed to in ruby, in JS when we set ```b = a```, JS makes a copy of a's value rather than pointing b to a's value. (see diagram in notebook)
			* has no methods or attributes
				* has nothing, just it
	* make a string
	
		```
		var c = "hello";
		```
		* JS uses both single and double quotes. We decide which to use depending on what types of quotes are inside our string. 
			* If string has single quote, use double quotes, and vice versa:
			
			```
			var msg1 = "It's going to rain today."
			```
* Errors
	* ```NaN``` - not a number
		* if you try to take the square root of -1
		* if you try to multiply a string and a number together 
* False Values
	* null
	* undefined
	* 0
	* empty string: ```""```
	* *Note*: empty arrays are true: ```[]```
* Arrays
	* How to put stuff into it
	
		```
		var arr = [];
		
		arr.push(5);
		arr.push(12);
		
		```
	* Note: you **cannot** push elements into your arrays with the push symbol ```<<```
	* How to remove an element from an array
	
		```
		arr.pop()
		```
* ```Console.log();```
	* This will be our new puts
	* anything you want putsed to the screen, put it inside the parenthesis
	
		```
		console.log("Boop");
		```
* Conditionals in JS
	* If syntax: if, in parenthesis my condition, then, in curly braces, my statement
	
	```
	if (a > b) {
  		console.log("a is greater!");
	} 
	```
	* else:
	
	```
	if (a > b) {
  		console.log("a is greater!");
	} else {
  		console.log("a is not greater!");
	}
	```
	* elseif: 
	
	```
	var a = 10;
	var b = 20;

	if (a > b) {
  		console.log("a is greater!");
	} else if (a < b) {
  		console.log("a is not greater!");
	} else {
  		console.log("ala, they are equal");
	}
	```
	
* Comments in JS

	```
	// comment
	
	```
	
	```
	/* creates a section of comments
	that you can close with */
	```
* New Operators
	* Increment Operator ```++```
		* adds one
		* ```i++;``` will return old value of i
			* same as ```i += 1```
		* ```++i;``` will return the new value of i
	* Decrement Operator ```--```
		* subtracts one
	

				
**Vocab**

* node
* variables
* null
* undefined
* primatives

---

### Review of Morning

* Used as a web framework in node
	* Node
		* Framework run on JS
* Language used in game development
* How does JS differ from Ruby?
	* much stricter
	* less room for optional spaces and parenthesis 
	* in terms of datatypes
		* primitives are not objects 
	* setting ```b = a```
		* unlike in Ruby, JS makes a copy of a's value and sets b equal to it
		
### More JS

**Incrementing**

* Until now we've been setting a variable, and then incrementing in a while loop:

	```
	var age = 50;
	var bumpNumber = 1;
	
	while (bumpNumber <= age) {
		console.log("Bump" + bumpNumber + "!");
		bumpNumber++;
	}
	```
* Easier way to do this is with a **for loop**:

	```
	for (initial statement, condition, post-check statement)
	```
	
	* How would a for loop for our incrementing look?
	
		```
		for(var bumpNumber = 1; bumpNumber <= age; bumpNumber++) {
  			console.log("Bump" + bumpNumber + "!");
		}
		```
* Switch loops
	* Allows you to create conditionals for numerous cases. 
	* Exercise:
		* Create a calculate in js with a switch statement to the operator:
		
		```
		var operand1 = 4;
		var operand2 = 7;
		var operator = "multiply";
		var answer;

		switch (operator) {
  			case "add":
  			answer = operand1 + operand 2;
  			break;

  			case "subtract":
    		answer = operand1 - operand2;
    		break;

  			case "multiply"
    		answer = operand1 * operand2;
    		break;

  			case "divide"
    		answer = operand1 / operand2;
    		break;
		}	
		
		console.log("Answer: " + answer);
		```

	* we need breaks otherwise loop won't work
		* get out of case statement
	* create a case in case nothing matches
		* default case
		
			```
			default:
    		answer = "Check your math, bro.";
    		break;
    		```
    		
### JS Nitty Gritties
 
* Naming Varialbes
	* Most things in JS are camel case
* Objects
	
	```
	var obj = {
		name: = "Hari",
		age: 50,
		occupation: "Awesome"
	};
	```
	
	```
	console.log(obj.age);
	
	=> 5
	```
	
	```
	obj.age = 5;
	```
	* this changes the age value
	
	* Add properties to obj
	
	```
	obj.boop = "cat";
	```

* Object literals
	* An object literal is a list of zero or more pairs of property names and associated values of an object, enclosed in curly braces ({}).
	* A property of an object can be either a value *or a function*