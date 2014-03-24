# Day 13 Notes | Week 3 Day 3

March 12, 2014

---

### Learning Objectives

* Explain and use the client-server model
* Define a "communication protocol" in reference to computer communication
* Name the 4 basic parts of the internet stack
* Explain and use why we have a stack
* Describe the basics of the HTTP protocol
* Compare and contrast two modes of HTTP
	* Are we serving file mode (GET /logo.png or GET /index.html) or resource mode?
	* Resource mode
		* For when things become more dynamic. Picture Google Calendar. There isn't a file on their site that represents everyone's specific calendar, instead the server builds it on the fly.  
* Explain difference between GET & POST
* Explain the parts of a URL in detail

---

### The Internet

* Communication
	* Sending a message
		* At it's core the internet is about communication between different things. 
		* What does communication consist of?
			* Sending a message
		* Examples
			* Git
				* When we do ```git status``` git sends us a message in a textual form
			* Ruby
				* OOP - methods are us	 sending messages to objects
	* Rules
		* communication protocols
			* Layering
			* Standards
				* Where we define clearly what the rules are and how different people trying to write/use these protocols can follow them
				* Standards allows us to build an implementation 
					* When you take a set of standards and turn them into a functioning product (software or somtimes hardware)
					* There are fundamental technologies the internet depends on (IP address, for instance), which are open standards that have to be implented on every piece of hardward and software to make sure that all pieces of software/hardware can interact with each other. 
			* The core things that protocols must cover are **syntax**, **semantics** and **synchronization** of communication.
				* semantics
					* In example of: ```somelocation.inventory_list(num)```
					* semantics are a) what does inventory_list mean and b) when we pass num = 5, maybe we only want the first 5 things
					* the meaning of the messagae is encoded in the semantics
				* synchronization: 
					* the order in which the messages go
					* and more
			* Implementations
				* Pieces of software or hardware that actually implement the protocol (make it come to life) 
				
				|  TCP/IP MOdel |
				|---|				
				| application  |
				| transport  |
				| IP  |
				| link |
				
			* Addressing
				* Directing where your message goes. 
				* The receiver of the message in OOP is the thing we called the message on: ```receiver.message(argument)```
			* Data Format
				* How do we encode the message
		* Example
			* Introduction scenario (in business context). 
				* If Adam is introducing McKenneth to Dave, you introduce the junior person to the senior person. Then introduce the other way. Then shake hands. 
				* **Synchronization**:
					* Rules about order in which things happen
				* **Semantics**:
					* Each piece has meaning. 
					* When Adam says "McKenneth, this is Dave" now McKenneth knows his name and there is meaning behind the messages conveyed. 
				* **Implementation**:
					* When Adam decides to follow the guidelines and introduces someone
					
					
* Internet Stack
	* Application (FTP vs HTTP vs Gopher)
		* FTP
			* File Transfer Protocol
			* Sending files
		* HTTP
			* Hyper Text Transfer Protocol
			* Foundation for communication for www
			* How client and server communicate 
			* text
			* request info from server/send info to server
				* built around client-server model
				* can also upload via HTTP (think attach file via gmail)
		* Gopher
			* distributing, searching, and retrieving documents over the internet
			* interface is like a finder GUI - very much document, folder oriented
			* used because it has simple-syntax and is simple and quick
				* however, lost out to HTTP because it made a user download all of the files and set up in a 'choose your own adventure' style rather than presenting everything to user
			* very menu-driven
			* *the web as it could have been*
		* SSH
			* connect to a remote computer's server
				* get a terminal for a remote computer, but run it locally
	* Transport (TCP vs UDP)
		* TCP (Transmission Control Protocol)
			* one of the main protocols of the IP suite
				* Within suite there is internet protocol (handles addressing, getting packets from source to destination)
			* TCP brings reliability, ordering and error checking
				* If a router gets overloaded it could drop some data. So we need a way to detect when messages don't get delivered properly and resent them. TCP does that. 
					* We send packets over internet that needs to get reassembled on the other end. (**chunking** is sending in smaller pieces) Each piece is labeled so it can be put together correctly. TCP gives us this.
			* TCP is like having delivery confirmation so you know if something got delivered 
			* When you want to create a TCP connection you talk to the other end and they say they're listneing and for every piece of data you send they send you a message that they've received it. This is called the **TCP handshake**. 
		* UDP
			* another important layer of the IP stack
			* send entire data as datagram instead of breaking data up into packets
			* can form a connection with another host without being connected before
			* doesn't add header 			
			* Compared to TCP
				* Does not guarantee that your data got delivered
				* We're trading reliability for speed
			* When to use?
				* This is great for video conferencing - if you drop a small piece of video conferencing data, you have a small glitch, but it mostly works and you get a lot more speed -- 1% data loss for 20% speed bump. 
				* Don't use if you need to ensure data is consist and that it gets there
	* Internet
		* IP
			* Sends packets across internet. 
				* Packets consist of header and message
					* Header - source, destination
					* Message - what's the message we're trying to send
			* Depends on the link layer
				* You can't send packets over the internet unless you're connected to the internet
			* IP is all about how to identify the source and destination and how to get it there
				* To send messages over internet we need to name (give addresses) to sender and receiver
				* Addresses must be unique for the routing to work or router won't know which one to send it to
		* DNS
			* it's basically a giant phone book that translates URL names into an IP address
			* on it's own a router wouldn't know how to send a url like google.com, so we translate it into an IP address
			* System of DNS servers all talk to each other and share their phone book information
			* Your router is assigned a name server. when you search for google.com it asked that name server for google.com's ip address
	* Link (Physical)
		* IEEE 802.3
			* for establishing a *direct connection* and about the signal of that connection
				* connects hubs, switches and wires
				* also says something about how we're moving electrons of photons along to wire
			* creates a LAN over a short distance
			* specifies standard for POE (power over ethernet)
			* limit is 330 ft for copper ethernet (fiber optic can go much further)
		* IEEE 802.11
			* Wireless (WLAN)
				* when two try to connect at the same time it's called a collision, and the base station can't hear either. 
				* WiFi has rules about how to deal with collisions, frequencies of messages, when it hears a collision it will tell devices to slow down and stop sending messages as frequently
			* MAC
				* hardward id
				* it's how routers knows a little about each device even without an ip address
				* when routers talk to the base station, the computer can talk to the base station even without an ip address
					* it's like everyone having a unique voice
			* a, g, b
			
