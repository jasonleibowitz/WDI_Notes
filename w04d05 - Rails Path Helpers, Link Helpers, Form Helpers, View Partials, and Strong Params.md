# Day 20 Notes | Week 4 Day 5

March 21, 2014

---

### Learning Objectives

* Understand & use the following to DRY our Rails code:
	* path/url helpers
	* link helpers
	* form helpers
	* view partials
* Understand & explain the difference between
	* rails view template & partial
* Use strong params to protect our app from mass assignment

---

### Abstractions

* You need to know underlying code
	* At some point they always fail
	* They are helpful though
* Make code easier to read and write

### Rails Helpers


** Path Helper **

* You can use these in *controllers* or *views*
	* They only work if you create the RESTful routes in the routes.rb file rather than hand code them. 
* create RESTful routes
	* In route file write: ```resources :superheroes```
* redirect with path
	* ```redirect_to(superheroes_path)```
* redirect to the show path of the superhero we created:

		def create
    		@superhero = Superhero.new
    		@superhero.name = params[:name]
    		@superhero.age = params[:age]
    		@superhero.power = params[:power]
    		@superhero.weakness = params[:weakness]
    		@superhero.save

   			redirect_to(superheros_path(@superhero))
 		end
  				

	* or even better: ```redirect_to(@superhero)```
	* for edit path:
	
			redirect_to(edit_superhero_path(@superhero))
* We can do this for the follow four paths:
	* GET index - @superheros
	* GET new - @new_superhero
	* GET edit - @edit_superhero
	* GET show - @superhero
	
* Path Helper for links
	* ```superhero_path(superhero)```
	
** Root Helper **

* In root file you can specify a root path by:

		root to: 'superheroes#index'
		
** Link To **

* Ruby method that Rails give us

		<%= link_to superhero.name, superhero %>
		
	* link_to is a method, just like the path helpers, that take 2 arguments
		* the first name is what is being displayed
			* you can also use a do block to specify what is displayed
		* the second is the link(path) - we can pass an object as the path
		
** Delete Link To Helper **

* we can only send a delete method via a form, but link to helper lets us get around that:

		<%= link_to "Delete", superhero_path(@superhero), method: 'delete" %>

** Form Helper **

* form_tag
* form_for(@object)
	* needs an instance of the class of the thing you want to make a form for
	* create an instance of a new superhero object in the controller's new method
	
			@superhero = Superhero.new
			
		* this only lives here so that the form_for knows where the form goes
			
	* opening and closing form tags
	
			<% form_for (@superhero) do |f| %>
			
			<% end %>
			
	* generate labels and inputs
	
			<%= f.label :name %>
			<%= f.text_field :name %>
			
		* creates a label for name and a text field for name
			* Note: automatically capitalizes label name
* Strong Params
	* form_for nests everything into a hash for us
	* permit
		* to let rails know what params are permitted to be updated we have to change params like:
		
				params[:superhero].permit(:name, :age)
		* write a method to retun the permit value
		
				private
				def superhero_params
    				return params[:superhero].permit(:name, :age, :power, :weakness)
 				end
 				
 		* call it on ```Superhero.create```
 		
 				Superhero.create(superhero_params)
 		* permit all
 			
 				.permit!
 			* bang here means "dangerous"
 			* if later on our superhero gets an attribute that needs to be protected, we could forget to change bang. 
 				* don't use it
 	* require
 		* we can use ```params.require(:superhero).permit(:name, :age, :power, :weakness)```
 		* this first requires that there's a param called :superhero and then permits what we white list
 		* if superhero has isn't there, then originl form would return nil
 			* we could call nil.permit exception
 			* or could throw a more helpful error
 				
 * edit form
 	* if object form points to already has data, it'll automatically point to the edit path
 	* it will automatically also populate fields with value in database
 				
 * private method in Rails
 	* in ruby they can only be called in Class
 	* in rails we can't call an action on it
 		* only for controller to use
 		
 ** View Partial **
 
* View Templates
	 * Views we're working with are called View Templates
	 * That means they are standalone
	 	* They can stand on their own
* View Partials
	* Reuse parts of a view in another view
	* Main difference, filename starts with underscore
	* Naming
		* name of view partial must start with underscore: ```_form.html.erb```
	* Calling it
		* call it in form view by:
		
				<%= render "form" %>
				
	* Submit button naming
		* in the view partial don't name the button and form_for is smart enough to know what to call it
			* Create <%= object.name %>
			* or Edit <%= object.name %>
* **Pass variable**	 into View Partial

		<%= render partial: "superhero", locals: {superhero: current_superman} %>
		
* Pass multiple variables into View Partials

		<%= render partial: "superhero", locals: {superhero: current_superman, show_title_as_link: true } %>
		
* Render
	* In controllers you only render templates (normal views)
	* Inside of views you can only call render on partials
	 	* You can't put a template on a template
	 	
* Refactoring in Rails
	* rendering view partials with one variable
		* conditions:
			* have an activerecord model object
			* you have a partial whose name is the same as the model 
			* if you *only have one* variable in the partial that represents the model object itself and the variable is the same as the model name and class name
		* render current superhero: 
		
				<% @superheroes do |current_superhero| %>
					<%= render current_superhero %>
				<% end %>
	* iterate over array in the same way:
	
			<%= @superheroes %>


			
### Teach Rails Irregular Pluralizations

* Edit the file ```config/initializers/inflections.rb```
	* add the following
	
			ActiveSupport::Inflector.inflections(:en) do |inflect|
  				inflect.irregular 'superhero', 'superheroes'
			end