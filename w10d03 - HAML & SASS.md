# Day 48 Notes | Week 10 Day 3

April 30, 2014

---

### Learning Objectives

* Explain the benefits and downsides of HAML
* Explain relationship between HTML & HAML syntax
* Use HAML in a Rails App

### [HAML](http://haml.info/)

**Intro**

* Meaning
	* HTML abstraction markup language
* Pros
	* More succinct
	* Type less
	* More readable
	* Designed with Ruby in mind
* Cons
	* Not as widely used standard
		* If you're working in a team everyone needs to commit to using it
	* Have to compile from HAML to HTML
	
**How To Use**

* Tag
	* ```%``` sign to make tags
	* class with ```.```
	* id with ```#```
	* indentation matters because it auto closes tag when you use different indentation
	* div is implied element
		* ```.content``` will create div with class content
	* To start embedded Ruby just use ```=``` sign, don't need clown hats
	
		```
		%section.container
  			%h1= post.title
  			%h2= post.subtitle
  			.content
    			= post.content
		```
* HAML Gem
* Filename
	* Named ```file.html.haml``` instead of erg
	* Rails goes right to left when compiling filenames
* Embedded Ruby
	* ```=``` sign is when you want to render Ruby
	* ```-``` is when you don't want to render it
	* Don't need end blocks
		* If you **indent** under a Ruby tag HAML knows to automatically add an end tag
* **Embedding Ruby into tags**

	```
	.hoot{id: dom_id(hoot), data: {hoot_info: hoot.to_json, url: hoot_path(hoot) }}
	```
	
	* div with class hoot
	* If you put curly braces after class or id names that means you're going to give a list of attributes for this tag
		* The curly braces automatically let us run Ruby - it is a Ruby hash
	* The only problem is that we lose the auto-conversion of data into JSON
		* If we want something to be converted to JSON we have to call ```hoot.to_jason```
* **Dom_ID** Pattern
	* So common and useful that HAML has class reference built in
	* If we want to call DOM ID on something, do this:
	
		```
		%div[hoot]{data: {hoot_info: hoot.to_json, url: hoot_path(hoot)} }
		```
		
		* hoot is a variable that always hold an ActiveRecord object
		* will give class of hoot and the id will be the dom_id of the hoot
	* For things like data attributes, we still have to put them in manually
	
### HAML-Rails

* Haml gem gives you everything you need to run your app
* But this gem allows you to build a scaffold in HAML
	* ```rails g scaffold Comment title content```
		* this will build a comment view with all pages in haml
		
### SASS

* Two variants
	* .sass - it's more HAML style in that you don't need the braces. Indentations determines when you open and close curly brackets
	* .scss - easier to read
* Nesting
	* You can't nest in CSS, but you can in SASS
	* Instead of writing css under .hoot and then something else under .hoot a, you can just do:
	
	```
	.hoot {
	css: value;
	}
		a {
		css: value;
		}
	```
	
	* Nest even further
		* a:hover can be nested under a with a **parent reference**
		
			```
  a {
    font-size: 10px;
    color: red;
    text-decoration: none;
    &:hover {
    color: inidianred;
    text-decoration: underline;
    }
  }
			```
* Namespace Nesting
	* Nest font-size under font-family
	
		```
	font: {
    family: sans-serif;
    size: 14px;
  }
		```
		
* Mixins
	* Common properties shared across different elements
	* Once you define a mixin you can include it in any element
	* Mixins have to be defined before you include them
	
		```
		@mixin border-radius {
  			webkit-border-radius: 10px;
  			moz-border-radius: 10px;
  			border-radius: 10px;
  			border: 2px solid red;
		}
		```
		
	* call mixin
	
	```
	@include border-radius
	```
* Variables in SASS
	* Variables start with a ```$``` and we set them with a ```:```
	* Anywhere below the declaration of a variable we can use it
	
* Mixins can accept variables

	```
	@mixin border-radius($radius, $thickness, $color) {
  		webkit-border-radius: $radius;
  		moz-border-radius: $radius;
  		border-radius: $radius;
  		border: $thickness solid $color;
	}
	```
	
	* Pass in variables when you call mixing
	
		```
		@include border-radius(5px, 1px, blue)
		```
* Set default values for mixin variables

	```
	@mixin border-radius($radius: 10px, $thickness: 2px, $color: $primary-color) {
	```
* Nesting - How Deep
	* Don't nest more than 3-4 deep
		* Helps keep code semantically simple
		* For performance reasons
			* Your browser will eventually have to interpret these selectors. If you nest too deeply there can be performance concerns
			
* Commenting in SASS
	* You comment with two ```//``` symbols
	
	```
	// Comment in SCSS like this
	```
* SASS allows your to write functions, which you can call and return values
	* Maybe you want to dynamically determine the width of the sidebar based on the width of the window
		* Write a function that does the math and returns that value
		
		```
		.sidebar {
  			width: fluidize($width);
		}
		```
	* Color Functions
		* Secondary color is always complement of primary
* Import Directive
	* CSS has @import directive
		* this loads another CSS file and will load it's properties at its function
		* The problem with this is that it's something the browser sees
			* If the browser sees 50 requests, it will make as many requests as you have import directives. 
	* SCSS has import too
		* But it does it differently
		* It loads them in when the asset pipeline is created