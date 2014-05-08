# Day 53 Notes | Week 11 Day 3

May 7, 2014

---

### Backbone Review

**Model**
	
* A way to represent our data on the client-side, all of its attributes and behaviors. 
* Much like in Rails, but the key difference is that all of the data lives on the client's browser. It's not stored there permanently, but stored there while the program is running. Data originally comes from server. 
	* Models are like classes
	* We can use it to generate new instances.
* Standard way to send this data to client is via JSON and updates representations on client-side. Allows syncing between models on client and server sides.
* **Events** on Backbone models are a lot like events on jQuery. Listens for event, then triggers function. 
	* Built-in Model Events:
		* change - triggered any time you call set on a model - change a model
		* destroy 
		* all - listen for any event that fires
		* [more](http://backbonejs.org/#Events-catalog)
* We can set default attributes when we define model attributes
* **urlRoot**
	* Path where resource (to do items) is stored on the server
* *How to Generate new instance (with default done value)*
	
		```
		var TodoItem = Backbone.Model.extend({
			urlRoot: '/todos',
			
			defaults: {done: false}
		});
		
		var getMilk = newTodoItem({content: "get some milk"})
		```

**Collection**

* A set of models
	* Point to a model's class
	* Has a url
		* Instead of creating a new to do and adding it manually, we can just tell the collection to create a new to do and it'll know where to go
		
			```
			var TodoCollection = Backbone.Collection.extend({
				url: '/todos'
				model: TodoItems
			});
		
			var todos = new TodoCollection();
		
			todos.creates({content: "Get Milk"});
			```
* Can add listeners for when the collection is added to, we can then do something to the view

**Views**

* Views have a really important property, their el, which is an actual DOM element that we can manipulate
* A lot of the view logic is related to building the element and listening to events that happen inside the element
	* **Building the element**
		* Like our render function
		* Instantiate a new copy of the view and render it, which will give all the info about the model it's associated with, set stuff on the element (set what the elements HTML is via templating)
		* tagName can set what the element is. default is div
	* **Listening to Events Inside the Element**
		* Ultimately, the element is a div and has things like checkbox, paragraphs, etc. We can say things like, when they click on the span run this function. 
			* We can this is scoped - when you click on span inside *my own element* run the xDelete function
			* The events are all scoped to their own element
	* When we look at the to do example, each of the boxes on the page is its own itemView. 
		* Each item has its own event listeners listening to things on it
		* The view is tied to the model with the data in it. Any functions that run after events run on the view's specific model
		
---
		
### Router

* Two Problems:
	1. Where do things like instantiating new things happen / managing the state of our app
		* In Rails the request comes in, we run the route, then parse it and send response
			* We don't have that model in Backbone - this app is always running on the page
			* So how do we manage initialization of the application
* Show a new page in Backbone & manage history
	* Manage history so app doesn't feel broken to users, and users can click 'back' button
	* Allow users to bookmark part of the application
* No analogy to Rails
	* but basically a combination of controller & routes
* Backbone.Router
	* 2 main things
		* Routes Property is basically the routes - path => action
		
			```
			routers: {"todos/:id": "show",
            		  "": "index"},
			```
		* Initialize is where application initialization happens
		
			```
			initialize: function() {
    		this.collection = new TodoCollection();
    		this.localList = new listView({collection: this.collection});
   	 		this.input = new InputView({collection: collection});
			```
* Create instance of router in document.ready

	```
	todoApp = new AppRouter();
	```
* Actions
	* Index
	* Show
		* links start with hash tags - because it's an anchor link which doesn't tell browser to make full page refresh - we don't have to worry about default action
		
			```
			<a href="#todos/<%%= id %>"Detail</a>
			```