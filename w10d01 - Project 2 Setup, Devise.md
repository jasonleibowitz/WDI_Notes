# Day 46 Notes | Week 10 Day 1

April 28, 2014

---

### Project 2 Notes

**Due Today**

1. User Stories
2. ERDs
3. Link #1 & #2 in Project Readme
4. Add HAMco as Collaborators on Repo & Everything else

**Questions**

* File GH issue

**Scrum**

1. What did you do yesterday?
2. What are you doing today?
3. What's in your way?


---

### Devise

* Gem for User Auth
	* Does user auth with passwords
	* Handles sending conf emails to valid email addresses
	* Handles forgotten password
	* Manages cookies and sessions to say 'do you want me to remember you'

**Code Along**

* Create avengers_app rails app
* Create model avenger with name and power

	```
	rails g scaffold Avenger name power
	``` 
* According to Devise docs, run ```rails generate devise:install```
	* Bunch of instructions come out
* Create model that utilizes devise

	```
	rails generate devise User
	```
	* This creates user model, migration (for users table), test stuff, and it wrote something into the user model and created a route for devise_for user
	* This added devise modules to our user model
	
		```
		devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable
		```
	* devise uses bcrypt
	* user model already has secure password, we can create new password, and if we forget password devise will help user recover it
* Run db:migrate

**Routes**

* We get a bunch of routes for free with devise
	* devise_for is a helper that does a lot of this for us
* Devise Controller
	* Go to '/users/sign_up'
		* This controller and views are hidden from us
		* It's a *rails engine*, which means you don't explicitly see everything it does
		
* What do we still have to do
	* Links to sign in
	* Links to sign out
	* etc
* Devise helpers
	* user_signed_in?
	* current_user
	* user_session
		* if you wanted to put something in the session variable, use this

**Sign Out**

```
<%= link_to "Sign Out", destroy_user_session_path, method: 'delete' %>
```

**Sign In**

```
<%= link_to "Sign In", new_user_session_path %>
```

**User Authorization with Devise**

* According to docs ```before_action :authenticate_user!

**Edit Sign In view File**

* Out of the box there are no devise views, and devise pulls the views from it's engine
* However, if we want to edit the views, look at the docs under *configuring views*
	* ```rails generate devise:views```
	* here devise makes a copy of it's default views files and puts them in your project so you can override the default
	* Now you have a devise folder under views


### Forgot Password

* [First set-up action mailer](http://guides.rubyonrails.org/action_mailer_basics.html#example-action-mailer-configuration)
	* There are a few different delivery methods you can use
		* Send Mail is an open source delivery software that lives on most Unix servers
	* Config Environment File
		* delivery errors set to true
		
			```
			config.action_mailer.raise_delivery_errors = true
			```
		* config delivery_method to :sendmail
		
			```
		 	config.action_mailer.delivery_method = :sendmail
			```
		* Other settings
		
			```
			config.action_mailer.perform_deliveries = true
  			config.action_mailer.raise_delivery_errors = true
  			config.action_mailer.default_options = {from: 'no-reply@example.com'}
			```
		* default host 
		
			```
			config.action_mailer.default_url_options = { :host => 'localhost:3000'}
			```
			* can use path helpers to access this
	* Restart Server
		* for changes to take effect 
* Forgot button automatically shows up

**RailsCasts**

* [Setting Up Devise](http://railscasts.com/episodes/209-introducing-devise)
* [Customizing Devise](http://railscasts.com/episodes/210-customizing-devise)