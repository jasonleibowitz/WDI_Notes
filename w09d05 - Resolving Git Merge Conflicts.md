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