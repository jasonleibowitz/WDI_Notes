# Day 30 Notes | Week 6 Day 5

---

### Learning Objectives

* DRY up views using partials and locals
* Move login from views to controllers
* Skinny controllers, fat models
* Use class variables and constants to avoid hard-coding parameters
* Construct models & class methods to handle API calls

---

### Rotten Tomato API App Review

* Rails doesn't care about GET forms
	* So don't use rails forms to create get forms.
* Specify the folder for partial

		<%= render 'rt_api/search_form' %>
		
* Multiple layout pages
	* Make a new layout view named after the controller. 
	* because this layout view has the same name as the controller, Rails will look for it first when generating a view
		* **convention over configuration**
		* you can split up different parts of your layout file into partials
* Skinny controller, fat models
	* Controller
		* like person at the gate
		* calls on the models and views
	* Views
		* A views job is to be able to take some sort of data and generate a visual representation of that data
		* Views should never query the database, that's the controller's job (which it does by talking to the model)
* URI class in Ruby
	* Nice library that does URI massages for us
		* if we have special characters that aren't allowed in standard URL, it would replace those characters with standardized characters
		
				URI.escape(query)
				
* Initializers
	* All the things that are run when you startup your Rails server
* Class Variable vs Constant
	* Constants can't be changed, whereas class variables can be
	* Both are values we can apply to the entire class
* Avoid sticking in hard coded values into your program
	* You never know if you want to use that same parameter in multiple places. If it's in multiple places and you want to change it, you're going to have a bad time.
* **Encapsulating** gem code
	* Any time you rely on someone else's code, encapsulate it so if you have to change it, you only have to change it in one place