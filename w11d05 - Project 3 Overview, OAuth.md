# Day 55 Notes | Week 11 Day 5

May 9, 2014

---

### Project 3 Review

* User Stories, ERD Diagrams
* Tests
	* At least acceptance tests
		* Capybara
		* Test user stories
			* "When I go to /movies and click search in this box I should see Robocop movies show up"
	* Unit Tests
	* Will be checked by Monday/Tuesday
* Primary Focus
	* Code Quality
		* DRY
		* Consistent/Formatting
		* Well-documented
			* Comments where appropriate
			* Really good ReadMe
		* Encapsulation
		* Semantic (good names)
		* Tested
		* Small
* Goal
	* Create a portfolio piece to demonstrate your abilities to the community and potential employers
	
---

### Lessons for Next Week

* Omniauth
* Rails Optimization
* Alg/BigO/Recursion
* Server Deployment (other than Heroku)
* Refactoring Modules

### OAuth

---

**Learning Objectives**

* Understand OAuth
* Use omniauth & devise for 3rd party auth in a rails app

**What is OAuth?**

* Standard for 3rd party authentication
	* Let people log in to your site via Facebook or google and get information about them after they log in
	* Logging in means multiple things
		* Verify identity so they can re-login
		* Can't ask for user's Facebook username and password and then verify - that's bad practice 
* Three Players 
	* Provider
		* The authentication provider, Facebook for example
		* They have a list of users with usernames and passwords. We want to talk to Facebook and know that someone visiting our site is so-and-so on Facebook
	* Consumer
		* Our app
	* End User
		* The user trying to log into our site
* How It Works
	* User goes to our site
	* Click a link to login via facebook
		* Facebook wants to protect its user's data. It won't let any app get user data. Before any of this happens we'll have to get an API User Key. 
			* This is how Facebook regulate who can use their API
		* Our app sends the user to Facebook to login there
		* Facebook will say "are you sure you want to give puppygifs.com your info?"
		* The user's computer makes a request to Facebook and receives a response. At some point they have to be sent back to our site. 
			* How does Facebook know to send user back to our site?
			* Because user is sent to Facebook along with a request token, which tells Facebook which app the request is coming from. 
		* Facebook sends user back to our site with access token and callback
			* What info do we need from user to ensure their identity? 
			* User's access token can be sent from our site to Facebook. Facebook responds with info and permanent access token. 
			
### Steps Redux
			
1. User goes to our site
2. Our site goes to Facebook and asks for a request token
	* Our site tells user to go to Facebook and login with request token
3. That triggers user's browser to go to Facebook and do a login with that request token. 
	* At that point Facebook knows who it gave that request token to. 
4. User logs in, then Facebook sends them back a response that has a login token, as well as a callback URL that it'll send the browser to
	* That callback URL is a special one we set up on our site that listens for login info
5. Our app takes login token from user and sends it to Facebook along with our credentials (id and secret)
6. Facebook replies with *info* about user that had logged info and depending on permissions we asked for in our request, Facebook may give us an access token we can hang on to for a fixed amount of time (week, month, year), which allows us to take action on user's behalf (post something to Facebook wall, etc)
	* that sends another request to Facebook with new access token and action
	* Facebook remembers access token - who it came from and what user it is for and will do action
	
### Code Along

**What's Happening?**

* Login with Twitter route is ```/users/auth/twitter```
	* My app knows that the first thing I need to do is get a token 
* Twitter URL includes **oauth_token**. 
* After user logs in to Twitter they'll be sent back to our site along with their *twitter user id*
	* site finds user's twitter id and then logs them in

**Code Bro**

* Gemfile
	* You need Devise to make this work
	* You also have to use omniauth gems for your specific provider: omniauth-twitter
		* This gem auto loads in omni auth. You could also just require 'omniauth' as well, but that's not necessary
* Set up normal devise user
	* loading devise gem
	* generate devise:install
	* generate devise model for user
	* migrate users
* Add omniauth strategy in ```devise.rb``` - :twitter, and consumer key and consumer secret
* Routes
	* We have our normal root route
	* We have normal resources route
	* For signing in and out we need devise routes 
		* part of devise config process
		* since we're doing omniauth as well, we need to add the callback
		
			```
			devise_for :users, controller: {omniauth_callbacks: "omniauth_callbacks"}
			```
			
* We'll have a separate controller handling user giving us info from twitter
	* we'll create an ```omniauth_callbacks_controller.rb``` file
	
		```
		def OmniauthCallbacksController < Devise::OmniauthCallbacksController
			def twitter
				user = User.from_omniauth(request.env["omniauth.auth"])
				if user.persisted?
					sign_in_and_redirect user, notice: 'Signed in!'
				else
					session['devise.user_attributes'] = user.attributes
					redirect_to new_user_registration_url
				end
			end
		end
		```
* User model
	* model for self.from_omniauth(auth) to create new user if they don't already exist