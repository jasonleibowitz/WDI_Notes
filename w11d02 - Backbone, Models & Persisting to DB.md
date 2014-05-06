# Day 52 Notes | Week 11 Day 2

May 6, 2014

---

### Learning Objectives

* Utilize models & collections to store data on the client-side
* Sync information between models/collections & server
* Utilize events to maintain sync between front-end & back-end

---

### Making Sure the DOM Loads Before We Need It

* Why did we have to move our javascript include tag under the yield tag in the application layout?
	* Because we were trying to "do things" before the DOM loaded
	* Template errors are usually errored by: ```"Uncaught TypeError: Cannot read property 'replace' of undefined."```
	* Fix:
		* We can move the creation of templates to the initialize function, after an itemView is created, which means that the template won't be called until after the DOM is rendered. 
		* In order to execute certain pieces of JavaScript only after the DOM is loaded is to utilize ```document.ready``` and declare everything that must be called *after* the DOM is loaded in there
		
		```
		$(document).ready(function(){

  			var input = new InputView();
  			var list = new listView();

		});
		```
		* **Note:** You shouldn't have more than one document.ready files because only the one rendered last wins
* Access list that was created in document.ready
	* Give key-value pairs to views you're creating. You can now call on these elsewhere. 

	```
	var list = new listView();
  	var input = new InputView();
  	
  	input.list = list
	```
	* We gave the inputView an attribute that refers to the listView we created
	* We can now call on this list within the inputView by calling on ```this.list```
		* **Note**: We have to give input the attribute of list on a separate line instead of assigning it within the creation of ```new InputView``` because when creating a new View only certain attributes can be passed in.
		
### Models

* Models exist in ```app/assets/javascripts/models```
* Move models array into models file
	* Program still works
* Create model file via Backbone:

	```
	var todoItem = Backbone.Model.extend({

	});
	```
	* Create a new task:
	
		```
		var task = new TodoItem();
		```
	
	* Set content of new task
		
		```
		task.set('content', 'Buy Milk');
		```
	* Get value of attribute of task
	
		```
		task.get('content');
		=> Buy Milk
		```
	* **NOTE** You can view task object directly and even set attributes directly, but you shouldn't because a lot of the other backbone function rely on your using the get and set functions:
	
		```
		task.attributes
		=> Object {content: "Buy Milk", done: false}
		
		task.attribute.done = true
		=> true
		```
		
* Backbone models can do a lot more than just get and set attributes, we can also save it to the database
	
	
**Connect Model to Database**
* Give model urlRoot
	
	```
	var TodoItem = Backbone.Model.extend({
		urlRoot: '/todos'
	});
	```
* You can save an instance of a TodoItem now by calling task.save();
	* This happens via an AJAX request by using the path of urlRoot to construct the route
* Our server still needs to know what to do with this
	* POST /todos should go to create action
	* Create action takes params passed to it and creates a new todo
	* Create action can respond to JSON with some information upon successful save
* Get updated information from database

	```
	task.fetch();
	```
	
	* This AJAX request goes to ```'/todos/:id'```
* Set variable to object in databse
	* Create new variable as TodoItem with id that exists, then fetch from db
	
		```
		var task = new TodoItem({id: 1});
		task.fetch();
		task.get('content');
		=> "Don't buy warm milk"
		```
* Backbone models can be converted to JSON, which returns a simple object:

	```
	models[0].toJSON()
	=> Object {content: "Book a hotel", done: false}
	```

**Connecting Models to Views**

* Render models into Backbone model objects
* when setting variables we need to call on model object attributes:

	```
	var todoContent = this.model.get('content');
	var completed = this.model.get('done');
	```

* When creating variable for compiled view, we need to return a hash 

	```
	var compiledView = this.template(this.model.toJSON);
	```
	
**Fetching Data from Database for Views**

* Iterate over each model in document.ready and fetch each one:

	```
	_.each(models, function(model){
    	model.fetch();
  	});
	```
	* This is an AJAX request and we need to wait for it to finish before rendering them or we'll get an error
	* We can solve this by using **success**
		* Pass .fetch call an object, which can have a property, success
		
			```
			  _.each(models, function(model){
    			model.fetch({
      			success: function() {
        			console.log('Fetched!');
      				}
    			});
  			});
			```
	* We can tell each model to append to the list after it is fetched
		* The model doesn't have a view property, but the view has a model property
			* If we can talk to the view, we can be sure the model knows how to do things
			* When each item is initialized it was being told to render itself. That's a big no-no because first we have to fetch. So we tell it to fetch, and upon success THEN render itself
			
			```
			initialize: function(){
    			console.log('new item view');
    			this.template = _.template($('#todo-template').html());
    			this.model.fetch({
      				success: function() {
        				this.render();
      				}.bind(this)
    			});
  			},
			```
	* This only works because without bind "this" means the window object. We need to bind it to the particular item view we're trying to create
	* This is making 7 AJAX requests to the database to get information
	
	
### Collections

* Create a collection that extend from Backbone collection and specifies what model it should be using

	```
	var TodoCollection = Backbone.Collection.extend({
		model: TodoItem
	});
	```
* Create a new instance of the collection of to do items

	```
	var collection = new TodoCollection();
	```
* Add the array of models into this collection

	```
	collection.add(models);
	```
