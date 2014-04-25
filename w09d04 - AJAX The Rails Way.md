# Day 44 Notes | Week 9 Day 4

April 24, 2014

---

### Learning Objectives

* Use and Explain the ways that Rails & JS can interact
* Use and Explain trade-offs of different ways of integrating
* Pass data from Rails to JS
* Understand concept of unobtrusive JS
* Use UJS helpers to reuse partials

### Pirates vs Ninja

* Plain Inherently Rail-y Apps That're Essentially Static
	* This is what our project 1 was
	* Pros
		* Simple
		* Maintainable
		* DRY
		* Search Engine Optimization
		* Speed
	* Cons
		* Not interactive
		* Rudimentary UI
		* Requires page refresh
		* Speed
* Nicely Interactive Nothing but JavaScript Apps
	* Framework examples
		* backbone
		* meteor
		* angular
		* ember
	* these frameworks do all rendering and take all data and put it into view
	* Pros
		* Speed
			* Server's only job is to create JSON and the front-end's job is to render it into HTML. We're offloading a lot of the work of loading HTML to the client. This can often be a much lighter load on your server
		* highly interactive
		* offline functionality
	* Cons
		* non-DRY
			* view code is in the JS
		* Bad SEO
			* if you don't have static content renders, the spiders (web crawlers) have no way of interacting with your content. They can't see your index or content unless you take special measures, which increases complexity. 
		* Complex
		* Immature
		* Give your algorithms to the client
		
### AJAX 'The Rails Way'

* Hari-Style AJAX
	* We were hand-writing our JavaScript.
	* We were hand-coding a ninja app
	* There is no non-AJAX method to handle showing hoots

**Code Along**

* Add Hoots to index page with a partial
	* The partial should render one hoot
	* You can iterate over each hoot and ```render current_hoot``` or just ```render @hoots```
* Pass data from JS to Rails
	* Make a request with AJAX
* Pass data from Rails to JS
	* Option 1 - Not Elegant (Obstrusive JavaScript) - in views
		* Create a global variable using JavaScript in view
	
			```
			<script>
  				window.id_to_alert = <%= @id_to_alert %>
			</script>
			```
		
		* Alert:
	
			```
			alert(window.id_to_alert)
			```
		* Ideally all JS would live in the javascript file
	* Option 2 - Data Attributes
		* introduced in HTML 5
		
			```
			<div class="hoot-content" data-author-name=<%= hoot.author %>>
			```
			* do not affect rendering to user at all, only affect how we render things to page and so JS can consume data. 
			* we can access these data attributes to get info about this hoot
		* Access these attributes
		
			```
			$('.hoot-content).eq().data()
			=> Object {authorName: "McKenneth", id: 1}
			```
		* Since this is an object you can chain on to it
		
			```
			$('.hoot-content).eq().data().id;
			```
		* Rails automatically translates from HTML naming conventions to JS naming conventions for us
			* ```data-author-name``` => ```authorName```
			* ```data-id``` => ```id```
* Data Attributes via a method: **content_tag**
	* Rails gives us a helper to create data content tags so it looks nicer and is easier to read/maintain
	
		```
		<%= content_tag(:div, class: "hoot", data: {id: hoot.id, author_name: hoot.author }) %>
		
			<!-- content here -->
			
		<% end %>
		
		```
	* Set alert for first hoot's id using content_tag (no JS in html view)
	
		```
		var id_to_alert = $('.hoot-content').eq(0).data("id");
  		alert(id_to_alert);
		```
* One more thing (about data attributes)
	* instead of specifying all of the attributes we want to put on the content tag, we can put everything at once
	
		```
		<%= content_tag(:div, class: "hoot-content", data: {hoot_info: hoot} ) do %>
		
		<% end %>
		```
	* converts this object to JSON
		* call data like:
		
			```
			$('.hoot-content').eq(0).data("hootInfo").id;
			```
* Why do these data elements exist?
	1. so we can share data with JS
	2. Unobtrusive JavaScript
		* UJS File
		* There are cases where if we attach data properties of certain names, this javascript object will use it for us:
		* Example:
			* Create a button in hoot partial
			
				```
				<button id='<%= hoot.id %>' data-confirm="You sure you want to un-hoot 'n holler?">Unhoot!</button>
				```
				
		* The UJS driver will automatically handle some actions 
			* copy a confirmation box if we have data-confirm attribute
			* data-method
				* default method used by links is GET, but if we want to make a link that has a delete verb, we have to use JS to do that
			
### Force Form to Submit an AJAX Request (not HTTP)

* There's a third type of format the controller can respond to:
	1. html
	2. json
	3. javascript
		* we want to render a view: ```render 'create.js.erb'```
			* in Rails the default thing to do if you don't specify is to render a view of that name, so we don't have to specify ```"create.js.erb"```
		* just write ```format.js { render }
* To force a form to submit as AJAX rather than as a HTTP request you must add remote: true to the form_for

	```
	<%= form_for Hoot.new, remote: true do |f| %>
	```
	
### Review

* How is a form changed to an AJAX request
	* bind an event to the submit button
	* AJAX request is a standard request, but it has a little flag that says "this request came from JavaScript", where the format is set to js. 
		* request isn't handled any differently
* Put HTML into JavaScript variable
	* use escape_javascribe rails helper
	
		```
		var hootPartialHtml = "<%= escape_javascript(render @hoot) %>"
		```
	* short-hand for ```escape_javascript``` is just putting a ```j```
* Pass instance variable to JavaScript partial
	* 

### Misc

* Naming Conventions
	* Ruby
		* snake_case
	* HTML
		* snake-case
	* JS
		* camalCase