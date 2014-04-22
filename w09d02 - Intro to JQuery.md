# Day 42 Notes | Week 9 Day 2

April 22, 2014

---

### Learning Objectives

* Explain difference between JavaScript & jQuery
* Use jQuery to select elements in the DOM
* Use jQuery to manipulate the DOM & CSS attributes
* Use jQuery to animate events
* Use jQuery to set up and handle events

---


### Intro to jQuery

* What is it?
	* A library for JavaScript. 
		* Just a set of methods and objects. Doensn't specify how to set up your app
	* A framework is a type of a library that gives you more structore. 
		* Rails is a framework because it's a selections of methods and objects, but it also builds a new app for you with a pre-determined structure.
* Difference between jQuery & JavaScript
	* When you're writing in jQuery, you're still writing in JavaScript
* Why do we use jQuery
	* Makes our job easier
	* Handles things specific to web programming
		* Simplifies aspects of web programming 
			* finding elements in the DOM
	* Handles animations and events more cleanly
	* Handles cross-browser compatibility for us
	* Does, however, come at small cost of performance
* jQuery Documentation
	* [The Learning Center](http://learn.jquery.com/)
		* Collection of articles
	* [jQuery Documentation](http://api.jquery.com/)
		* What are the methods and how it works
* .min Library
	* Libraries comes in two version:
		* Vanilla version that is human readable and what devs work on
		* .min is a minified version that should be used on websites
* Loading jQuery
	* You first have to load jQuery before loading your own code
	
		```
		<script src="http://code.jquery.com/jquery-2.0.3.min.js"></script>
    	<script src="script.js"></script>
		```
* Namespacing
	* Way of ensuring you don't use global variables
	* Most libraries, if built properly, will use this
	* How to use namespacing:
		* In Ruby we use a combination of modules and classes
		* In JavaScript, if we want to avoid global variables: 
			* we can be really careful and always use var
			* we can always define variables in a function so the variable is only valid within that function. 
				* creating a function will give you a new scope, but what if you want to protect function names?
				* functions can also have attributes
			* jQuery is a function itself. So everything within jQuery is an attribute or property of the function jQuery
	* A big part of programming is managing namespacing
		* we'll see this a lot more when we get to backbone
	
### Code Along

**jQuery Selectors**

* jQuery puts itself in ```$``` so you don't have to type of ```jQuery``` each time.
* Selecting elements that match a string
	* Syntac for jQuery DOM selection is basically the same as CSS
	* Select everything with a given class, header:
	
		```
		$('.header')
		```
	* elements
	
		```
		$('h1')
		```
		* if you just put a pure element name without a ```.```, it selects the element
	* IDs
	
		```
		$('#my-field')
		```
	* [Pseudo Selectors](http://www.w3schools.com/jquery/jquery_ref_selectors.asp)
		* :first-child - ```$("p:first-child")```
		* :last-child - ```$("p:last-child")```
		* Comma separated them adds them together:
			* ```$("h1,div,p")``` will select all h1, divs and paragraphs
* Get all input fields inside form
	* ```$("form input")
* Get the first header element (index element)
	* ```$('.header').eq(0)```
* Every time you run the selector you have to search through the DOM
	* If we need to do something with an object, save it to a variable
	* On any subsequent reference to this variable it doesn't need to search the DOM again
	
**Add Elements**

* ```.append()```
	* If you want to append something to a set instead an individual object, just called .append to the set and it will append the item to every element of the set. 
		
		```
		$("h1").append("<p>This is added dynamically via jQuery!</p>");
		```
* Get text of paragraph

	```
	$('p').text()
	```
* Change text of paragraph

	```
	$('p').eq(1).text("I was just changed using jQuery");
	```
* Show/Hide element
	* ```.toggle```
		* takes argument for speed
* Find
	* ```.find```
	
		```
		$('ul.color-list).find('#favorite-color')
		```
	* ```.parent()```
	* ```.siblings()```
	* ```.last()```
	* ```.next()```

**Manipulating the DOM**

* Call the following on a selector or a nest of selectors:
* ```.html()```
	* will replace the HTML nested inside of it
	* pass it an element or a string
* ```.text()```
	* changes text inside of an html element
* ```.append()```
	* adds to the end of an element
	* appends inside element
	* puts something inside something else
		* since elements can only have one parent, if the element was somewhere else it moves
* ```.appendTo()```
	* Opposite of append
	
		```
		purples.appendTo(songList);
		```
	* Take all purples and put them in the song list
* ```.after()```
	* adds element after closing tag of what you specify
* ```.clone()```
	* copies element. If you move this copy the original element won't be affected
* ```.remove()```
	* removes an element
	
		```
		purples.remove();
		```
		* purples still exists, it just isn't attached to the DOM
		* we can still reference them
* ```.css()```
	* will return the css value of element
	
		```
		purples.css('color')
		=> "rgs(128, 0, 128)"
		
		purples.css("font-family")
		=> "Times"
		
		purples.css("font-family", "san-serif")
		=> purple.css({"font-family": "sans-serif", "color": "red"})
		```
* ```.addClass()```
	* change class of element
	
		```
		purples.addClass("big");
		```
* Note:
	* It's best practice to toggle between classes rather than add css to element, unless you're taking user input and adding to style
* ```.val()```
	* changes the value of input
	* val is for things inside form elements, whereas text is for things inside text elements
* ```.hide()```
	* makes it disappear
* ```.show()```
	* makes it show
* building html element
	* if you pass selector something that looks like html element it will build it for you:
	
		```
		$('<li>hello</li>')
		=> [<li>hello</li>]
		```
	
**Animations**

* animate transparency
	* ```.hide()```
	* ```.show()```
	* ```.fadeIn()```
		* doesn't change width unlike hide/show
	* ```.fadeOut()```
		* doesn't change width unlike hide/show
* you can animate any CSS value with a number
	* limited though
	
	```
	.animate(.animate({	opacity: 0.25,
    					left: "+=50",
   						height: "toggle"
 						})
 	 ```
 * [animation easing](http://easings.net/)
 
 **Events**
 
 * There are a bunch of events
	* Until now we've done ```addEventListeners```
	* jQuery lets us add arbitrary event listeners using ```on```
	
		```
		$('h1').on("click", changeToGreen)
		```
		
	* Because we write on click so often, we have shortcuts for some:
	
		```
		$('h1).click(changeToGreen);
		```
		* click
		* mousedown
		* mouseup
	* When you want to define a function that responds to an event on an element you need to define this (wrap it in a jQuery finder method)
	
		```
		$('h1').mousedown(function(){
			$(this).addClass("huuuuge");
			});
		```
		* jQuery sets the value of this to be whatever was moused down over, but we still need to find it so we can run jQuery functions on it
		
* What triggers events
	* browser runs events in response to a number of actions
		* mouse moving
		* window scrolling
		* resizing
		* keyboard presses
		* clicking into form element
		* tabbing away from form element
	* whenever we click on an element in the DOM, it triggers a click event
		* if we bind a function to that event
* We can create functions that take events as arguments
* When you click in the window it register the event on the most specific thing possible, the most nested div
	* then it goes to the parent element

* Nested events

	```
	$('body').on("click", "div.square", drawOnClick
		
	```
	
	* bind click event to body and run drawOnClick whenever event comes from div.square
	* benefit of adding event handler to parent
		* when adding new divs, we don't care that it doesn't have event handler on it. the parent does. so event will work on it as well
	* we can control this bubbling a little
		* **stop propegation**
			* ```event.stopPropogation();``` - prevents events from bubbling up any further
		* some events have default action browser will do in response
			* if you click a button in a form, the browser will submit the form
				* your handler in js will get run before the form is submitted though
			* what if you want to override default action
				* ```event.preventDefault```