# Day 47 Notes | Week 10 Day 2

April 29, 2014

---

### Learning Objectives

* Explain the asynchronous nature of JS execution
* List the 4 methods for scheduling execution, and explain how to use them
* List potential pitfalls of timers, and how to avoid them

---

### JavaScript Timers

**JS is Asynchronous**

* JS Functions do not wait for one to finish before going on to the next one
	* We see this with AJAX requests
		* You make a request, and request returns info at some indeterminate behavior
		* If you have code belong the AJAX request that depends on the request to run, that may not be the case
* Events is another example
	* JS is events driven
	* When users click your interface that will fire off functions
		* You cannot predict the order in which users will click buttons on your page
		
**Delay**

* We can tell a function to be executed later rather than immediately

	```
	setTimeout(sayHello, 2000);
	```
	* Delay is in milliseconds 
	* The function *sayHello* will continue until we clearInterval
	
		```
		clearInterval(20)
		```
	* We call 20 because the last interval returned the id of 20. 
	* Timers return unique ids that we can use to interact with them
	
**How to Create a Timer that continually Increases By One**

* Two things need to be done:
	* Deal with user interface
	
		```
		var startButton = $('#start');
		var pauseButton = $('#pause');
		var resetButton = $('#reset');
		var timer = $('#timer');
		```
* Now just have timer start ticking
	* d
* To Pause Timer
	* Need to clear interval
	* Create reference to it
	* If we declared variable in start button function it would only be available there
		* so instead set it in the document.ready, and don't assign anything to it yet 
* Prevent multiple clicks on start
	* Set TimerActive variable that is false unless timer has started.	
	
### Pass Arguments into Timers

* If you need to pass arguments in to a function in which we're delaying:

	```
	setTimeout(cat.speak, 2000, "hello");
	```
* If you also need to pass in the value of *this*, use bind to present the functions value of this
	
	```
	setTimeout(cat.speak.bind(cat), 2000, "hello")
	```
	* or wrap in anonymous function:
	
	
		```
		setTimeout(function() { cat.speak("hello")}, 
		2000);
		```