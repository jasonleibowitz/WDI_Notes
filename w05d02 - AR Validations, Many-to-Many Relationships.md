# Day 22 Notes | Week 5 Day 2

March 25, 2014

---

### Learning Objectives

**Validations**

* Understand what AR validations are and how they affect persistence
* Compare AR validations with SQL constraints
* Understand when validations are run and how they can be bypassed
* Understand what AR errors are
* Use common validations in a Rails App
* Display errors on c form

**Many-to-Many Relationships**

* Implement a many-to-many relationship in a Rails app utilizing: 
	* has_many_and_belongs_to
	* has_many, through

---

### Validations

**Intro**

* Definition
	* Way to validate data coming into our app according to some rules - (usually from users), sometimes from API
* How do validations work in Rails?
	* Defined in models because validations are all about data, and our models are our interace to our database validations. 
* Benefits vs Constraints
	* Constraints limited in what can be defined
		* presence
		* uniqueness
		* greater-than, less-than, equal-to
	* what can't they do
		* verify that an email address looks like an email address
	* performance
		* AR validations allow us to not hit the database before we check if it's valid

**Coding Validations**
		
* How to define a validation on a class:

		class User < AR::Base
		
			validates :email, uniqueness:true
			validates :name, :email, presence:true
		
		end
		
	* validates is a class level command
		* options
			* presence
			* uniqueness 
			* etc
	* you can have multiple validates call on one line
		* each validates call has at least one attribute to validate, then takes at least one value to validate
* When do the validations get run?
	* Only when data is being saved into the database
		* ```create```
		* ```save```
		* ```update```
	* When you run a ```.valid?``` method on an object
		* will return true or false
		* flip version is ```.invalid?```, which returns true if invalid
	* *Note*: If you call methods (create, save or update) without a bang then your method won't run and keep running. If you add a bang then it will throw an error
* If object is invalid
	* returns false
	* add email attribute to errors hash
		* hash where keys are attribute names and the values are strings like "must be unique"
		* nothing gets put or updated in the errors hash until you call one of the four validation methods
* Code Along

		me.errors.full_messages
		
* Bypass Validations
	* There are a number of methods, like ```update_attribute``` or ```update_column``` where validations aren't run
	* We can also run ```save(validation: false)``` and we can save regardless of validation
	* When would we bypass validations
		* Normal users need to have validations on data they input, but admins could by pass them
* Validations for Tunr
	* Users
		* :email, present, uniquness
		* :password, length(5)
		* :name, presence
		* :balance, numericality, >0, decimal places
		* :admin, presence 
	* Songs
		* :title, presence
		* :artist, presence (associated artist)
		* :link, presence
		* :artwork, presence 
* Note for Rails Console
	* If you edit your models definition you need to run ```reload!``` for rails console to update
	
**How to display errors on form**

* Implement an if statement that if there are any errors to list them. 

```
<% if @user.errors.any? %>
    Please correct the following errors:
    <ul>
      <% @user.errors.full_messages.each do |msg| %>
        <li><%= msg %></li>
      <% end %>
    </ul>
  <% end %>
```
**How to highlight fields with error**

```
.field_with_errors input {
  border: 2px solid red;
}
```
	* form_for automatically puts ```div``` around fields with error

---

### Many-To-Many Relationships in Rails

**Join Tables**

* We need a join table when creating a many-to-many relationshipi
	* the join table will hold the playlist_id and song_id. It will look like:
	
	|  playlist_id | song_id  |
	|---|---|
	| 1  |  1 |
	| 1  | 2  |
	| 1  | 4  |
	|  2 | 5  |
	|  2 | 2  |

	* if the join table is empty, you have playlists, but they don't have any songs in it
		* you do, however have songs, but they don't belong in any playlists yet
* Process of adding song to playlist
	* if we want to add song 3 to playlist 2 we're adding a new row with "playlist_id = 2" and "song_id = 3"
