# Review Session

---

### Backbone

**Model Associations**

* We can extend them with any properties we want. Say a model has a comments property. 
* When we populate the collection with json information we can just include comments as well
	* This is happening in the Rails app's jbuilder file. 
	* Attributes are watched by things like change - this is a better way to do it then adding comments in via initialize in the model

**jbuilder**

* Will return a JSON file if the format requests JSON
* What to do
	* jbuilder gem
	* build view.json.jbuilder
	
**Router**

* Best to let the Router handle
	1. Mapping of routes to functions (which are really controllers)
	2. Contain controller functions 
		* The job of the controller functions are basically like the job of the Rails controller: 
			* fetch data
			* manipulate data with help of collections and models
			* manage state of views on page	

**Review**

* When we first make a request to page our **Rails* route connects to our Rails controller, which makes a list of all todos and renders our view page. 
	* This renders a static html page. 
	* Along with this HTML page, the javascript include tags sends the applications.js file
		* This file says we need all the jQuery stuff, underscore, backbone, etc. (the order of this matters)
		* We also instantiate our app router and call the start function on it
* Backbone Router
	* Whenever we instantiate any of the backbone classes, it is going to call whatever initialize function we define
		* The router instantiates a collection, a collection view, and the input view
			* collection - doesn't have initialize, so doesn't do anything
			* collection view - empties itself, then for every collection in it add it to the page. There are no collections yet, so it isn't doing anything yet either.
* Backbone.History
	* This what has the router start handling routes
	* It activates the router to start comparing your current URL to the list of routes it knows about

**Show Route**

* If we create a link on the page using a '#' fragment it won't trigger a new page refresh, but it does trigger the backbone router to take us somewhere else. 
	* the router will listen for a url change to ```todos/:id``` and will take us to the ```todoShow``` route, which listens for the id
	* the show function listens for the id ```function(id)```, and fetches all of the todos in the collections and deletes all until the one we want. 
		* not the best way to do it. 
		
	```
	todoShow: function(id){
		this.todoCollection.reset();
		var currentTodoItem = new TodoItem({id: id})
		currentTodoItem.fetch({
			success: function(){
			todoApp.todoCollection.add(currentTodoItem);
			}
		});
		$('#app).html(todoApp.todoCollectionView.el);
	}
	```

**Routes with History API**

* Using a slash, the browser by default will make a full-page refresh
	* Backbone has something called ```push state```, which isn't supported in all browsers, but it lets the Backboen router pick up routes
* How to set up
	* Set up Backbone.history.start with a push state of true
	
		```
		Backbone.history.start({pushState: true})
		```
	* Create a function that intercepts all links (non fragmented)
		* The router has a navigate function that takes a string "todos/2, {trigger: true}"
			* If you don't pass trigger: true, it'll just change the url. With trigger, it'll also run the associated function. 
		* We need a way to intercept every link, then call ```app.navigate``` and pass this function
			* link-by-link basis
				* in events
				
					```
					'click span.show-detail': 'navigateToDetail'
					```
				* function
				
					```
					navigateToDetails: function(item){
						todoApp.navigate("/todos/" + this.model.get('id'), {trigger: true});
					}
					```
		
	* Create your links without hash (#)
	
		```
		routes: {
  		"help/:page":         "help"
		```
		
**Different Detail View**

* You can have more than one template on an item and render it on different pages, detail vs regular for instance. 

### Page-Specific JavaScript

* Require tree ./common instead of just . because that would require everything
	* Anything not in this list won't be loaded in every page
* Create separate js file that isn't required
	* On specific page include javascript include tag
		
		```
		<%= javascript_include_tag "comments" %>
		```
* In application.rb under module TodoApp add

	```
	config.assets.precompile += %w( comments.js )
	```