##Preface
This is intended to be a brief introduction to `git`.  
Every git command has it's own man page in the format of `git-<command>` (e.g. `man git-add`).  
You can always google for some much more detailed tutorials.

##What is git
> *git - the stupid content tracker*
> 
> "git" can mean anything, depending on your mood.  
 - random three-letter combination that is pronounceable, and not
   actually used by any common UNIX command.  The fact that it is a
   mispronunciation of "get" may or may not be relevant.
 - stupid. contemptible and despicable. simple. Take your pick from the
   dictionary of slang.
 - "global information tracker": you're in a good mood, and it actually
   works for you. Angels sing, and a light suddenly fills the room.
 - "goddamn idiotic truckload of sh*t": when it breaks
> 
> *from https://github.com/git/git#readme*

A distributed revision control system first developed by Linus Torvalds to manage the Linux kernel's source code.  
Now a widely used tool to manage software projects.  

###Pros
* Fast and scalable.
* Distributed: Easy to collaboration with others and development on local machine.
* Versioning: Rollback to an earlier working version if you've messed up the code.
* Branches: Smart workflow.
* Stupid content tracker: Even monkeys know how to use it.

###Cons
* You have to learn it first.

##Install git
###If you're using Ubuntu
```bash
$ sudo add-apt-repository ppa:git-core/ppa
$ sudo apt-get update
$ sudo apt-get install git
```
###If you're using CSIE workstation
It's already installed.

###If you're using Windows
Please don't use Windows.

##Github
A popular git repo hosting site. Many open source projects are hosted here.  
~~Social network for geeks~~  
Anyone can host *unlimited* number of git repos for *free* as long as the repo is public.  
However, you can get the student pack (https://education.github.com/pack) and you get 5 *free* *private* repos and a bunch of coupons.  
This tutorial is also hosted on github: https://github.com/silverneko/git-tutorial  


Bitbucket is another git hosting site and it offers free *private* repos, 
but it has limitation on number of collaborators.

##How to use git
Lines starting with a dollar sign are shell commands.  
Options quoted with square brackets means it's optional.  
Options quoted with angle brackets means it's a must.  

###git config
```bash
$ git config [--global] user.name  <INSERT NAME HERE>
$ git config [--global] user.email <INSERT EMAIL HERE>
$ git config [--global] push.default simple
$ git config [--global] core.editor <INSERT YOUR FAVORITE TEXT EDITOR HERE>
```
Set/query git configs.  
Drop the `--global` option if you only want to change the config for current repo.

###git init
```bash
$ git init [directory]
```
Creates a git repository. Make a directory into a git repository.  
If `path name` is empty then it will init in the current directory.

###git status
```bash
$ git status
```
Show the status.

###git add
```bash
$ git add <file | directory>
$ git add .
$ git add --all
```
Stage the changes.  
The first command adds files/directories into staging area.  
The second command stages **everything** in the working tree. **USE WITH CAUTION!!!**  
The third command stages everything and do `git rm` on removed files for you.

###git rm
```bash
$ git rm [--cached] <file>
$ git rm [--cached] -r <directory>
```
Delete files/directories from git's tracking list and working tree.  
Add `--cached` to remove *only* from the tracking list. (i.e. Untrack a file but don't delete it from working tree)

###git mv
```bash
$ git mv <file> <path>
```
It kinds of explains itself already.

###git commit
```bash
$ git commit [-a] [-m <message>]
$ git commit --amend
```
Commit the staged changes.  
`-a` Automatically stages **tracked files** before commiting.  
`-m <message>` Use `<message>` as the commit message.  
`--amend` Replace the latest commit by a new commit. We often use this to fix typos in commit message. **USE WITH CAUTION!!!** *CHANGING THE HISTORY IS DANGEROUS, MAKE SURE YOU KNOW WHAT YOU'RE DOING.*  

###git reset
```bash
$ git reset [file]
$ git reset [--soft|--mixed|--hard|...] <commit>
```
The first command removes files from staging area. (i.e. the opposite of `git add`)  
The second command rewrites commits like `git commit --amend`, read more in `man git-reset`. **BE EXTRA CAUTION AS YOU ARE REWRITING HISTORY (AGAIN)**  

###git show
```bash
$ git show
```
Shows many things. Try it yourself.

###git log
```bash
$ git log [--all] [--stat] [--decorate] [--graph] [--pretty=<oneline|short|...>]
```
Show the commit log.  
There are a lot of options for this command, read more in `man git-log`.

###git diff
```bash
$ git diff [commit1] [commit2]
```
If no commits provided, show the diff of the working tree and the current branch. (i.e. show uncommit changes of current branch)  
If only `[commit1]` is provided, show the diff of the working tree and `[commit1]`.  
If `[commit1]` and `[commit2]` are provided, show the diff of the two commits.

###git branch
```bash
$ git branch [-v]
```
List all branches.  
Add `-v` for verbose.

```bash
$ git branch [-u <upstream>] <branchname>
```
Create new branch `<branchname>`.  
Add `-u` to also set it's upstream to `<upstream>`.

###git checkout
```bash
$ git checkout <branch|commit>
```
Change current branch or checkout a commit.

###git merge
```bash
$ git merge <branchname>
```
Merge current branch with branch `<branchname>`.

###git clone
```bash
$ git clone <repository> [directory]
```
Clone repository into given directory.    
If `[directory]` is not given then git will try to make a new directory and the directory name is determined by `<repository>`.

###git pull
```bash
$ git pull [remote] [branch]
```
Pull remote changes and apply it to the current branch.  
If `[remote]` and `[branch]` are omitted, git will pull from current branch's upstream.

###git push
```bash
$ git push [remote] [branch]
```
Push local changes to remote branch.  
If you've set `push.default` to `simple` then `[remote]` and `[branch]` can be omitted and it'll push to current branch's upstream.

###git stash
```bash
$ git stash [--all]
```
Stash uncommited changes and turn working tree *clean*.  
Use `--all` to stash *untracked* files, too.

```bash
$ git stash pop
```
Restore stashed.

We often use `git stash` before `git pull` because `git pull` requires a clean working tree.
A clean working tree can be obtained by doing a `git commit`, but sometimes the changes on working tree are too minor to commit.
In such cases we can do something like:
```bash
$ git stash --all          # stash the dirty tree
$ git pull origin master   # get new changes from your co-workers
$ git stash pop            # restore your working tree and continue working
```

###git blame
```bash
$ git blame <file>
```
Show what revision and author last modified each line of a file.

###git tag
```bash
$ git tag -a <tag name> [-m <message>] [commit]
$ git tag -d <tag name>
```
Tag a commit.
`-d` to delete a tag.

##Common workflow
```
 1.need new feature so open a feature branch
  |              3.merge the completed feature back to master
  v               v
--+---+---+---+---+---+---+ master branch
   \             /
    \-+---+---+-/  feature branch
      ^ 2.work on the new feature on a seperate branch
```
1. Open a new `feature` branch.
2. Work on the new feature on `feature`, commit, testing, commit, debug, loop ...
3. When the feature is complete, merge `feature` to `master`.
4. Delete `feature` branch. (Optional)

This way your not-yet-done work won't mess up with other collaborator's work, because everyone is working on their own branch and only working code would be merge back into `master`.

##References
- Github's interactive git tutorial: https://try.github.io/levels/1/challenges/1
- Git's official documents, a bit prettier than man pages: http://git-scm.com/doc
- Github's markdown guide, teachs you how to write in markdown: https://guides.github.com/features/mastering-markdown/

