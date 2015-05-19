# Git First Aid
## _git... commit... Oh Shit!_ 
#### (Because we've all been there)

# Helpful URLs 

* [http://41j.com/blog/2015/02/common-git-screwupsquestions-solutions/](http://41j.com/blog/2015/02/common-git-screwupsquestions-solutions/)
* [http://pcottle.github.io/learnGitBranching/](http://pcottle.github.io/learnGitBranching/)
* [Command Line Helpers](https://github.com/git/git/tree/master/contrib)
* [Official Documentation](https://git-scm.com/doc)

## Key <a id="key"></a>

* '$'  Indicates this line is a terminal command: $ git status

* '<some text>'  The angle brackets indicate a required field: $ git add <file>

* '[some text]'  The square brackets indicate an optional field: $ ls [-a]

* '(some note)'  The parenthesis indicate an author's note

* '.'  Reference to current working directory

* '..' Reference to back/up one directory


# A Few Basics

## Help Commands 

````
  $ git help <command>

  $ man git-<command>
````

_Note: hit 'q' to get out of the help page_

# Connecting with GitHub 

### To add your local project to your git profile (must first create it on GitHub)

## HTTPS
````````````````````````````````````````````````````````````````````````
$ git remote add origin https://github.com/<git profile>/<repo name>.git
$ git push [-u] origin master
````````````````````````````````````````````````````````````````````````

## SSH
````````````````````````````````````````````````````````````````````
$ git remote add origin git@github.com:<git profile>/<repo name>.git
$ git push [-u] origin master
````````````````````````````````````````````````````````````````````

_“-u” makes “$ git push” assume "origin master" by default_



# Cloning
## (Boba Fett)

To clone a repo form GitHub: (will create a directory of the repo name at '.')
```````````````````````````````````````````````````````````````````````````
  $ git clone <copy and paste URL from GitHub (there is a button for this)>
```````````````````````````````````````````````````````````````````````````


# The Good Stuff 

## Where Is This Commit?

Show a visual graph of your commit history relative to checked out branch
`````````````````
$ git log --graph
`````````````````

# Fix Last Commit Message #
###########################

Add/update info from stage to last commit and update message with 'new message'
`````````````````````````````````````
$ git commit --amend -m 'new message'
`````````````````````````````````````


# Reset

To un-stage a file you accidentally staged
``````````````````
$ git reset <file>
``````````````````
Undo the last commit, moving everything from that commit back into staging
````````````````````````
$ git reset --soft HEAD^
````````````````````````

Undo last commit and discard all its changes (they don’t go back to staging)
````````````````````````
$ git reset --hard HEAD^
````````````````````````

Undo last 2 commits and discard all changes (they don’t go back to staging)
````````````````````````
$ git reset --hard HEAD^^
````````````````````````

###################
# Reverting Files #
###################

Ever made a tone of changes but need to revert just one file?
  $ git checkout -- <file to revert>
    OR
If you need the file from 2 commits ago (and you are on master for example)
  $ git checkout master~2 <file to revert>


##################
# Removing Files #
##################

Remove from repository and add that change to the stage.
  $ git rm <file or folder name>

Stop tracking a previously tracked file/folders like node_modules or something
you should not be tracking. (You still have it in your local repo)
Add that file or folder to your .gitignore file
  $ git rm --cached <file or folder name>


############
# Checkout #
############

Checkout <branch name>
  $ git checkout <branch name>

discard current changes and checkout latest commit version of <file>
'--' Ensures that you don't checkout a branch by mistake
  $ git checkout -- <file>

'-b' creates the branch. Thus you can create and checkout all in one step
  $ git checkout -b <branch name>


##########
# Branch #
##########

Create a new branch called <name> (Also see $ git checkout -b <branch name>)
  $ git branch <name>

Log local branch name(s)
  $ git branch

Log remote branch names
May not log them all, if local data about remote is out of date
So using ($ git fetch) to update your data first
  $ git branch -r

Delete branch called <name> (fails if there is un-merged data)
  $ git branch -d <name>

Delete branch called <name> (Force Delete, even if un-merged data exists)
  $ git branch -D <name>


#########
# Merge #
#########

Merge <name> into currently checked out branch
eg. On branch master. $ git merge cat   (Merges cat into master)
  $ git merge <name>

For collaborative projects it can help to follow this work-flow:
- On branch master
- $ git checkout -b boss_plugin
- Make a boss plug-in with all its changes and files
- $ git add <All your plug-in stuff>
- $ git commit -m 'Add wicked awesome code'
- $ git checkout master
- $ git pull origin master
- $ git checkout boss_plugin
- $ git merge master
- Now if there is a merge conflict fix it.
    Regardless of conflict: Run all your tests again to be sure the latest
    version of master did not make any breaking changes.(fix if err)
- $ git push origin boss_plugin
- Go to origin repo online (probably GitHub) and make a 'pull request' from
    your boss_plugin branch to master
- Wait for super slow dev team to merge the awesomeness (Done) (or DIY)


##########
# Remote #
##########

Every needed to add another remote?
  $ git remote add <name> <address>
  E.x. $ git remote add origin git@github.com:<uber-dev>/<repo name>.git

List what repositories git knows about
  $ git remote -v

Remove <name> remote from project
  $ git remote rm <name>

Show tones of up to date data bout <repo name>
  $ git remote show <repo name>
  E.x.   $ git remote show origin

Clean up deleted remote branches… (comes in handy in group context)
Updates local repositories knowledge of remotely tracked branches
This is a command you will want to run from time to time on group projects.
  $ git remote prune origin


# Fetch

Update local repo with current info about remote without performing any merges
This ensures commands like ($ git branch -r) give up to date info
  $ git fetch



# Rebase  (Cue the ominous music)

Note: Rebasing can be a bit tricky. You checkout a secondary branch
e.g. bugfix and then rebase it onto master

Rebase commits:
e.x. On branch 'bugfix'. $ git rebase master (rebaseses master under bugfix)
Now checkout master and merge the freshly rebased 'bugfix' branch into master.

```
  $ git rebase <branch to rebase on to>
```

Interactively rebase last 3 commits

```
  $ git rebase -i <HEAD~3>
```
<<< Conflicts >>>
After fixing a conflict you continue the rebase

```
      $ git rebase --continue
```

Skip the conflict

```
      $ git rebase --skip
```

Abort the rebase and go back to just before you ran $ git rebase
```
      $ git rebase --abort
```


### REBASE DIAGRAM AND EXPLINATION:
REBASE STEP 1 takes place just before MERGE (master merge)
The (v#) and (bv#) are version info not branch names… helpful for this diagram
REBASE STEP 2: $ git checkout branch master      $ git merge bugfix
Because of the rebase, as far as git is concerned bug fix started at v3 not v1.

```
       MERGE               OR      REBASE STEP 1        |      REBASE STEP 2
master merge *              |                           |     bugfix v2 *
            / \             |                           |     bugfix v1 *
 master v3 *   * bv2 bugfix | master v3 *               |     master v3 *
           |   |            |           |  * bugfix bv2 |     master v2 *
        v2 *   * bv1        |        v2 *  * bugfix bv1 |     master v1 *
           |   |            |           |  * master v3  |     master v0 *
           \  /             |           |  * master v2  |
   master v1 *              | master v1 *--/            |
             |              |           |               |
          v0 *              |        v0 *               |
```
note the 7 commits on MERGE and the 6 total commits on REBASE STEP 2
Basically rebase makes it so you're branch is not based off of v1 where it
split off but is based off of v3 where it will be joining back in. e.g. Rebase.


# Rewriting History --->


```
$ man git-<command> (Use this to check the man page for more help)
```

Note: including the '-f' command helps to ensure that if the file or thing
searched for is not present in a given commit its absence will not result in a
critical fail and terminate the function.

Note: If you have to do this more than once you may need an extra '-f' in
there somewhere because git has a backup in the .git file and '-f' will force
it to overwrite it's backup with a new backup.

```
ex.
$ git filter-branch --index-filter 'git rm --cached --ignore-unmatch master_passwords.txt'
$ git filter-branch -f --index-filter 'git rm --cached --ignore-unmatch master_username.txt'
```

Rerun every commit on this branch applying the given shell command and
recommitting in order.
```
$ git filter-branch --tree-filter <shell command>
```

Example case: You committed your 'passwords.txt' file and that is a big nono.
You need to rewrite history so there is no trace of that file anywhere.
Here is what you do:
```
ex. '$ git filter-branch --tree-filter "rm -f passwords.txt"' 
```
Doing this checks every commit for passwords.txt and deletes it if found, then recommits.

To run the command on EVERY commit in the repo:
```
$ git filter-branch --tree-filter <shell command> -- --all
```
Specifying --HEAD instead of --all runs the command in only the current branch


Note: Commands used here must operate on the staging area, so mostly git commands.
  $ git filter-branch --index-filter <shell command>
  ex. $ git filter-branch --index-filter 'git rm --cached --ignore-unmatch passwords.txt'

Remove empty commits from history. Say you undid everything a given commit
added, it would just be empty and taking up space.
$ git filter-branch -f --prune-empty -- --all


# Extra Notes 

## git add 

Add everything from current directory and all sub-folders
```
$ git add .
```

Add <file name> and <file name>
```
$ git add <file name> <file name>
````

Add everything in the file css/
```
$ git add css/
```

# git diff

Show difference between last commit and unstated files:
```
$ git diff
```

Show difference between staged files and last commit:
```
$ git diff --staged
```

Get the diff of last commit. '^^' would go back one further and '^^^' one more after that:
```
$ git diff HEAD^
```

Get last 5 (or maybe 6) diff commit info
```
$ git diff HEAD~5
```

Get diff between commit < sha > and commit < sha >. Everything in-between inclusive.
```
$ git diff <sha> <sha>
```

# Tag

List tags. Can ($ git checkout < tag >) to checkout the commit with < tag >:
```
$ git tag
```

Tag the currently checked out commit.
```
      $ git tag -a <tag> -m <'message'>
E.x.  $ git tag -a v0.0.3 -m 'version 0.0.3'
```

Push tags to the remote repo. without this the tags just remain local:
```
$ git push --tags
```
  

#### "Well, they don't call it the 'Terminal' because it saves lives..."
