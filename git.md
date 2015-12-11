git
===

##  Basics

### git init
Create a new local repository

### git status / st
Check local repository status

### git diff
Show changes in the working directory since last commit
* --staged : changes in the staging aera

### git add {path}
Add {path} file(s) to the staging area  
* -p {file} : Add only selected fragments of {file}  

### git reset
Reset the stage to last commit  
* {file} : remove {file} from the staging area  
* -p {file} : remove selected fragments of {file} from the stage  
* --hard : reset the stage and the working area to last commit  
* --soft HEAD^ : cancel last commit and get commited files back to the staging area  
* --hard HEAD^ : cancel last commit and changes to commited files  
* --hard HEAD^^ : last commit and previous one  

### git checkout -- {file}
Undo changes to {file} since last commit

### git commit -m "Message"
Create a commit (snapshot) of staged files with a message
* --amend : add the current staging area to last commit
* --amend --no-edit : do it without changing the commit message
* --amend -m : change the last commit' message

### git rm {file}
Delete {file} and completely remove it from the index
* -r : recursively delete a directory
* --cached : only remove from the index and keep file

### git show
Show an object (blob, tree, tags and commit)
* {remote}/{branch}:{file} : Show {file} from {branch} in {remote}
* HEAD^:{file} : Show {file} snapshot from last commit

### git rebase
Replay commits
* master : replay commits from master to current branch
* -i : interactive mode

### git revert {hash}
Undo changes of {hash} commit and create a new commit
* -n : do it without committing again

## Remote

### git clone {url}
Create a local depot by cloning remote depot {url}

### git remote
Manage remote depositories
* add {name} {url} : add depot {url} as remote with name {name}
* rm {name} : delete remote {name}

### git pull {remote} {branch}
Get new commits from distant repository {remote} in {branch} and merge them into the current branch

### git fetch {remote} {branch}
Get new commits from distant repository {remote} in {branch} but do not merge them automatically

### git push {remote} {branch}
Send local commits from {branch} to distant repository {remote}
* -u : save {remote} and {branch} as default

## Stash

### git stash save -u "{message}"
Save the working directory in the stash without commiting with {message}

## git stash list
Show all stashes created

### git stash pop
Get back the stashed working directory

### git stash drop
Remove the latest stash
* stash@{x} Remove stash number x

## Branches

### git branch
Show all existing branches
* -r : show only distant repository branches
* {branch} : create branch with name {branch}

### git checkout {branch}
Go to branch {branch}
* -b : go to branch and create it if necessary
* --theirs : when fixing pull conflict, keep remote files
* --ours : when fixing pull conflict, keep local files

### git merge {branch}
Merge {branch} in the current branch

### git branch -d {branch}
Delete branch {branch}

### git cherry-pick {commit}
Get {commit} from another branch and replay it on current branch 

## Tags

### git tag
Show all tags
* -a {tag} : create a new tag named {tag}
* -a {tag} {commit} : tag a specific commit
* -m "{message}" : add a message with the new tag
* -d {tag} : remove a tag

## Log

### git log / lg
Show log
* `--author=‘{author}’`: filter by author
* `--grep=‘{message}'`: filter by commit message
* `-10`: show last 10 commits
* `origin/master..master`: show commits that will be pushed
* `-- {file}`: show commits affecting {file}
* `-S "{string}"`: show commits where modified lines match {string}
* `-p`: display diff for each commit

### `git blame {file}`
Show a file with last modifying commit for each line

### better git blame with git log
* `git lg -S ".article-title" -p file.html`

## Config

### git config --global
Set global variable
* alias.{alias} {command} : créer un alias {alias} pour la commande {command}
* user.name {name} : nom de l'utilisateur 
* user.email {email} : adresse de l'utilisateur

### My config

git config --global user.name "Clement Bourgoin"
git config --global user.email "cb@nokto.net"
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.lg "log --graph --pretty=tformat:'%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%an %ar)%Creset'" -10