* We can have our collection fetch data from the database by adding a url to the collection and calling fetch on the collection

	```
	var TodoCollection = Backbone.Collection.extend({
  		url: '/todos',
  		model: TodoItem
	});

	var collection = new TodoCollection();
	collection.fetch();
	```
	* This makes an AJAX request to the todos index route, takes each object of To Do and store it in a model of class TodoItem. 

* Steps
	1. **Instantiate Collection**
	2. **Populate collection (get models from server)**
		* Fetch (Populate) the Collection on document.ready
	
			```
			collection.fetch({
    			success: function() {
      			var list = new listView({collection: collection});
      			var input = new InputView();
      			input.list = list;
    			}
  			});
			```
			* We need to use success so that the listView and inputView is not created until after the collections are fetched
	3. **Create listView that is related to the collection**
		* Tell listView that its collection should be collection
		
			```
			var list = new listView({collection: collection});
			```
			* We do this because of step 4
	4. **Create itemViews and have them know who their model is**
		* In backbone views keep track of models
		
			```
			addSingle: function(todoModel){
    			var todoView = new itemView({ model: todoModel });
    			this.$el.append(todoView.el);
  			}
			```
	5. **Whatever you've generated, put on to page (append)**

### Destroy Model Objects

* We want to be able to get rid of the model on the front-end
	* Remove the itemView from inside the listView, then let the server know that it's gone and that the server should delete it
* When we click the x on ToDo page the item should be deleted
	* Model should go away first, then the view should disappear as a consequence 
	* The 'x' belongs to the itemView. If we get rid of the itemView before we remove it from the collection we won't be able to get to the collection anymore
* After we click the x:
	1. Hits the views event
	2. Views event tells its model to destroy itself
		
		```
		delete: function(){
    		this.model.destroy();
  		},
		```
	3. Then we need to get rid of the view itself. 
		* We can use ```this.$el.remove();``` to remove it from the view, but this isn't the best way. If we had a button that iterated over the collection and destroyed each model, we have to repeat this over there
			* it becomes hard to keep views in sync with database. We have to keep track of every place we remove model, we also have to remove on view
		* We should add an event listener on the item that listens to *any* change on its model. If there is a change on the model it does something
		
			```
			this.listenTo(this.model, 'destroy', this.removeEl);
			```

### Add Model Objects	

* Add a new model to the view, which then tells the server to make a new one
* InputView has two things in it: input text box and a button
	* There is an event listener on click of the button that runs the function newTodo
		* Currently newTodo is creating a new model object that is not part of the collection and runs the addSingle method on ```this.list```, which is our listView. 
		* A new view is created and that view is appended on the screen
	* There are a few things wrong with this:
		* When we click the add button we want whatever task we add to be automatically persisted to our database
		* The new model we're creating is floating around outside of our collections. It should be *a part of our* collection. And it should go through the same process the other models in the collection go through in order to get its own view.

**Adding new model to collection**

* Edit newTodo function so that we can edit the collection
	* In order to do this the inputView needs to know about the collection. Add a property to input in application.js
	
		```
		input.collection = collection;
		```
	* Better yet we can do it when we create the input:
	
		```
	    var input = new InputView({collection: collection});
		```
* In InputView create the model inside collection:

	```
	this.collection.create(newTodo);
	```
	* Add and Remove edit the View without touching database. Create and Destroy edit the collection *as well as* the database
	
**In document.ready model is telling view to do something, fix it**

* Right now the model, collection, is telling the views to do something. That is bad practice. The Views should instead be listening to the models and then do something.
	* When we addToDos, first empty out the existing ToDos so we don't get duplicates:
	
		```
		addTodos: function(){
    		this.$el.empty();
    		this.collection.each(this.addSingle, this);
    	}
		```

**ListenTo Reset Instead of Add**

* Right now when we fetch for the first time the same thing is happening as when we create
	* Have fetch reset itself so it's only firing off one [reset event](http://backbonejs.org/#Collection-reset):
	
		```
		collection.fetch({reset: true});
		```
		* The way reset works is that if the first time it fetches it gets 4 objects it add its to the view. The next time it fetches it see there are two more things. It knows that the first 4 are already on the page so it doesn't try to re-add them. 
		* Reset erases what's on the page and re-adds everything
			* It's a way to tell your program that you're not doing a conventional add, but pulling from the server
			* Telling any listeners on 'add'

### Modify Model Objects

* We need to track modifications
	* On a click update database
	* ItemView listens for any changes in the database and when it hears a change event it updates its view
* On click toggle status of done attribute:

	```
  	completed: function(){
    	var checked = this.$('input:checkbox').is(':checked');
    	if (checked) {
      		this.model.set('done', true);
    		} else {
      			this.model.set('done', false);
    		}
    	this.$el.save();
  	},
	```
	* First check status of checkbox, and do the opposite. 
* Now we need the view to update itself when the database is changed
	
	```
	this.listenTo(this.model, 'change', this.render);
	```
	
### Why Do We Use Backbone?

* No refresh
* Reduce load on server
	* Structured in such a way that I can make a bunch of changes and all those things just happen to the model, but we don't constantly bother the server. And we just have a save button, that every time we click it everything in our view gets bundled into one request and gets sent to server