* Layers of Abstraction
	* When writing an FTP program, you only have to speak TCP or UDP, but don't have to care about details of how the ethernet cables work. 
	* This abstraction allows us to free up our minds to think about programs in a higher level context and allows us to create complex system.
	* As web developers we are focusing on HTTP

* The Internet Stack as the US Post Office
	* If we talk about mailing a letter in the USPS
		* Link/Physical
			* Message on sheet of paper
			* Put in envelope
		* Internet
			* Put source and destination on envelope = IP Address
		* Transport
			* Put letter in mailbox = make TCP request
				* Mailmail checks for stamp = TCP rules to see if valid
			* Mailmail takes to post office, who sorts it and takes it closer to destination (i.e. nearest post office for destination)
			* Post Office sends mail to distribution center, who mostly cares about destination zip code, i.e. IP address
			* Post office sends to another DC, to another and then back down. 
		* Application
			* I want to mail a letter = I want to send a file (FTP)/I want to visit a webpage (HTTP)
			
---
			
### HTTP

* client/server
* request/response
	* Loading webpage
		* When we load a webpage we make a **request**:
			* header
				* to: google.com
				* path: /  (what are we requesting)
				* verb: GET
					* HTTP has a number of verbs. It's akin to saying 'what do i want and what do i want you to do with it'
				* get is the most common verb
				* other verbs:
					* GET 
					* POST 
					* PUT 
					* DELETE 
				* verb is like CRUD
					
					| Verbs  | CRUD  | SQL  |
					|---|---|---|
					| GET  | READ  | SELECT  |
					| POST  | CREATE  | INSERT  |
					| PUT  | UPDATE  | UPDATE  |
					| DELETE  | DESTROY  | DELETE  |
			* body
				* doesn't always have body
				* can include form data
		* Response (Expect to receive):
			* HTML -> text
				* If there are images on the page, it makes the request for text first, then makes another HTTP request for images
		* The HTTP request sits inside of one IP packet
			* header
				* Status Code (3-digit numbers)
					* 200 - everything worked
					* 404 - error, page not found
					* 500 - errors related to server themselves having problems
				* Length
					* We need to know when the end of the response is
			* body
				* webpage text
				
* Command Line HTTP Request
	* curl -v
		* -v is a flag that means "be more verbose"
* Command Line act like server
	* Net Cat
	
		```nc -l 'server_num'```
		
* Coming soon!
	* Sinatra
		* Ruby framework where we can write simple responses to HTTP using Ruby
		
---
		
### URL

* a url is a way to retrieve something. 
	* doesn't have to be http, can also be ftp, etc. 
* http://www.google.com/search?term=cheese&lang=en#footer
		
		scheme://host/path/more_path?query-string#fragment
	1. scheme://
		* this is like a protocol
	2. google.com
		* host
	3. /search
		* path
		* will direct either to a file or resource
	4. ?term=cheese&lang=en
		* start with question mark (?)
		* these are hashes (some key equal a value)
			* ```{ term: 'cheese', lang: 'en' }```
		* this is how we pass in additional parameters to our request
		* this goes in the body
	5.  "#fragment"
* HTTP verbs are not part of the URL
	* POSTS and PUTS are part of forms
	* GETS should never change information on the server, but POSTing or PUTing can change info on the server. That's why it's warning you you're resubmitting a form. 
		* Submitting a form twice can give you a different result than submitting it once. 