# CSS with Larry Buchanan

March 29, 2014

---

* make something dominant
	* peole don't know where to look
	* Food Network
		* lack of dominance
		* slider has same amount of visual weight as lower chunk of links. font is too small
		* ad is so big that all thre ecomponents compete for attention
		* red is the most powerful color there is
			* your eyes will focus on red first
		* eye also drawn to big stuff
		* decide what's important
			* make those thing more dominant 
	* Airbnb - better
		* one big image. 
		* you know to go to the one search box
* use 2 fonts
	* serif
		* has feet
		* serious, business
	* sans serif
		* no feet
		* helvetica or arial
		* fun, light-hearted
	* pick one serif and one sans serif
		* want contrast
		* two of the same types aren't different enough
		* give different fonts different jobs
			* header
			* logo
			* body
				* never less than 16px (not like 16pt)
	* web font resources
		* google.com/fonts
		* beautiful web type (hellohappy)
			* aggregates the best web fonts from google fonts
		* typekit
		* cloud typography
	* go to fonts for larry
		* georgia
			* standard on comps
			* always render as georgia
			* what times uses for body
			* designed for screen
			* times new roman
				* default if you don't set font, and set as serif
				* harder to read on screen
		* helvetica
			* track it in a little bit and you've got NYC Subway branding
	* fonts in vogue
		* cloud typography
		* gotham
			* free version is proxima nova (typekit)
			* free version of proxima nova is raleway
				* decent substitute 

* consistence
	* builds street cred
	* as you maintain consistency your website gains credibility
* don't use too many colors
	* use black, white and one other color (blue, green, red)
	* dark dark grey and white is even better - makes site more sophistocated 
		* sets thing back in page a little more
	* color palette 
		* kuler.adobe.com
* process
	* make sure you have a solid design base before adding to it
* move stuff around for a while
* cinderblock
* skeleton
* larrybunch.com/resources
* emmet

---

### Building a Website
* basic foundation
	* [getskeleton](http://www.getskeleton.com/)
		* very basic and customizeable
* advanced foundations
	* [zurbs foundation](http://foundation.zurb.com/)
	* [twitter bootstrap](http://getbootstrap.com/)
* Responsive
	* great
	* maybe think about start designing for mobile
		* nothing says to start with desktop site then scale down
	* skeleton gives you a responsive grid system
		* someone did the math on using media queries
		* you just build on top of it
* skeleton.css
	* gives us basic grid layouts so we don't have to worry about floats
* [web typography](http://practicaltypography.com/typography-in-ten-minutes.html)
	* good to know how to use this
* sketching
	* always sketch site before you code anything
	* get a rough idea so everyone's on the same page
	* very helpful
	* gives us a guide about where we want to go
	* css classes
		* write out all css classes before we code them
	* example site:
		* container
		* header element
			* h1 - logo
				* only one h1 per page
				* should be the most important thing on the page (headline)
			* nav element
				* inside header
			* note: header and nav are html 5 elements which is the same as saying div.header
		* container for body (wrapper)
			* posts
			* sidebar
* Emmet
	* ! - html skeleton
	* Lorem - lorem text
	* nest within
		* this
			![alt][before.png]
		* turns into this
			![alt][after.png]
* Design
	* Give form to content, not content to form
	* content will dictate form
* Comments
	* Comment when divs close
		* END HEADER
		* END POSTS
		* etc
* HTML Entity
	* ```&copy;``` = &copy;
	* ```&amp;``` = &amp;
* Reset Stylesheet
	* Every browser will render your stuff very differently
	* If you want to reset all browsers and start from the same rules, use a reset stylesheet
		* same as normalize
* Link stylesheets
	* link reset
	* then link to skeleton (could also use skeleton's base reset)
* Style.css
	* Line Height
		* golden ratio - 1.62
	* Font Family
		* ```font-family: Georgia, serif;```
			* backup is serif
	* Font Size
		* Default is 16px
		* ```em``` inherits font from inside container
		* ```rem``` inherits font size from body 
	* Give classes bold background colors
		* to help with positioning
			* whatever you want to be full width you call it class "sixteen columns"
			* "ten colums" makes it only 10 columns
			* "six columns" makes it 6 columsn
		* offset columns
			* two columns offset-by-two
* width
	* width: 100%; vs background: cover;
		* 100% refers to div it's within
			* so you don't have to set height
			* so div resizes and image within it resizes too
		* background:cover; more useful when you want to set it as background image
* put space between things
	* delinate what's the header, where the posts are, then add style to fonts
* Example site
	* get nav in right spot
		* margin-bottom: 20px;
			* mainly use bottom. helps if you only use it the same way, so if somethings off you know what's pushing it off
		* get nav options in line with logo
			* currently display block because they each have a full line
			* float right pushes to other side
	* letter spacing: space between characters
	* text-transform - all caps
* Stylesheet order
	* good practice to order by how it appears on page
	
larrybuch@gmail.com