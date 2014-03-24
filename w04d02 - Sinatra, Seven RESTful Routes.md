# Day 17 Notes | Week 4 Day 2

March 18, 2014

---

### Learning Objectives

* List the seven RESTful routes
* Relate RESTful routes to CRUD
* Create a RESTful Sinatra app

---

* The Seven RESTful Routes
	* Guide to seting up routes
	* Any APIs we talk to follow REST conventions
	* Stands for **Representative State Transfer**
* The Routes:
	1. Index
		* GET '/things'
		* if resource (one of the tables in our database) is called things the route should point to '/things'
		* get all the things
		* Focus of this action is ```Thing.all```
	2. Show
		* GET '/things/:id'
	3. New
		* GET '/things/new'
		* get form so we can enter data to create new thing
	4. Create
		* POST '/things'
		* once data is entered and click submit data from that form goes here. This action is responsible for putting into database
	5. Edit
		* GET '/things/:id/edit'
		* Get a particular form, that already exists, so we can edit it. 
		* similar to new, but we're grabbing a form to edit something that already exists
	6. Update
		* PUT '/things/:id'
		* Update is to edit, as create is to new. 
		* When we click submit on that form it gets updated. UPDATE actually edits the database. 
	7. Destroy
		* DESTROY "/things/:id"

### Game of Thrones Exercise

* These apps are called *CRUD apps*
	* What we do should not be limited to CRUD apps, but they're the foundation for many things we'll do. 