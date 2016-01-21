[back to overwiev](/../..)

#Git
```Ruby
😭
```
##### Table of Contents  
[Mostly used](#mostly-used)  
[Getting Help](#getting-help)  
[Setup](#setup)  
[Staging](#staging)  
[Commiting](#commiting)  
[Branching](#branching)  
[Teamwork](#teamwork)  
[Removing](#removing)  
[Ignoring](#ignoring)
[Renaming](#renaming)  

##Mostly used
**git init** –– to initialize  
**git status** –– to check what’s in the folder  
**git add filename1 filename2** –– to add files to git  
git add . –– adds all files  
**git diff filename** –– to see the differences –– q to quit  
**git commit -m “text”** –– to commit the changes  
**git log** –– to see all commits  
**git branch -a** –– show’s all branches and highlight the one you’re on  
**git checkout branchname** –– to switch to a branch  
**git branch branch-name** –– create a branch  
**git reset 5d69206** –– the 7 first numbers of the commit you want to reset  

In Git, the commit you are on is known as the HEAD commit. In many cases, the most recently made commit is the HEAD commit. To see the HEAD commit, enter: $git show head

Git has three main states that your files can be in: committed, modified, and staged

**Modified**: file was changed but not commited yet.  
**Staged**: file marked in its current version to go in next commit.  
**Committed**: data safely stored in local database.  

**Working directory** – single checkout of one version of the project  
**Staging area (also Index)** – stores data about what to be commited  
**Git directory (Repository)** – main backup place  

The basic Git workflow goes something like this:  
You modify files in your working directory.  
You stage the files, adding snapshots of them to your staging area.  
You do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to your Git directory.  

##Getting Help
####Within git
git help &lt;verb&gt;  
git &lt;verb&gt; --help  
man git-&lt;verb&gt;  
####outside of git
in the #git or #github channel on the Freenode IRC server (irc.freenode.net)

git status –– full status  
git status -s –– simplified status (?? = new files not tracked yet A = staged M = modified MM = modified, staged and then modified again so both staged and unstaged)  
git diff –– compares staged with not staged  
git diff --staged –– compares staged with commited  
(--staged and --cached are synonyms)  

##Setup
####Global
git config --global user.name "John Doe"  
git config --global user.email johndoe@example.com  
git config --list –– to show all settings  
####Project
git init –– adds git  
####New
git add .  
git commit -m 'initial project version'  
git remote add origin URL  
git push -u origin master  
####Existing
git clone URL (directory name, optional)  

##Staging
**git add filename** –– stages the file exactly how it is at this moment, for every change this command has to be run again to stage the new version.

##Commiting
**git commit** –– commits with basic info + large message  
git commit -v –– commits with changes + message  
git commit -m –– commits basic info + message inline  
git commit -a –– commits basic info + message & git add all files  

##Branching
**git branch** –– show’s the branch you’re on  
git branch branch-name –– create a branch  
git branch -d branch-name –– delete the branch  
**git checkout branch-name** –– switches to the specified branch  
**git merge branch-name** –– merges the actual branch with the branch specified  

##Teamwork
**git clone remote-location clone-name** –– clones a remote git project. Location of original + a name for your directory. The original will get the name origin  
**git remote -v** –– Lists a Git project's remotes.  
**git fetch** –– gets any possible changes and stores them into a branch called origin/master  
**git merge origin/master** –– merges the fetched stuff with your local stuff  
**git push origin your-branch-name** –– pushes your branch to the origin where it can be reviewed and added.  
**git rebase develop** –– merge the develop branch with your branch  
**git pull** –– Get latest Data from the actual branch  
git pull --rebase remote-name branch-name –– http://gitready.com/advanced/2009/02/11/pull-with-rebase.html  
**git branch -a** –– Show all available branches  
**git checkout branchname** –– Switch to a branch

####Teamwork Workflow:
Fetch and merge changes from the remote  
Create a branch to work on a new project feature  
Develop the feature on your branch and commit your work  
(Fetch and merge from the remote again (in case new commits were made while you were working))  
Push your branch up to the remote for review  
> Make Pull Request (WIP)  
> Merge

##Removing
**git rm** –– remove file  
git rm -f –– to remove it when it’s already in the index  
git rm --cached –– keep on hard drive but remove from being tracked  
git rm log&sol;&bsol;&ast;.log –– removes all files that have the .log extension in the log/ directory  
git rm &bsol;&ast;~ –– removes all files that end with ~  
**git mv file-from file-to** –– to rename a file  
**git branch -d branch-name** –– delete the branch  

##Ignoring
Create a .gitignore file and write in that file:  
&ast;.[oa] –– ignore any files ending in “.o” or “.a”  
&ast;~ –– ignore all files that end with a tilde (~) which is used by many text editors such as Emacs to mark temporary files  
####Rules
Blank lines or lines starting with # are ignored.  
Standard glob patterns work.  
You can start patterns with a forward slash / to avoid recursivity.  
You can end patterns with a forward slash / to specify a directory.  
You can negate a pattern by starting it with an exclamation point !.  
&ast; matches zero or more characters  
[abc] matches any character inside the brackets (in this case a, b, or c)  
? matches a single character   
[0-9] matches any character between them (in this case 0 through 9)  
a/&ast;&ast;/z would match a/z, a/b/z, a/b/c/z, and so on.  
Example  
&ast;.a –– no .a files  
!lib.a –– but do track lib.a, even though you're ignoring .a files above  
/TODO –– only ignore the TODO file in the current directory, not subdir/TODO  
build/ –– ignore all files in the build/ directory  
doc/&ast;.txt –– ignore doc/notes.txt, but not doc/server/arch.txt  
doc/&ast;&ast;/&ast;.pdf –– ignore all .pdf files in the doc/ directory  
Templates: https://github.com/github/gitignore

##Renaming
**git mv file-from file-to**  
Is the equivalent to:  
mv README.md README  
git rm README.md  
git add README