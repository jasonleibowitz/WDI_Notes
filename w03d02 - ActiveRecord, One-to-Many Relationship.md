# Day 12 Notes | Week 3 Day 2

March 11, 2014

---

### Learning Objectives

* Explain what functionality AR Associations give us
* Set up an AR association for a one-to-many relationship
* Set up an AR association for a many-tomany relationship

---

### Active Record

* Review
	* Let's your program talk to the database in its own language
	* We interact with objects to do things with our database
	* The attributes (instane variables) in Ruby map to the columns in the corresponding table
* Active Record Methods
		
		class Cassette < ActiveRecord::Base
	
	
		end
	
	** Creating **
		
	* ```Cassette.all``` - grab all records in database
		* Returns an array of cassette objects
	* ```c = Cassette.new``` - make a new cassette
		* But not yet in database
	* ```c.save``` - saves a new instance of the Cassette object to the database
	* ```c = Cassette.create``` - makes a new cassette and saves it to the database
	
	** Finding **
	
	* ```Cassette.find```
		* find with the primary key
	* ```Cassette.find_by(:name => 'song_name')```
		* find with a column value
		
* iTunes Example

	** Find **
	
	* Find all songs by Metallica
		* Step 1 - Find the artists's id
		
				metallica = Artist.where(:name => "Metallica")
				
			Note: ```.where``` always returns an *array* to us, even if the array only has one thing in it. To get it out of the array we use ```.first```
			
				metallica = Artist.where(:name => "Metallica").first
				
			.find_by is a better method because it doesn't return an array, but it only returns one object
			
				metallica = Artist.find_by(:name => "Metallica")
				
		* Step 2 - Return all songs by metallica (using id of artist)
		
				metallica_songs = Song.where(:artist_id => metallica.id)
				
** ActiveRecord Association **

* What is ActiveRecord Association?
	* Active Record makes it easy for us to tranverse the relationship between different entities. 
	
** One-to-Many Relationship **

* Many Side -- In the class Ruby file add the following:

		class Artist < ActiveRecord::Base
			has_many(:songs)
		end
		
	* This allows us to automatticaly connect to the artist's song without creating the connection ourself. We connect by typing:
	
			metallica = Artist.find_by(:name => "Metallica")
			
			metallica.songs
			
	* It automatically links to a table named songs, which is lower-case, pluralized Song class. 
		* In the songs table it's looking for lower-case artist_id as the table it uses. 	
			* ActiveRecord expect this convention. If you named artist_id a_id instead, this would not work. 
	
* Create a song under a specific artist id:

		megadeth.songs.create({:title => "Hangar 18", :genre => "Metal", :length => 328})
		
	* Because we create this under ```megadeth.songs``` we don't have to *manually* assign the ```artist_id``` as ```megadeth.id```. 
	
* If you create a song in PSQL without an artist_id, you can push it into the metallica.songs array:

		sad = Song.find_by(:title => "Seek and Destory")
		
		metallica.songs << sad
		
* One Side -- Add the following to the one side of the one-to-many relationship:

		class Song < ActiveRecord:: Base
			belongs_to(:artist)
		end
		
	* Now we can do ```sad.artist``` to see the artist of song even though the column in song is only artist_id. 
		* This pulls the entire object of the associated artist. 