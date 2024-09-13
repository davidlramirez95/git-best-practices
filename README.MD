# Git Best Practices

Here are some of my notes working with git for several years.


## Git tips
* Use git aliases for frequently used commands  
* Git stash to save our work before switching to other branches  
* Don't forget to pop the stash to avoid stale stashes   
* Use `git blame`` to trace the author — useful for communicating the changes  
* Take note that `git revert` is useful if you have small changes but disadvantageous if dealing with squashed commit because it reverses all of your changes 
* Use this shortcut to auto push your local branch to remote:
`git config --global push.autoSetupRemote true`


## Git Workflow Strategy
This discusses different approaches to merging our feature branch to main.

* ## git merge

### PROs
* Pulls in the latest changes to main. 

### CONS
* It creates an extra commit on the feature branch which makes it untidy.
* It's for every merge we tie a  knot in a rope, if we do this a lot, it can make the history a bit messy.


* ## git rebase and fast forward merge
 
### PROS
* It gives us a clean, straightforward commit history, if the main history is linear. It's like straigtening out of a rope in one line.

### CONS
* Complicated when dealing with diverged branches.

* A pain to resolve when encountering a lot of commits with merge conflicts.


* ## git squash

### PROS
* Feature branch commits are squeezed into a single commit when merged to main. This keeps main history linear and tidy.

### CONS
* We lose the fine details of the individual feature in the main branch's commit history.


## PR issues:
* Determine the severity of the change  
    * Determine if its a small refactor or a huge rearchitect of the code and negotiate witht the timeline
* Deal with late review comments
    * Ask the reviewer if its optional or a mandatory change to determine whether to apply it or not
    * The reviewer could also tell the PR owner in advance on the change
* making sure we are in the same loop of the github workflow  
* Taking action on the PR action items  
    * Communicate with the reviewer if necessary  
    * Ask questions if the review in unclear  


## Git intermediate commands

* git reflog
* git graph
* git bisect  

Scenario:   

If you're not sure, pick a really old commit. You can even pick the first commit. This isn't as absurd as it sounds. Binary searches are very efficient; 10,000 commits can be searched in just log2(10000) tries or 13 tries.

For example, let's say your commit history looks like this.

a - b - c - d - e - f - g - h - i [main]
You know the feature is broken now, you don't know when it was good, but you do know it was introduced at d. Pick c, the commit immediately before the feature was introduced, as the "good" commit.

$ git bisect start
$ git bisect bad
$ git bisect good c
```
                               HEAD
a - b - c - d - e - f - g - h - i [main]
        g                       b
```
Then git bisect will do a binary search of d - e - f - g - h until it finds a bad commit immediately after a good one.

In this case it will start with f. Let's say that's bad.

$ `git bisect bad`
```
                   HEAD
a - b - c - d - e - f - g - h - i [main]
        g           b           b
```

Git assumes everything after f is also bad. Then it might try d. Let's say that's good.

$ `git bisect good`

```
           HEAD
a - b - c - d - e - f - g - h - i [main]
        g   g       b           b
```

Then it will try e. Let's say that's bad.

$ `git bisect bad`

```
               HEAD
a - b - c - d - e - f - g - h - i [main]
        g   g   b   b           b
```
e broke the feature.[1]



## Navigating Git Graph


## Git Aliases vs alias

The difference between Git alias vs `alias` is the former is using Git configuration whereas the latter uses the system command (if your OS supports it).


### Git alias

To create your first simple alias, just try:
`git config --global alias.st status`

This will add a new line to the [alias] section of your global config file and create the section if it isn't already there. Let's have a look:

```
$ cat ~/.gitconfig
[user]
   name = Jan Krag
   email = jan.krag@example.com
[alias]
   st = status
```

### Alias

alias is a system command available on MacOS, to run the available alias git commands, execute: `alias` and find some cool git commands.


References:  
[1] Git Bisect Scenario example, https://stackoverflow.com/questions/72738822/how-to-perform-a-git-bisect-if-i-dont-know-which-old-commit-is-good