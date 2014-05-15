# Day 59 Notes | Week 12 Day 4

May 15, 2014

---

### Learning Objectives

* Explain what an algorithm is
* Benchmark code
* Explain what Big-O measures and why we use it
* Explain what a divide & conquer algorithm is

### What is an Algorithm

* Some process that acts on data
	* A series of steps that something can follow
* Two classes of objects that run algorithms
	* Humans
		* Making PB&J Sandwich
		* Building Ikea furniture 
	* Computers 
		* Anytime you write a method you're basically writing an algorithm
* The classic example of an algorithm:
	* Write an algorithm to make a peanut butter and jelly sandwich
	* To make it you have to break down the problem into simple steps
* Algorithms need to be well-defined for them to work effectively 

**Well-defined Algorithm**

* .sort
	* When you called ```.sort``` on an array Ruby implements a sorting algorithm
	* [Website to see sorting visually](http://www.sorting-algorithms.com/)
* There are many different way to define an algorithm to do something
* **Recursion**
	* As part of algorithm when you run the same algorithm on a sub-set of the data
	
### Big O

* An expectation of the worst-case scenario and how many operations that worst-case will take
* [Big O Complexity Chart](http://bigocheatsheet.com/)
* When you're computing the Big O, you compute from what's more costly for your computer to implement
	* Big O is an approximation. We care about orders of magnitude. 
		* We don't say n^2 + n because the small terms don't mean anything. We only care about n^2
* If something has a big o of 1:
	* That means no matter how many inputs we have it always takes the same amount of time (operations not time)
* O(n) - **Linear time complexity O(n)**
	* Adding one to a number
	* It gets more complex as you increase numbers you're adding together
		* 12 + 1 is one operation
		* 12345 + 11111 is five operations 
* **Logarithmic Complexity aka "n log n" O(log(n))**
	* Example: 
		* Looking someone up with the name "Charlie in the phone book"
		* Look for "C" by index, and if no index look in middle
* **Exponential Complexity O(n^2)**
	* Multiplying two three-digit numbers is nine operations
	* This doesn't scale linearly, it scales **exponentially** 
	* Bubble sort is one of these 
* **Polynomial Complexity O(2^n)**
	* Example
		* Cracking a password: taking a very large number and finding two numbers that multiply
* **Factorial Complexity O(n!)**
	* Example - **The traveling salesman problem**
		* Start someplace. You want to travel to every city and going home without going through the same city more than once. At every point in your algorithm you have to make a choice of where to go next. 
		* These are basically non-computable 

### Divide & Conquer Algorithm

* Steps
	* Can only be used on sorted sets
	* Pick a spot, often in the middle. Are you before or after where you want to be. You can then throw away half the problem. 
	* If you're after, look before and pick a spot in the middle of that
	* Continue until you zero in on what you want
	
### How to Benchmark Code in Ruby

* If you require Benchmark in Ruby you have a benchmark class

	```
	Bechmark.bmbm(10) do |bm|
	
	end
	```
	
* You can write a rake task that tests the benchmarking of your app
* Premature Optimization
	* As programmers, we have a tendency to want to optimize things. 
	* However, first write a simple solution and if it's not slow, don't optimize it
	
### Abstract Data Type

* See reading