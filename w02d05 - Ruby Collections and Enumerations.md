# Day 10 | Week 2 Day 5

---

### Ruby Collections & Enumerations

* Select items from inside nested arrays. 
	* ```a = [['hi', 1],{name: "adam", age: 9001}]```
	* how to get the word hi
	
	```
	a[0][0] => 'hi'
	```
	
	* how to get the word 'adam':
	
	```
	a[1][name] => 'adam'
	```