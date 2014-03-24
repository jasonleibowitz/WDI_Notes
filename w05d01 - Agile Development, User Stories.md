# Day 21 Notes | Week 5 Day 1

March 24, 2014

---

### Learning Objectives

* Explain Agile Development
* Describe a user story
* Describe the benefits of user stories
* Pivotal Tracker

---

### Agile Development

* More error checking built-in
	* testing project throughout the process
* Break project into smaller features
	* Have a large group of people working on separate stories communicating all the way through
* Waterfall Development
	* Cons:
		* Harder to find errors
* Tools
	* RSpec/Testing
	* ERDs
		* Figure out models you want to have, their attributes, and how they relate to each other before you even begin coding
		* Helps us plan out out thoughts and how the entities of our app will work together
	* REPL (Pry)
		* Allows us to debug our app while we're building it and not once everything is starting to fail
	* user stories
* Problems
	* Syntax
	* Architecture related to UX/UI
	* blank page problem
	* order, how to build it

### User Stories

* First Step of Agile Development
	 * Always have a plan. Never start coding without having a plan first.
	 	* User stories help us solve a lot of these problems.
* Feature
	* Particular functionality of application
	* What types of users?
		* Power Users
		* Small-time user
		* Admin
	* Make a user story for each *type of user* you may have on your site
* Structure of a User Story
	* As a (blank), I want to (blank) so that (blank).
		* A *who* a *what* and a *why*. Who are they, what do they want to do and why do they want to do it?
	* Power User Example
		* As a power user I want to make a catalogue of all the movies in the world so I can always have info about movies. 
	* If you ever find yourself with a feature with no user benefit, you may not need that feature. 
	
** Parts of a User Story **

* Scrum
	* Analogy for agile development
	* 3 core components (pigs)
		1. Product Owner
			* Stakeholders
		2. Development Team
			* Group that does actual work
			* 3-9 people
			* Engineers, developers, designers, analyzers, etc.
		3. Scrum Master
			* Not necessarily a developer, but is a buffer between developers and potential distractions that would stop developers from building product on time
			* Support role
				* Example: this person keeps coming to my cubicle and asking me questions so I can't focus on my work
				* Another example: I can't build this part until the other part is built and the person building that is on vacation
			* Deals with project as a whole where project manager deals with people on the project
	* ancillary roles (chickens)
		* auxiliary roles: consultants
		* shouldn't have as much say
	* sprint cycles
		* entire team runs on sprint cycles (days - months)
		* take product backlog (features that need to be built), pick next features to build, then go through sprint of actually building it out
		* during these cycles you have daily scrum meetings (usually standing in start-ups)
			* to reiterate we team is doing each day
		* end of sprint cycle you have iteration or feature of product that is fully functional or tested
	* Very collaborative and team oriented
* User Story
	* 
* Minimum Viable Product (MVP)
* Acceptance Testing
* BDD vs TDD
	* TDD
		* Concentrates on short development cycles. 
		* Write a test, it fails, then write the method to make it pass. 
			* We do this throughout the writing of the app
		* For someone developing an app by himself, or for a very tight group that can understand what's written
	* Behavior Driven Development
		* More verbose
		* Easier for non-developers (or people not on the team) to understand what's going on in the tests
		* BDD takes into account functionality for the end-user
			* What the user wants to use the function for on the actual web-app
	* Problems that BDD solves
		1. Where to start?
			* TDD solves the blank-page problem. Write a small test to get started.
			* BDD not only helps you start, but helps you be mindful of where to start for the user. You're thinking of the business-value for the project (the end-user)
		2. What to test
			* Looking at business-value it brings to the project and thinkin about that when thinkin about what to test first
		3. How much to test in one go
			* what's do-able, i.e. how much you should be testing
			* you're using the user-story approach
		4. What to call test
			* Going back to the perspective you're giving to the rest of the team, BDD is more verbose so people outside of your team can understand where you're going with your test
		5. Understand why test failed
			* Helps to know why a test failed, but also looking at how the failure would affect the end-user
	* Focuses more on the outcomes of writing a test and not so much the implementation details of how you're going to perform an action
		* User login
			* TDD - we'd test that every step along the way works (can add, email, phone, etc). But if we want to add a different category, all of our tests would fail because they would only test certain attributes
			* BDD - checking that a user enters the login page and the result comes out. 
* Domain Modeling vs Problem Modeling
 	* Takes place in early stages of app
 	* Domain Modeling is just one part of Problem Modeling
 	* Domain Modeling
 		* Laying out entities working with attributes and relationship that those entities have
 	* Problem Modeling
 		* Laying out problem and thinking through approach
 		* Modeling the overarching issue: this is what I want to build, what steps do I need to take to build that. 