# Day 57 Notes | Week 12 Day 2

May 13, 2014

---

### Learning Objectives

* Make stuff go fast
* Dollas in the Wind
* Mo' queries, mo' problems

### Slow Index Page

* The slower a page takes to load, the slower your server is
	* It can't concurrently help other users
* Negative Impacts of Slow Loading Page
	* User waits forever and will leave
	* Limits how many users your app can server at a time, which in turn limits scalability of app
* Gems
	* Quiet Assets
		* Without this gem in development whenever the browser requests files from db it'll show up in the log.
		* This gem prevents that from happening so you can focus on important stuff
	* Rack Mini Profilier - ```gem 'rack-mini-profiler```
		* Shows up in development, not production
		* Tells you in ms how much time was spent everywhere
* What's slowing down the app?
	* **the n+1 query problem**
		* What is it?
			* If you first grab all the posts on your page you're making a query to grab all posts
			* Then you do a query per post to grab all comments on each post. 
			* This is a common problem that slows things down. 
		* How to fix
			* Tell Rails (ActiveRecord) that we'll be referencing the comment that belong to each of these posts. 
			* This is called eager loading
			
				```
				class PostController < ApplicationController
					
					def	index
						@posts = Post.all.includes(:comment)
					end
					
				end
				```
			* This will find all the posts, then issue one query to get all the comments for all those posts
				* The db has a special way to grab all the comments for this post
				* Here db only makes 2 queries
		* Gem bullet - ```gem 'bullet'```
			* Watches for n+1 queries (and others) and then will warn you
			* Install gem
			* Config/environments/development and include bullet configs
			
				```
				#Bullet Configs
  				config.after_initialize do
    				Bullet.enable = true
    				Bullet.rails_logger = true
    				Bullet.add_footer = true
  				end
				```
	* Caching Partials (aka dollas in the wind)
		* The Problem
			* The problem is that we're taking the post partial and then getting a string of text (html) from it. 
			* This partial is like a method - takes input like a comment, and outputs a string of text
				* Given a specific comment, if we rendered this partial 1000 times, the comment should always look the same
			* This is what defines a pure function (vs a method or procedure). A function means if we give it the same input, we'll *always* get the same output. 
		* The Fix
			* We save the output of the function so the program doesn't have to keep running the function
				* Called **memoizing** when done with pure function
				* In Rails-land (and with views) it's called **cacheing**
			* Tell partial when to cache and when to stop. Also tell it what to use as the lookup. 
				
				```
				<% cache comment.id do %>
				
					# comment partial goes here
				
				<% end %>
				```
			* Also enable cacheing in Rails development config file (```config/environment/development.rb```)
			
				```
				config.action_controller.perform_caching = true
				```
				
				* Whenever the program finds a comment with an id in the cache, it pulls from the cache
			* **warming up the cache** is when you run the function for the first time and it has to query to build the cache
		* Where do we save cache
			* In production we save in RAM, but that means we need more RAM
			* In development we save on disk
				* Edit config/environment/development.rb
			
					```
					config.cache_store = :memory_store
					```
		* You can also cache posts instead of just in comments
	* Dealing with expired (invalid) cache
		* instead of cacheing by comment id only, use a mix of comment id **AND** when the comment was last updated
			* Update cache erg line
		
				```
				<% cache "#{post.id}_#{post.updated_at}" do %>
				
					# comment partial goes here
				
				<% end %>
				```
	* Even more detailed cache name
	
		```
		<% cache "comment_#{comment.id}_#{comment.updated_at}" do %>
		```
 	* Now make it easy the Rails way
 	
 		```
 		<% cache comment do %>
 		
 			# comment partial goes here
 		
 		<% end %>
 		```
 		
 		* If cache notices you pass in an active record object it will generate a "cache key", which is what we wrote before
 	* Nested cache
 	
 		```
 		<% cache [post, post.comment] do %>
 		
 			# post partial goes here
 		
 		<% end %>
 		```