# Day 45 Notes | Week 9 Day 5

April 25, 2014

---

### Resolving Git Merge Conflicts

* Changing different lines at the same time
	* rebase
		* it's ok you can stick adam's changes on top of trinity's changes even though adam's changes come after trinity's
		* ```git rebase origin master```
	* git pull
		* this is actually a merge (short for git fetch and git merge)
		* git will apply adam's changes to where they came from then apply a new commit with both as parent
* Changing the *same* line at the same line
	* git pull will give you auto-merge conflict
	* do **git status**
		* will tell you which file is conflicted
	* open it and you will see a file with ```<<<<<HEAD```, this is where your merge conflict is
		* fix the conflict
	* git status
	* git add file you did fix on
	* git status again
		* you should see changes should be committed
	* git commit with msg about fixing merge conflict
	
### How to Delete a Branch
	
```
git branch -d test_branch
```

* Still need to delete on GitHub
	* Go to branches and click "view merged branches". 
		* See which branches have been merged into others (and will have no new commits), then click *delete branch*

### Set Up Branches for Project 2

* We need 3 branches:
	* master
	* development
	* features
* Create development branch
	
	```
	git checkout -b development
	git push
	```
* At this point dev and master are the same
	* Never make commits to master or dev, only merge into them
* Make feature branch

	```
	git checkout -b secondary_memes
	git push
	```
	
	* Make new files on this secondary_memes branch, then commit and push it.
* When you're done with feature branch merge back to development
	* go on to branch you want to merge changes on to, then choose branches to merge into that branch
	
		```
		git checkout development
		git merge secondary_memes
		```
		
### Git Stash

* Put all your changes away so you can make another commit without committing what you were working on
* Then ```git stash apply``` and it'll take all your changes and pop them back on
* Great for fixing small things like typos without having them related to the other files you're working on

### Git Tsar

* Team members switch off git tsar daily
* Make sure development is stable
	* Work with teammates to fix if not stable
		* should generally be
* Checkout master, then merge development into master
	* do this once a day
* Why are we doing this
	* practice because this is how dev teams usually do this
	* anytime during the project someone can push project to heroku so instructors can see our app live
		* may be a day behind, but good indicator of progress
	
### What Should Scrum Meetings Cover (Project 2)

* Go over every git commit so everyone knows what's going on

### Turbo Links

* What is it
	* Used to be a Ruby Gem, but put into Rails 4
	* It injects JS into your app
		* Hijacks every link click to *your* app, and instead of it being handled by the browser, it turns every link click and form submission into an AJAX request behind the scenes
		* AJAX request refreshes the page and just replaces entire body of document instead of refreshing page
	* Only refreshes body, not head content
	* Advantage
		* Don't have to reload head
* Point of this
	* Performance Benefits for free
* Only breaks one thing on app
	* Doesn't rerun document.ready on each new page
		* Event is only triggered when page is loaded
	* Fix
		1. Disable Turbolinks
		2. New document JS command
		
			```
			// Totally refresh
			$(document).ready(readyFunction);
			
			// on page load, special event that turbo links triggers
			$(document).on('page:load', readyFunction); 
			```