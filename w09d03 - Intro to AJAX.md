# Day 43 Notes | Week 9 Day 3

April 23, 2014

---

### Learning Objectives

* Use and Explain how AJAX is different from traditional browser requests
* Construct GET, POST, DELETE AJAX requests to a Rails app
* AJAX requests to a Rails application
* Understand when AJAX is Appropriate

### AJAX

**What is AJAX**

* AJAX stands for Asynchronous JavaScript and XML
* AJAX is not a new programming language, but a new way to use existing standards.
	* Capability of JavaScript
* AJAX is the art of exchanging data with a server, and updating parts of a web page - without reloading the whole page.
	* Modify *some* things in the DOM without reloading page
* Applications
	* Chat without reloading entire page
	* Submit form without having to scroll back down
	
### XMLHttpRequest

* Object that has capability to discreetly talk to a server, i.e. make AJAX requests
	* The work XML is kind of outdated now
* How to use?
	* Instantiate a new one: ```var myRequest = new XMLHttpRequest();``` then add parameters to it
		* Not easy to use, kind of cumbersome
	* jQuery made this easier for us: ```$.ajax```
	
### [jQuery AJAX](https://api.jquery.com/jQuery.ajax/)

* Important Parameters
	* url
		* we are trying to construct an HTTP request. that request needs to know where to go
	* method
		* HTTP request needs to know its verb, what it's supposed to do
	* dataType
		* what do we expect the response to be? 
		* what kind of data are we expecting? 
			* JSON, XML, HTML, etc?
	 	* if we request JSON, then the server needs to know how to respond to JSON
* Constructing an AJAX request in Chrome Dev Tools

	```
	$.ajax({
			url: '/',
			method: 'get',
			dataType: 'html'
			})
	```
	
	* Will return an object
	
* Set up in Rails
	* Have Rails respond to request in JSON by returning all hoots in json format
	
		```
		def index
			@hoots = Hoot.all
			respond_to do |format|
				format.html
				format.json do 
					render json: Hoot.all.to_json
				end
			end
		end
		```
* Put JSON objects in variable

	```
	var hootsResponse = $.ajax({
		url: '/hoots',
		method: 'get',
		dataType: 'json'
		})
		
	var hoots = hootsResponse.responseJSON
	```
	
* Create h4 with content of first hoot

	```
	$('<h4></h4>').text(hoots[0].content)
	```
	
	* Append to body of page
	
		```
		$('<h4></h4>').text(hoots[0].content).appendTo('body')
		```
	* Or iterate over each and append
	
		```
		for(var i = 0; i < hoots.length; i++){
			$('<h4></h4>').text(hoots[i].content + " posted by " + 			hoots[i].author).appendTo('body')}
		```
		
* Move code to Rails
	* JS in Rails is only run when it's sent to browser
* **Promise Function**
	* When we put JS in the document.ready part of the page, the rest of the code will not wait until the AJAX request is received before running. This gives us an undefined error. 
	* To fix this we use a promise function to make our code run only when the AJAX request receives its response:
	
		```
		  var hootsResponse = $.ajax({
    		url: '/hoots',
    		method: 'get',
    		dataType: 'json'
  			}).done(function(){
    			var hoots = hootsResponse.responseJSON;
    			$.each(hoots, function(index, hoot){
      			$('<h4>').text(hoot.content).appendTo('body');
    			});
  			});
		```
		* done means when the request was sent out and received successfully
			* meaning no errors
	* We can pass in arguments to the promise function
		* instead of defining ```var hoots = hootsResponse.responseJSON``` we can pass it in to the done function:
		
			```
			var hootsResponse = $.ajax({
    			url: '/hoots',
    			method: 'get',
    			dataType: 'json'
  				}).done(function(data){
    				$.each(data, function(index, hoot){
      				$('<h4>').text(hoot.content).appendTo('body');
    				});
  				});
			```
		* data will be a response property for whatever you asked for
* Problem: Everyime we run ```fetchHoots()``` we have multiple versions of our hoots on the page
	* Edit AJAX function to only append the ones that aren't on page already
		* Advantage - We're appending less things to the DOM
	* Other option is to trash everything on the page and re-append
		* No read difference because browsers are really fast
	* How do do this:
		1. Create div inside which you'll put all hoots
		
			```
			<div id="hoots">

			</div>
			```
		2. Before you run JS method to append hoots, empty div
		
			```
			.done(function(data){
    		$('#hoots').empty();
			```
**Create a new Object using AJAX**

* Create an input box

```
function createHoot(){
  var userContent = $('#content').val();
  var userAuthor = $('#author').val();
  $.ajax({
    url: '/hoots',
    method: 'post',
    data: {content: userContent, author: userAuthor}
  }).done(function(){
    fetchHoots();
    $('#content').val("");
    $('#author').val("");
  });
}
```

**Destroy Objects via AJAX**

* Create a button on each hoot that has the id of the object
* Add listener to these buttons with ajax delete method
* Find_by vs Find
	* find will return an error if what you're looking for doesn't exit, whereas find_by will return nil.
	
### When Is AJAX Appropriate

* When we want to talk to the server, but we can't afford to lose the page we're on


### Project 2 Overview

* Objectives
	* Work with other developers on a common code base
		* Using git effectively (managing branches)
			* Work Flow: 
				* Master branch with initial commit
				* Immediately branch off into a development branch, this is where you as developers merge changes and test how they work together. (Called 'developer-development' or 'integration branch')
				* When it's time to work on a new feature one person branches off to a new feature and makes commits. 
				* You eventually merge your work back into development
					* Here you may have a merge conflict, so you resolve that. 
						* one branch merged in you delete feature branch
				* Development branch may not be stable
					* if you have some instability, you may branch off a new branch to fix it or make a commit to resolve it
				* When you have a development version that has integrated feature work and is also stable, then you can merge it back into master
			* Each day there will be a git tzar who will make sure development branch is in a stable state and push project to heroku. 
		* morning scrums
		* divide work logically
			* deal with merge conflicts
	* Manage team interactions in collaborative software development environment
		* separate personal concerns from professional concerns
		* as a team realizing what you can do
			* scoping your project - don't take on too much, or take on too little
		* take advantage of team environment
	* Build a complex project involving asynchronous data exchange between the front-end and back-end
		* use what we've covered this week (JavaScript)
		* Do not only build a simple CRUD app without JS
		* Build a Rails app with JS
		* Authentication
		* At least 3 models
		* AJAX
	* Ensure code quality and adherence to a consistent code style
		* Same things we discussed in project 1
		* Semantic code
		* Well-tested (TDD)
			* Capybara
			* Jasmine
			* RSpec
		* User Stories
	* Present your work effectively to the coding and non-coding public
* Teams
	* Team Hedy Lamar: Eric, Jason, Stephen M, Verner

	 	
### Misc

**Pro Tips**

* New line in Chrome Dev Tools - ```Shift + Enter```
* jQuery stands for: "JavaScript Query"
	* we do a lot of finding (i.e. querying) with it