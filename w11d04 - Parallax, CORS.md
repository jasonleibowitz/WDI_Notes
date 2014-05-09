# Day 54 Notes | Week 11 Day 4

May 8, 2014

---

### Morning Exercise: Parallax

* Positioning of Background
	* Move it to the bottom of the stack by setting the *position to fixed* and *z-index of -1*.
		* This makes background stick there and sets it behind everything else
		* All elements of a default z-index of 0
* Set position of images
	* Position should be *relative*
* Track scroll of window
	* We do this with jQuery's ```$(window).scrollTop()```
		* This tells us the number of pixels out of view from the top of the window
		
			```
			$(window).scroll(function(){
    			var scrolled = $(window).scrollTop();
    			console.log(scrolled);
    		});
			```
	* We want to update our bg's top css property by the negative of the scroll amount
		
		```
		$('.bg').css('top', -(scrolled * 0.25) + 'px');

		```
* Grab images and make them scroll too
	* Images should scroll in opposite direction of background
	
		```
		$('.top-left').css('top', (scrolled * 0.8) + 'px');
		```
	* We can also transform images with scroll
	
		```
		$('.top-left').css('transform', 'rotate(' + (scrolled * 0.1) + 'deg)');
		```
* Track mouse movements

	```
	$(window).mousemove(function(e){
		console.log(e.offsetX);
	});
	```
	* This will log the x position of our mouse
	
---
	
### CORS

**Cross Site Request Forgery (CSRF)**

* **Links**
	* Clicking links are safe because it's just a GET request
		* GET requests never do anything dangerou
* **AJAX**
	* The other way browsers can make requests other than clicking links and submitting forms is via AJAX
		* So far our AJAX requests have been going back to the **same origin**, which means if I'm on kittengifs.com and say to make an AJAX request it'll go to the same domain we're on. 
	* How to make safe AJAX requests to other domains, like API calls? 
		* The browser tells the server where the request comes from.
		* The browser checks for permissions first - does the server allow AJAX request from wherever the request is coming from. If it does, then the browser sends the request along. 
			* This is called a **pre-flight request**
		
---
 
* Ensure requests coming from one site are coming from the correct site
	* The server puts a token on every form it generates and remembers all token for a certain amount of time and who received that token. The server then expects that token to be included in every form submission. If that token is not in the submission the request is ignored. 
* Rules
	1. Cookies are limited by domain and the browser's job is to protect that (send cookies only to the domain they belong to)
	2. Forms/CSRF/Auth Token
	3. CORS
	
### Movie.r Backbone App

* 