# Day 51 Notes | Week 11 Day 1

April 5, 2014

### Learning Objectives

* Explain what Backbone it
* Explain why we use backbone
* Difference between a framework and library
* Extend backbone's view object and define custom attributes
* Utilize events to update a view
* Utilize templates to render info

---

### Backbone

**What is Backbone?**

* A framework, like Rails, is something that provides a rigid structure and support for building web applications. It requires that you adhere to a set of implementation details. 
	* Backbone isn't a framework: it doesn't have a structure for building an app, but it does give you structure for building MVCs
	
* **Libraries**
	* Backbone is a library - makes things easier. 
		* Libraries include things we do all the time and allows us to utilize them without writing them each time.
	* Gives us code to interact with our code easier and faster
		* Tells us exactly what to do
			* If we don't follow those configurations, things get messy
	* Backbone is a way of organizing your code
		* You don't *need* backbone, you can write everything yourself, but it gets tedious. 
* jQuery
	* Also a library for JavaScript
	* Makes syntax easier than regular JavaScript
	* jQuery provides an API for DOM manipulation
		* API doesn't have to correlate to a server that gives and receives request

**What do we use Backbone for?**

* We use it for building single-page apps that rely heavily on the client and browser rather than on the server for providing functions 
	* Manipulating data
	* Doesn't require constantly going back to the server
	
**How does it function?**

* It gets all the views, functions and other info it needs and holds it within the browser. It then listens for events and then presents what is needed. 

### Example of a Single-Page App

* Gmail
	* Click on something, the page doesn't send a request to the server for new info, it just provides it
* Benefits
	* You have your whole website on one page
	* Really fast because everything is there from the beginning. You change it as you need it. 
	You can unload your processing to the client-side browser
* If you need to change something on the server
	* it just sends a tiny little packet to the server
	
### MVCs

* Rails
	* Rails has rMVCs
		* Routes 
		* Models - connected to **database**, which persists our data
		* Views - connected to partials 
		* Controller
* Backbone
	* Mimics Rails
		* **Router**
			* handles all requests coming into backbone app, and then routes them to a specific action
			* if we go to users, that will be our index route and will say "go to the users controller, index action and perform whatever is defined in a method there"
		* **Model**
			* Represent our data
			* Have all of the attributes of what is being stored in the database
			* Our browser doesn't have a database, but we still need something like that to make use of things in our models. So Backbone has **collections**, which acts like a pseudo-database in our browser
				* When we start our app backbone goes to our database and pull all collections needed
		* **Views**
			* Render model info
			* Maintain controller logic of what to do, piecing everything together and giving it back to user. 
				* There are no controllers in Backbone, so our views perform double-duty. 
		* **Template**
			* Partials allow us to display a view within another view
			* In backbone this is known as templates
			* Pieces of reusable HTML code that we can put model info inside, and then render within a view
			
### Syntax

**Make a new view**

* A view is an object that represents a DOM element
* When we make a new view, we're just [extending](http://backbonejs.org/#View-extend) the backbone object

	```
	Backbone.View.extend(properties, [classProperties])
	```
* You can pass in options: 
	* [constructor/initialize](http://backbonejs.org/#View-constructor)
		* initialize it with new and then pass in options
		
		```
		new View([options])
		```
	* If you don't assign tagName, it will automatically default to an empty ```div```
* Pseudo-Classes
	* Have classes and options
	* One class is **el**. You can access this like any other property: ```view.el```
		* Better way to access is via namespaced object: ```view.$el```
* Attributes
	* Utilize this to make changes on element
* jQuery
	* ```view.$(selector)``` works just like jQuery, but searches within this view. 
* template
	* There is a property attached to every view object. It allows us to utilize variables to actually embed JavaScript variables within any type of text
* render
	* utilizes template
	* take model, takes all attributes, puts it inside template, and makes it html of template
	* view is container with el attached to it, and it is stuck inside template and then shown to user
	* render is how we gather all elements and show it to the user
	
**Events**

* This is how Backbone knows what to do
* The way we establish events are with keys and functions
	* The key is a function and the object is a key-value pair
	* Pass it a string representing the actually event, on click, then give it another string representing another function we've assigned
	
		```
		events: {
    	"dblclick"                : "open",
    	"click .icon.doc"         : "select"
		```
	* The functions, "open", are defined within the view
	
		```
		open: function() {
    		window.open(this.model.get("viewer_url"));
  		},
		```

### Code Along

* Create a new View constructor

	```
	var View = Backbone.View.extend({});
	var view = new View()
	
	view = child {cid: "view1", $el: jQuery.fn.jQuery.init[1], el: div, constructor: function, on: function…}
	```
* Set class Name

	```
	view.className = "boy"
	```
* What is the element of view?

	```
	view.$el
	
	=> [<div>​</div>​]
	```
	* If we don't use the dollar sign, it'll give us a vanilla JavaScript object, which is most likely not what we want

### Build a Rails App

* Create a scaffold for a Rails App utilizing a model for a to do, with content and a boolean that says whether it's completed or not
* Add ```underscore.js``` and ```backbone.js``` to ```vendor/assets/javascript```
* Require these file in the application.js file

	```
	//= underscore
	//= backbone
	```
	* must require backbone after underscore and underscore after jQuery.
* Create a views folder under ```app/assets/javascripts``` and inside that folder add ```inputView.js```
* Define constructor inside InputView* 
	* Add object inside parenthesis
		* each view has an ```el``` attached to it (as defined in index.html.erb)
		
			```
			var InputView = Backbone.View.extend({
  				el: 'div#new-todo',
  
			});
			```
	* Add event bindings
	
		```
		var InputView = Backbone.View.extend({
  			el: 'div#new-todo',
  			events: {
    			'click button#add': 'newTodo'
  			}
		});
		```
		* we define on *click* of the button with id ```#add```, we run the function 'new Todo'
* Create new file ```listView.js```
	* Take the to do item and add it to the list view

**Utilizing Templates**

* Define after class name
	* Depends on underscore
	* Use Ruby rockets to embed JS variables
		
		```
		template: _.template('<p><%= content %> <input type="checkbox" <%= done ? "checked" : "" %> /> <span> x </span> <p>');
		```
	* Call template file
	
		```
		var compiledView = this.template(this.model);
		if(completed){
    		this.$el.toggleClass('done');
  		}
		```

* We can put the template in the html.erb file (within a script)
	* If we do that we need two percent file to let the html.erb file know it's jabascript
	
		```
		<script type="text/template" id="todo-template">
		<p><%%= content %>
		<input type="checkbox" <%%= done ? "checked" : "" %> />
		<span> x </span>
		</p>
		</script>
		```
	* Pull this template into js file by:
	
		```
		template: _.template($('#todo-template'))

		```
		* But we're interested in the html inside of it. The way we get html from inside jQuery is ```.html```
	* This is the **best practice** way of doing it. Pull out all html from js file and put it in html.erb page
	* If you have lots and lots of templates you can then move it out to another file

**Add Ability to Delete a Todo Item**

**Add Ability to Add new Todos**

* In InputView under the newTodo function:

	```
	newTodo: function(){
    	console.log("hi i'm clicked");
    	var content = this.$('#content').val();
    	var completed = false;
    	var thisTodoModel = {content: content, done: completed};

    	list.addSingle(thisTodoModel);
    };
	```
	* We created the addSingle function in the global namespace, so we have access to it anywhere. 