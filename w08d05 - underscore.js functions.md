# Day 40 Notes | Week 8 Day 5

April 18, 2014

---

### Learning Objectives

* Read & Understand open source code
* Practice presenting technical concepts
* Become familiar with underscore.js
* Become familiar with Chrome Dev Tools

### Underscore

* Array Functions
	* Functions do not modify the array prototype
	* **first**
		* ```_.first(arr)```
		* Passing n returns the first *n* values of the array: ```_.first(arr, n)```
	* **map**
		* ```_.map(list, iterator, [context]) ```
			* list is an array
			* iterator is a function
	* **where**
		* ```_.where(arrayOfObjects, {attribute: value})```
		* will return all objects in arrayOfObjects where block returns true
	* **findWhere**
		* ```_.findWhere(arrayOfObjects, {attribute: value})```
		* will return first object in arrayOfObjects where block returns true
	* **every**
		* ```_.every(arr)```
		* will return true if every element in the array is true, false if there is a false value
		* can also pass block to see if every element passes *truth test*
			* ```_.every(arr, function(x){return x % 2 == 0});```
		* can be used to validate a bunch of data
	* **some**
		* ```_.some(arr)```
		* will return true if any element in the array is true, false if every element is false
	* **find**
		* ```_find(list, predicate, [context])```
		* looks through each value in the list, returning the first one that passes a truth test (**predicate**), or ```undefined``` if no value passes the test. The function returns as soon as it finds an acceptable element, and doesn't traverse the entire list. 
		
		```
		_.find([1, 2, 3, 4, 5, 6], function(num){return num % 2 == 0;});
		=> 2
		```
	* **filter**
		* ```_.filter(list, predicate, [context])```
		* behaves a lot like find, but returns all objects that match predicate, not just first
		
		```
		var evens = _.filter([1, 2, 3, 4, 5, 6], function(num){ return num % 2 == 0});
		=> [2, 4, 6]
		```
	* **union**
		* ```_.union(*array)```
		* Computes the union of the passed-in arrays: the list of unique items, in order, that are present in one or more of the arrays.
		
		```
		_.union([1, 2, 3], [101, 2, 1, 10], [2, 1]);
		=> [1, 2, 3, 101, 10]
		```
	* **intersection**
		* ```_.intersection(*array)```
		* Computes the list of values that are the intersection of all the arrays. Each value in the result is present in each of the arrays.
		
		```
		_.intersection([1, 2, 3], [101, 2, 1, 10], [2, 1]);
		=> [1, 2]
		```
	* **sortBy**
		* ```_.sortBy(list, iterator, [context]);```
		* Returns a (stably) sorted copy of list, ranked in ascending order by the results of running each value through iterator. Iterator may also be the string name of the property to sort by (eg. length).
		
		```
		_.sortBy([1, 2, 3, 4, 5, 6], function(num){ return Math.sin(num); });
		=> [5, 4, 6, 3, 1, 2]
		```
	* **groupBy**
		* ```_.groupBy(list, iterator, [context]);```
		* Splits a collection into sets, grouped by the result of running each value through iterator. If iterator is a string instead of a function, groups by the property named by iterator on each of the values.
		
		```
		_.groupBy([1.3, 2.1, 2.4], function(num){ return Math.floor(num); });
		=> {1: [1.3], 2: [2.1, 2.4]}
		
		_.groupBy(['one', 'two', 'three'], 'length');
		=> {3: ["one", "two"], 5: ["three"]}
		```
	* **invoke**
		* ```_.invoke(list, methodName, *arguments)```
		* calls the methodName on each value in the list. Any extra arguments passed to invoke will be forwarded on to the method invocation
		
		```
		_.invoke([[5, 1, 7], [3, 2, 1]], 'sort');
		=> [[1, 5, 7], [1, 2, 3]]
		```
	* **pluck**
		* ```_.pluck(list, propertyName)```
		* convinient way of grabbing a list of property values
		
		```
		var stoogers = [{name: 'moe', age: 40}, {name: 'larry', age: 50}, {name: 'curly', age: 60}];
		
		_.pluck(stooges, 'name');
		=> ["moe", "larry", "curly"]
		``` 
* Object Functions
	* **keys**
		* ```_.keys(object)```
		* returns an array of the names of an object's properties  
		* if you pass anything other than an object it will return empty array
		
			```
			_.keys({one: 1, two: 2, three: 3});
			=> ["one", "two", "three"]
			```
	* **values**
		* ```_.values(object)```
		* returns an array of all the values of an object's properties
		
			```
			_.values({one: 1, two: 2, three: 3});
			=> [1, 2, 3]
		 	```
	* **pairs**
		* ```_.pairs(object)```
		* converts an object into a list of [key, value] pairs
		
			```
			_.pairs({one: 1, two: 2, three: 3});
			=> [["one", 1], ["two", 2], ["three", 3]]
			```
	* **extend**
		* ```_.extend(desination, *sources)```
		* copy all of the properties in the source objects over to the destination object, and return the destination object. 
			* It's in-order, so the last source will override properties of the same name in previous arguments
		
			```
			_.extend({name: 'moe'}, {age: 50});
			=> {name: 'moe', age: 50}
			```
* Utility Functionsx
	* **escape**
		* ```_.escape(string)```
		* escapes a string for insertion into HTML, resplacing ```&```, ```<```, ```>```, ```"```, and ```'```.
		
		```
		_.escape('Curly, Larry & Moe');
		=> "Curly, Larry &amp; Moe"
		```
	* **times**
		* ```_.times(n, iterator, [context])```
		* invokes the given iterator function **n** times. Produces an array of returned values
		
		```
		_(3).times(function(n){ genieGrantWishNumber(n); });
		```
* Chaining
	* In ruby we can change methods naturally
		* ```arr.map``` returns array. So we can call ```arr.map.each``` and so on
	* In JavaScript this doesn't work
		* ```_.map(arr, ___)``` returns an array, but we cannot call ```_map.(arr, ___).each```
* Chaining Functions
	* **chain**
		* ```_.chain(obj)```
		* returns a wrapped object. Calling methods on this object will continue to return wrapped objects until ```value``` is used
		
		```
		var stooges = [{name: 'curly', age: 25}, {name: 'moe', age: 21}, {name: 'larry', age: 23}];
		var younges = _.chain(stooges)
			.sortBy(function(stooge){ return stooge.age })
			.map(function(stooger){ return stooge.name + ' is ' + stooge.age; })
			.first()
			.value();
			=> "moe is 21"
		```
	* **value**
		* ```_(obj).value()```
		* extracts the value of a wrapped object
		
		```
		_([1, 2, 3]).value();
		=> [1, 2, 3]
		```
* Function Functions
	* **memoize**
		* ```_.memoize(function, [hashFunction])```
		* catches the computed result of a function. Useful for speeding up slow-running computations. 
			* If passed an optional hashFunction, it will be used to compute the hash key for storing the result, based on the arguments to the original function. The default hashFunction just uses the first argument to the memoized function as the key
		
		```
		var fibonacci = _.memoize(function(n){
			return n < 2 ? n : fibonacci(n - 1) + fibonacci(n - 2);
			});
		```


---

### Misc

* Pairwise comparison
	* Refers to any process of comparing entities in pairs to judge which of each entity is preferred, or has a greater amount of some quantitative property.