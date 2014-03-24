# Day 15 Notes | Week 3 Day 5

March 14, 2014

---

### Bodega Code Review

* Why use methods?
	* DRY up your code
	* Readability - in the main body of your code keep things simple and short
* Proc vs Block
	
		Employee.all.map(&:info).join("\n")
		
		instead of 
		
		Employee.all.map { |e| e.info }.join("\n")
		
	* (&:info) - is two procs. Adam calls it "two proc shakur"
	
* Make tests general, not fragile
	* Don't make the test of a list method ```to eq("expected_string")``` because that's very fragile. If you change the order of how the string appears, your code will still work, but your test will break.
	* Instead use:
	
			.to include(l1.name)
			.to include(l2.name)
			.to include(l1.address)
			.to include(l2.address)
			
* HEREDOC
		
		menu = <<-MENU
		==========
		Main Menu
		==========
		1. 
		2. 
		
		MENU
		
	* If you write two less than signs and a word in all caps, then that same work again later on, everything in between those words will be a stirng. 
	* If you write ```<<-MENU``` then the last ```MENU``` does not have to be aligned all the way to the left. 
	
* Case Statements
	* great for equality
		* if ```menu_choice == "l"``` can be when ```"l"```
	* not great for comparisons
		* if ```menu_choice > 5``` use an if/else statement
* ActiveRecord Stores database queries in cache
	* To tell it not to do that you can put the method ```self.reload``` in the method that queries the database
* Short if/else statement

		return "No Products\n" if self.products.count == 0
		self.products.list
		
	* this will return "No Product" if that condition is true, else it will do the other thing. 
* ActiveRecord ID Matching:
	* If we need to pass in a location_id when creating a new Product object, we can pass in the full object itself, rather than just the ID. 
		* This is because ActiveRecord will automatically find the id of the location object when it has a column called ```location_id```
		* To be explicit you can add ```REFERENCES location(id)``` in the schema. 
	* This works because of *convention over configuration*. 
	
* Clear Screen	
	* Ruby command to just clear screen:

			puts "\e[H\e[2J"
			
* Except Error Catching

		begin
			code
		rescue Exception 
			puts "invalid product ID"
			next 
		end
		
	* next does the next iteration of the while loop
		* next only valid inside loops
	* the above Exception catches any error, we want to be more specific. So we may want to do ```rescue ActiveRecord::RecordNotFound``` instead