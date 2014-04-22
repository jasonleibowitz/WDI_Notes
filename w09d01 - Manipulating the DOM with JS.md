# Day 40 Notes | Week 9 Day 1

April 21, 2014

---

### Learning Objectives

* Use and Explain the HTML DOM
* Use js to manipulate the DOM
* Describe the importance of window on load
* Use alert() & prompt() for basic user interaction
* Set up event listeners on DOM elements
* List common DOM events

### What is the DOM

* Document Object Model (DOM)
	* For any webpage there exists a model of that webpage that consists of many objects that are all connected to each other in some way. That represents the document in question
* Node
	* used to describe general programming aspect, called **trees**
	* Each box on DOM diagram is called a node
* Syntax
	* Parents
	* Child
	* Sibling
* What's great about the DOM?
	* It turns out that the DOM exposes itself as an object that we can interact with in JavaScript
	* In JS there is a *global object* we interacted with. 
		* In the browser, the *window* is the global object
	* Document
		* One of the properties of the global object window. 
		* The DOM lives inside this property. 
* Interacting wtih the DOM
	* The first thing we need to be able to do is to **traverse** the DOM
	
* Code Along
	* View DOM in Console
		* Type ```document``` in console
	* View a specific element with an id
		* ```var pageTitle = document.getElementById("page-title");```
		* ```pageTitle.parentNode``` will displays the parent node of this element
		* ```document.getElementsByClassName("list-item");```
	* Traversal
		* ```firstChild```
		* ```lastChild```
		* ```childNodes```
		* ```parentNode```
		* ```nextSibling```
		* ```previousSibling```
	* Creation
		* ```document.createElement('tag')```
			* Pass a tag into it
			* But after we create the tag, it won't show up in the document because 
			it hasn't been attached to anything yet
			
		* How to attach text to tag:
			
			```
			var heading = createElement("h2");
			heading.innerHTML = 'Hi There';
			
			or 
			
			heading.textContent = 'howdy doo';
			```
			* Not all browsers support all of these method. 
				* textContent only supported by Chrome
				* innerHTML by most
	* Put New Tag on Page
		* ```Node.appendChild(childnode);```
			* appends node as last child of parent
		* ```Node.insertBefore(newNode, referenceNode);```
		* ```Node.replaceChild(new, old);```
			* Take one node out and replace with new node
	* Grab body tag
		* ```document.body```
		* Append new node to bottom of body:
		
			```
			document.body.appendChild(heading);
			```
	* Add class or id to node
		* ```fourth.id = "best-li-ever";```
		* ```fourth.className = "list-item";```
* Style with JS
	* style
		* ```title.style.color = "red"```
* Node properties
	* id
	* className
	* style
		* color
		* font-size
		* display
* Load js file into browser
	* Just link to the js file
		* ```<script src="app.js"></script>```
	* But this loads files in the wrong order. We want files to be loaded after the window is loaded. So use ```window.onload```:
	
		```
		window.onload = function(){
  			var body = document.body;
  			var title = document.createElement('h1');
  			title.textContent = "Page Title";
  			title.id = "page-title";
  			body.appendChild(title);
		};
		```
		
	* You need to be aware of when you want certain things to be run
	
### Alerts & PRompts

* Alert is a pop-up that will pause page-load until user clicks ok or cancel

	```
	alert("Title created. Click OK to make Nav Bar.");
	```
* Prompt is a pop up and can gather user input

	```
	var userResponse = prompt("Please enter your password");
	```
	* Not the best way to gather user data since we have forms, but it's useful for quickly checking something
	
### Event Listener

* Can do something when the user does something
	* A very useful event to listen for is *click*. 
* How to set up event listener for user clicks:
	1. Option 1
		1. Create a function in the js file
		2. In the html inside a tag make an onclick equal to the function:
	
			```
			  <h1 onclick="alertMe()">Test Heading</h1>
			```
	2. Option 2 
		1. Can also set a variable's onClick's attribute to a function when it's being created:
		
		```
		var title = document.createElement('h1');
  		title.textContent = "Page Title";
  		title.id = "page-title";
  		title.onClick = whoIsThis;
  		body.appendChild(title);
		```
		* The problem with this is that we can only have one function assigned to the onclick property. 
	3. Option 3 - Best Way
		* addEventListener()
			* function whose job is to register some instructions to a particular event that may happen to the object its called on. 
			* Will add an event to listen to and also associate that event with a particular set of code to be run
			
		```
		title.addEventListener("click", makeBlue);
		```
		* You can have more than one eventlistener on an object
* Capture form data via event listeners
	* Add input field and submit button to HTML
	
		```
		<h1 id="page-title">World DOMination</h1>
  		<input id="user-input" type="text">
  		<button id="add-to-list-button">Add to list</button>
  		<ul id="list">

  		</ul>
		```
	* Keep track of clicks on the button, then when that happs create a new li of user-input
	
		```
		window.onload = function(){
  			var button = document.getElementById('add-to-list-button');
  			button.addEventListener("click", addInputToList);
		};
		```
	* Create a function to add user input to list
	
		```
		function addInputTolist(){
  			var listItem = document.createElement('li');
  			var userInput = document.getElementById('user-input');
  			var list = document.getElementById('list');
  			listItem.textContent = userInput.value;
  			list.appendChild(listItem);
		}
		```
	* Good practice to place all element grabs in one place at the beginning of the function
* Delete specific list item by clicking on it
	* Delete things with ```removeElement``` or ```removeChild```
	
	```
	var list = document.getElementById('list');
	var bananas = list.childNodes[1];
	bananas.removeChild(bananas);
	list.removeChild(apples);
	```
	* We don't need to know the parent element to get rid of object: ```bananas.parentNode.removeChild(apples);```
	* create even listener to delete item listing when it's clicked
		* do this when it's created
* bind
	* testFunctionbind(userInput)
* remove stale event listener
	* add condition to move to done list that only lets action happen if li is on to do list
		* problem: stale event listener still there
	* create new line item and have it's text content mimic the origin li's text content
		* removes problem of stale functions
* Nodes can only have one parent
	 * you append to a new parents, it removes the association to the old one