* Find all songs on playlist 2 (join table is called *relationships*)
		
		Step 1 - p = Playlist.find(2)
		Step 2 - r = Relationship.where(:playlist_id => p.id)
		
		Step 3 - r.map do |re|
			Song.find(rel.song_id)
			
		end
		
**Many-to-Many in Rails**
		
* Naming a Join Table (Accessing in Rails)
	* snake-case name of both ids in table, but in alpha order: ```playlists_songs```
	* when we do this, Rails does step 2 and 3 for us
	
			p.songs
			
			# returns an array of all songs in the playlist for us
* id field in Join Table
	* We don't need an id field in the playlist because we never use it
		* ids are only for models. the objects in this table are not models
		* in fact, we will get errors if we put an id
		* We have to tell Rails *not to* create an id column
	* The only thing we have to ensure is that no two records have the same playlist_id and song_id, because then you can't differentiate between them
* Class relationship
	* In a one-to-many relationship
		* If we put a has_many and belong_to in the models, we can call ```artist.songs``` instead of ```Song.find_by(artist_id: metallica_id)```
		* This doesn't establish the physical relationship in our database, but id does give us sweet tools to traverse this relationship. 
	* Similarly, in a many-to-many relationship,
		* We need to put ```has_and_belongs_to_many(:songs)```
		* We still need to make the join table with the name ```playlists_songs``` AND with the attributes ```playlist_id``` and ```song_id```
* This type of JOIN Table in Rails is called a **HABTM table**
	
**Many-to-Many Relationship Through a Model**

* Users Purchasing Songs
	* We need to keep track of users purchasing a song
		* Know if we need to put a purchase button next to a song
		* Know how much money a user spent on site
		* Know what kind of music a user likes
	* Create a new table called ```purchases``
		* Each purchase goes into this table as a row
		* Attributes
			* user_id
			* song_id
			* amount
	* Each purchase is a thing
		* Like a resource
		* a user has many songs through purchases
		* a song has many users through purchases
* Multiple many-to-many relationships
	* If a user has a many-to-many relationship to songs via purchases AND via rating, user.songs can mean two things. 
	* How do we set what user.songs means?
	
			class User < AR:B
				has_many (:songs, through :purchases)
			end
* Purchases
	* If user has purchased song, then make download info visible to them. If they have not purchased it, then download info not visible 
	
**Create has_many, through in Rails**

1. Generation Migration of New Join Table
	* ```generate migration CreatePurchases```
2. Create model
3. Create relationship to purchases in Users
	* ```has_many(:purchases)```
4. Create Relationship from Users to Songs through Purchases
	* ```has_many(:songs, through: :purchases)```
5. Relate song to user
	
		has_many(:purchases)
		has_many(:users, through: :purchases)
		
6. Add helper methods to Purchases
	* We want to be able to do: 
		* purchase.user
		* purchase.song
		
		```
		class Purchase < ActiveRecord::Base
			belongs_to(:user)
			belongs_to(:song)
		end
		```
7. Create purchases_controller 
	* One action. We will get use that is making purchase and song that is being purchased. 
		* in other words, think of a good route that should map to this create action.
	
		```
		class PurchasesController < ApplicationController
			
			def create
				
			end
			
		end
		```
8. Add path to routes.rb

	```
	post '/purchases', to: 'purchases#create'
	
	```
	
	* since this is a post route we should assume the info is coming from a form.
9. Add form to allow users to purchase songs (added to song index)

	```
	 <form action="/purchases" method="post">
          <input type="hidden" name="song_id" value="<%= song.id %>">
          <input type="submit" value="Purchase!">
     </form>
	```
	
	* because of Rails authenticity a plain form won't work. We instead need a <% form_tag %>
	
	```
	<%= form_tag("/purchases", method: "post") do %>
		<% hidden_tag :song_id, { value => song.id } %>
		<%= submit_tag "Purchase!" %>
	<% end %>
	```

---
	
* Tun.r Homework
	1. Show all purchased songs on a user's page
	2. Add playlist functionality
	3. Remove purchase button after it is purchased
