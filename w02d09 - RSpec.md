#Day 9 | Week 2

March 6, 2014

---

###[RSpec Review](http://jakegoulding.com/presentations/rspec-structure/)

* *describe* vs *context*
	* describe for *things*
		* ```describe #matriculate```
		* more useful for complex return values 
	* context for *state* 
		* ```context "when the student is sick"```
		* Tests usually deal with permutations of state, and nested *context* blocks better represent this. 
		* Context names should **not** match your method names!
			* Explain why a user would call the method
			* ```context "assigning homework to a student"```
* before vs let
	* before for *actions*
	
		```
		before do 
			@classroom.initialize_roster
		end
		```
		* before aligns with context
		* context is used for state, before lists the actions to get to that state. 
			* For real objects:
		
			```
			context "the bathroom queue is full" do
				before do
					100.times { bathroom.queue <<  nil }
				end
			end
			```
			* For mock objects:
			
			```
			context "the bathroom queue is full" do
				before do
					bathroom_queue.stub(:full?).and_return(true)
				end
			end
			```
	* let for *dependencies* (real or test double)
		* let is lazy! 
		
		```
		let (:grade_levels) { [1, 2, 3] } 
		```
		* let defines a named variable
		* subject
			* is automatically based on the *describe*
			* can be explicitly specified 
			* can have a name
			* will be the target of 'bare' *should*s
		* use *subject* for the thing you are testing and use *let* for dependencies 