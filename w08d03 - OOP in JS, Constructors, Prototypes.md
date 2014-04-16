# Day 38 Notes | Week 8 Day 3

April 16, 2014

---

### Learning Objectives

* Understand how call and bind work in JS
* Understand what a constructor is
* Understand how to implement constructors in JS (with 'new')
* Use instaneeof and typeof
* Understand what a prototype is in JS
* Use prototypes to add properties to a 'class' of objects
* Use prototype to build 'inheritance' 

---


### OOP in Javascript

* Objects in JS
	* act a lot like hashes, but their keys can be called with functions
	* Hwoever it's weird becuase it's unlike most other programming languages
* No concept of a class in JS
	* We can build something that *acts like a **class-based OOP***, but it isn't really class-based
	* Instead we have something called **prototypal OOP** (aka prototypal inheritance)
* **Prorotypal OOP**
	* Used in JS and [Self](http://en.wikipedia.org/wiki/Self_(programming_language)
	
### Special Function Methods (Call and Bind)

* **Call Method**

	```
	something func.call(this.val, arg1, arg2, ...);
	
	something func(arg1, arg2, arg3,...)
	```
	
	* When you use parenthesis without call the values of this inside of the function relates to the function object you call it on
	* When you use call, the value of this is the first argument you put in call and all the other arguments are arguments the function would normally take
	
* **Bind Method**
	* Creates a new function whose value is set to whatever we pass in
	
	```
	kanyesBoundLove = kanyesLoveFunction.bind(kanye);
	
	kanyesBoundLove():
		=> "I LOVE Kim"
	```
	
### Make Person Exercise Using a Constructor Function

* Create a function that make a person object when given a name, occupation and loverName:

	```
	function makePerson(name, occupation, loverName) {
  		return  {
    		name: name,
    		occupation: occupation,
    		loverName: loverName
  		};
	}
	
	var tadashi = makePerson("Tadashi", "Kicking Ass", "Graphix");
	```
* Downside to doing it this way is that if you change constructor function, old objects created with it won't get that change

### Using new keyword 

* How to build it
	* call it the name of the thing it's going to build and capitalize is
		* closest thing we have to a class definition in JS
	* now we can just write statements that set properties and they'll be implicitly set on the object
	
	```
	function Person(name, occupation, loverName) {
		this.name = name;
		this.occupation = occupation;
		this.loverName = loverName;
		this.numArms = 2;
		this.love = function() {
			console.log("I love " + loverName);
		};
	}
	```
* Create objects with ```new```

	```
	var adam = new Person("Adam", "WDI Instructor", "Ruby");
	```
	
	* Note we do not need to return anything
	* We dont' need to implicitly create anything inside function
* What does 'new' do?
	1. Creates a new object
	2. Sets obj.constructor -> Person/monkey
	3. Sets object to delegate to Person()'s prototype
	4. Calls Person() with context of obj
		* ```Person().call(obj)```

### Prototypes and Constructors

* No class an inheritance in JS, instead prototypes and constructors 
* **prototypes** are just objects that are empty to start with
	* functions have a special function called *prototype*
	* they have exactly one prototype per function
	* (see diagram in notebook)
* multiple objects created with this same constructor will have the same prototype
	* if we want all monkeys to have a new attribute, we just update the prototype and all objects will have this default value
	* process of looking from object to constructor to prototype is called **delegation**
* functions in constructors vs prototypes
	* if we put function in constructor, each object created gets its own copy of that function
	* however, if we put the function in the prototype, each object gets created with one copy of the function
	* generally, we put **methods in the prototype** for performance reason
* Access constructor
	* ```jb.constructor``` or ```Monkey```
* Access prototype
	* ```Monkey.prototype``` or ```jb.__proto__```
* Add default age property (via prototype)
	
	```
	Monkey.prototype.age = 10;
	jb.age;
		=> 10
	```
* How to know if attribute is native or from prototype: **hasOwnProperty** function

	```
	jb.hasOwnProperty("age");
	```
* Other similar functions:
	* typeof
		
		```
		typeof(jb);
			=> 'object'
		```
	* instanceof
	
		```
		jb instanceof Monkey
		```	

### Type Conversion in JS

* Implicit Conversion

	```
	1 == "1"
	true
	```
	
	* what happens here is that JS converts "1" to a number and it returns true
* NonImplicit Conversion

	```
	1 === "1"
	false
	```
	* using three "=" symbols prevents implicit conversion, so we get false here




---
	
### Misc

* Set a variable equal to a function
	
	```
	var kanyesLoveFunction = kanye.love();
	```
	
	where
	
	```
	kanye.love = > console.love("I LOVE " + this.loverName);
	```

	