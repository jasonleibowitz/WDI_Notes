# Special - JS Review Session

April 27, 2014

---

### Topics

* AJAX
	* Configuring server to respond to AJAX requests (with JSON)
	* Configuring functions to run *after* AJAX request completes

---


### AJAX

**How to communicate with the server**

* Without AJAX
 	* Ways to communicate
 		* URL bar
 			* GET request
 		* Submitting forms
 			* POST request
 		* links
 			* GET request
 	* What did server give us back
 		* Whole page
			* page refresh
 		* Instructions to fetch another page
* With AJAX
 	* JavaScript is stuff that happens on the browser
 	* JavaScript knows how to communicate with the server
 	* JavaScript communicate to the server with AJAX
 		* The cool thing about this is that we no longer have to refresh the whole page
* 2 reasons why AJAX requests are important
	* Really cool to see stuff refresh on page
	* Doesn't destroy whatever's already on the page
		* Won't get rid of your tic-tac-toe game, etc.
* How to Make AJAX Requests
	* JavaScript natively knows how to make server requests via XML
	* jQuery lets us make AJAX requests easily

**Code Along**

* New Rails App
	* Create Ajax Controller
		* Index View renders text
		* Pinky Route that renders text
* How to make Ajax request to render Pinky text on index page

	```
	$.ajax({
	url: '/pinky',
	method: 'get'
	})
	```
	* This will return an object that contains the response from the server
		* ```responseText``` is the html of the requested page
		* This html is not easy to work with
			* We don't want an entire page, we just want certain information
		
**Get JSON from AJAX Response**

* JSON made for JavaScrip
	* Much easier to work with
* Edit controller so it can discern if browser is asking for info or AJAX request
	1. New Route that will return ```pinky_json```
		* Normal '/pinky' will return html and '/pinkyjson' will return a json object
		
			```
			def pinky_json
				info = { character: 'Pinky', catchphrase: 'NARF!' }
				render json: info.to_json
			end
			```
	2. Respond_to - make a decision on what controller sends back based on what request is asking for
	
		```
		  def pinky
    		respond_to do |format|
      			format.html {}
      			format.json do
        			render json: { character: 'Pinky', catchphrase: 'Narf!' }.to_json
      			end
    		end
  		end
		```
		* Ajax request's dataType should be 'json'
		
			```
			$.ajax({
			url: '/pinky',
			method: 'get', 
			dataType: 'json'
			})
			```

**AJAX Done**

* If you want to run a function after your AJAX request has completed, use the **done** function
	
		```
		$.ajax({
		url: '/pinky',
		method: 'get', 
		dataType: 'json'
		}).done(function(data) {console.log(data)})
		```
* Ajax Response in Sublime (with done)

	```
	$(document).ready(function(){
  		var ajaxResponse = $.ajax({
    		url: '/pinky',
    		method: 'get',
    		dataType: 'json'
  		});
	});
	```
	
	* ```$(document).ready``` means that once the page is loaded this JS will run
	* The variable ajaxResponse will not be available within the document.ready because other functions do not wait for the response to finish before running. 
		* So use ```.done``` to access the variable
		
			```
			ajaxResponse.done(function(jsonObject){
    			console.log(jsonObject);
  			});
			```

**JavaScript is Asynchronous**

* JavaScript functions are asynchronous. It doesn't wait for the thing before it to finsh before running
	* It does this because if the server takes a long time your application will be frozen for a long time
	
### POST Requests with AJAX

* define route in controller
	* in routes set this as post route
* ajax post request with *data*

	```
	$(document).ready(function(){
  		var ajaxResponse = $.ajax({
   	 		url: '/receiveinfo',
    		method: 'post',
    		data: {
      			weather: 'great',
      			reviewsesh: 'ajax',
      			venue: 'ga'
    		}
  		})
	```
* We should grab user data in a form
	* Make a form_tag
	
		```
		<%= form_tag '/receiveinfo', method: 'post' do %>
 			<%= label_tag :name %>
  			<%= text_field_tag :name %>

  			<%= label_tag :catchphrase %>
  			<%= text_field_tag :catchphrase %>

  			<%= submit_tag %>

		<% end %>
		```
		
**AJAX Forms**

* The only difference AJAX makes when using a form is that instead of the browser sending the info across, the browser does.
	* It doesn't change anything with respect to how we do things with our database, how we access things in params, etc.	
* Set form to send AJAX request
	* in form_tag add ```remote: true```
* Set respond_to in controller

	```
	def create
	  @character = Character.create(name: params[:name], catchphrase: params[:catchphrase])
    	respond_to do |format|
      		format.html { redirect to '/' }
      		format.js { }
    	end
	end
	```
* By default the create action will look under create.js.html after an ajax form with remote: true. 
	* Put JS logic in there to add newly created object to page
	
		```
		var character = $('<p>').text('<%= @character.name %>');
		$('body').append(character);
		```
* Things to keep in mind
	* **escaping**
		* There is the potential with user input that the user may inadvertently close string tags. To avoid this we have a method ```escape_javascript```
			* a shorthand for this is ```j```
			
### Drag and Drop

* [jQuery UI](https://github.com/joliss/jquery-ui-rails)
	* Load it up
* List needs id so it can be referenced from JS
	* content_tag_for, if you pass it a model object, it will automatically set the dom_id for it
* Make list sortable
	* -> defines an anonymous function
	
		```
		jQuery ->
  			$('#characters').sortable(
   	 			axis: 'y'
    			update: ->
      			alert('updated!')
  			);
		```
	* indentation takes care of arguments for function, when you stop indenting it stops the function
* Now draggable, but state doesn't save
* Save state of sort order
	* Add new method in controller
		```
		def sort
    		render nothing: true
  		end
		```
	* Update routes
	
		```
		resources :characters do
    		collection { post :sort }
  		end
		```
		* ```/characters/sort``` is now a post route
	* Update index.html to know where to send data of where we make request to
	
		```
		<ul id="characters" data-update-url="<%= sort_characters_url %>">
		```
		* this is url that object should make request to
			* don't want to put actual string of url because if you host site on different sites the string will change, better to use this helper]
	* Create function in js file
	
		```
		$.post($(this).data('update-url'), $(this).sortable('serialize'))
		```
		* $.post is a fast way to make a post request
		* 'update-url' taken from data-update-url from ul tag
		* we ask **sortable function** to give us a serialized version of all elements
	* After we move list we get an array of ids telling us what order items should be in
	* Trick: Log in controller to update position attribute:
	
		```
		def sort
    		params[:character].each_with_index do |id, index|
      		Character.update_all({position: index+1}, {id: id})
  		end
		```
		
**Shortcut for Document.ready**

	```
	$ 
	
	or
	
	jQuery
	```