# Day 31 Notes | Week 7 Day 1

April 7, 2014

### Learning Objectives

* Construct a Rails app that responds to requests with JSON
* Construct a rails application that responds to requests differently based on the requested format
* Understand when you would want to move away from that to another way of handling different requests

---
### Creating Our Own APIs

**Tool to Help with Testing**

* [Simplecov Gem](https://github.com/colszowka/simplecov)
	* Reads through the specs and code you've written and determines the coverage of the code you've written
	* Gives you a percentage of how much of your code is covered by your tests
	
**Create Our Own API**

* New Rails App - notes_app
	* notes table
		* id
		* author (string)
		* content (string)
	* controller
		* index
		* show
	* seed db with 5 notes
	
* Convert data structure in Ruby to a string that is syntactically JSON

		h = {a: "first", b: "second"}
		
		h.to_json
		"{\"a\":"first\",\b"b\":":\"second\"}"
		
		puts h.to_json
		{"a":"first","b":"second"}
		
	* To do this within rails, just tell a route to render something ```.to_json```
		* we don't need ```.to_json``` in views, but it's helpful to always explicitly specify to avoid confusion or making assumptions that could be invalid
		
* Duplicate Routes
	* ```/api/notes``` does the same thing as ```/index```. 
	* Instead we can just specify what format we want ```/index``` returned as
		* That's what ```(.format)``` in rake routes means
	* in the old internet home.html represented a file, not it represented a resource - or a format of that resouce
* Exploit that for APIs in controller

		def index
			@notes = Note.all
			
			respond_to do |format|
				format.html do 
				end
			
				format.json to
					render json: @notes.to_json
				end
			end
		
		
		end
		
	* DRY it up
		* curly brackets instead of do end
		
			    respond_to do |format|
      				format.html do
     			 	format.json { render json: @notes.to_json}
				end
				
* Using respond_to works great when you have a simple applicaiton
	* when it's more complex, think about creating separate json controllers
	
---

### Rails Asset Pipeline

* Why
	* What are assets?
		* CSS
		* JavaScript
		* Images
	* What's alternative?
		* The public folder
			* directly accessible
			* Used to be the way we had to manage our assets in our Rails app
				* Had to do it manually
			* Downsides
				* Many requests
				* Large Files
	* Organizing Assets
	* Reduces number of requests
		* When a browser loads a webpage 
			1. First request, then browser gets HTML as response
			2. Second request to assets - if HTML specifies a link to a stylesheet, JavaScript, etc., the browser will make additional requests for each one. 
				* Can make requests to 20+ CSS files
				* Asset Pipelines concatenates all CSS into one big file (same with JavaScript)
				* *only for CSS & JavaScript*
	* Compression/Minification
		* All assets must be downloaded. CSS can be big for a specific site. 
		* The asset pipeline compresses this down to make it easier to download
		* **minification** - removes whitespace. harder for humans to read.
		* **compression** - gzip, most browsers can request that the server give it a zipped form of the CSS and JavaScript so it can be sent over the internet quickly and then the browser unzips it
		* *only for CSS & JavaScript*
	* Compiling (i.e. convert)
		* Alternative languages are converted into formats that the browser can understand
			* CSS -> SASS
				* More advanced CSS that allows variables, etc.
			* JavaScript -> CoffeeScript
				* Language that gets converted into JavaScript
				* Easier for humans to write
	* Cacheing
		* Assets don't change that often, even though the content does
		* Browser will download the file the first time it needs it. If it can be confident that the file hasn't been changed, 
		* Danger of cacheing 
			* Invalidating Cache (Busting the cache) - How do we inform the browser that the file has changed
			* [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) - for cacheing 
		* **Fingerprinting**
			* If we have one css file and one javascript file for our app, we'd probably call them application.css and application.js
			* The server has to set some kind of cache value to tell the server how long to hold on to the file, but the server doesn't know when we're going to change it next
			* Keeps track of files with hash that represents contents of file. If server version differs from cached version, the browser knows the asset is different
			* Allows us to cache assets indefinitely - because browser will update once filename changes.
				* Cache for a year, which is an enternity in internet life
	* Helper / Referencing Assets 
* After the Asset Pipeline compiles the assets they're put into ```/public``

	
**Asset Pipeline in Practice**

* Image
	* Save image in ```app/assets/images``` and call it via the image tag: ```<%= image_tag 'logo.png' %>```
		* If the image is in a sub-folder of an asset folder you have to specify it by name
* Sprockets
	* Reads the comments of our CSS file and knows what to embed in one CSS file

			*= require_self
			*= require forms
			*= require_tree .
			
	* ```require_tree``` means take a given folder and then load everything in that folder
		* doesn't specify order that files are loaded
* Launch app in production environment:

		rails s -e production
	* Create DB in productio mode:
	
			rake db:create RAILS_ENV=production
	* Compile assets in production mode
		* Rails goes through assets folder, compiles everything, then puts them into public mode
		
* In production mode, Rails doesn't server static assets. It expects the server to do so.
		
* High Performance Webrick
	* unicorn
	* [puma](http://puma.io/)
	* passenger
* call asset image in css
	* make css file an erb
	* use asset_path helper
	
			background-image: url(<%= asset_path('background.png') %>);
* wipe out precompiled assets

		rake assets:clobber
* Main Asset Rake Tasks

		rake assets:precompile RAILS_ENV=productions
		
		rake assets:clean RAILS_ENV=production
		
		rake assets:clobber RAILS_ENV=production
		
	* clean removes everything but latest version
	
	* Run server in Production Mode:
	
			rails server -e production
	* Production DB
	
			rake db:create RAILS_ENV=production
			
			rake db:migrate RAILS_ENV=production