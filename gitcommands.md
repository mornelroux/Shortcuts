
# Git Commands
### Used in Youtube 3 part series  

## Part one - Basic Git Commands

    git init                                                        #Start a git repository/directory, it generates a .git file with History and Staged records
    git config --global user.name "Morne Le Roux"                   #Configures the git repository with user for all git direcotories on this PC
    git config --global user.email "lerouxmorne7@gmail.com"         #Configures the git repository with email for all git on host PC
    git config --list                                               #Prints the configurations for current git repository
    git status                                                      #Command to view current changes to fils, staging area -> Basically all that is to be committed
    git add s1.sh                                                   #Adds file "s1.sh" to staging area, with commit message
    git commit -m "add file s1"                                     #Commits files in staging area to a brand new commit
    git log                                                         #Prints all commits made
    git diff                                                        #Shows the differences between files outside staging area and previous commit (I think)
    git add .                                                       #Adds all files in current directory to staging area
    git diff --staged                                               #Shows the differences between files inside staging area and the previous commit
    git commit -m "Add s2.sh and edit S1.sh"                        #Commits current staging area, with short commit message or decsription
    git rm s2.sh                                                    #Removes files from directory, and logs the removal of the file (same as "rm" and "git add <fileRemoved>" together)
    git commit                                                      #Commits staging area, but opens a textfile for commit comments instead of the "-m" short message
    git checkout -- s1.sh                                           #Restores file to previous commit version
    git reset HEAD s1.sh                                            #Removes file from staged area
    git log -- s2.sh                                                #Logs all commits made for file s2.sh
    git checkout 9ddb9 -- s2.sh                                     #Restores files from commit starting with 9ddb9
    vim .gitignore

## Part two - Branching and merging

    git log --all --decorate --oneline --graph                      #Short summary of commits
    alias graph="git log --all --decorate --oneline --graph"        #Creates a shorthand for the above command
    git branch SDN                                                  #Creates a branch with the name 'SDN'
    git branch                                                      #Shows all the branches
    git branch -a							#Shows all the available branches
    git checkout SDN                                                #Moves the symobolic pointer 'HEAD' to branch 'SDN'
    git commit -a -m "auth for S1"                                  #Stages and commits all changes made in branch
    git checkout master                                             #Moves the HEAD to the master branch
    git diff master..SDN                                            #Shows what will change when we merge SDN to master
    git merge SDN                                                   #Merges SDN branch with the current branch (normally to master) - Fast forward merge
    git branch --merged						#Shows which branches were merges with current branch
    git branch -d SDN						#Deletes the branch 'SDN'
    git checkout -d dev						#Creates and moves into a new branch called 'dev'
    git merge --abort						#Aborts merge process when conflicts occured
    git checkout 99d9903						#HEAD moves to a previous commit (Detached head)
    git branch staged						#Creates a branch where the detached HEAD with the name 'staged' (Note, git still in a detached head state)
    git checkout staged						#Git no longer in detached head state, HEAD pointer points to new branch called 'staged'
    git stash							#When uncommited were made in a branch, this will save the uncommited changes so we can switch to another branch
    git stash list							#Shows a list of staches in current branch
    git stash list -p						#Shows the changes in each stash
    git stash apply							#Git will reapply the most recent stash
    git stash pop							#Git will reapply the most recent stash and remove the stash from the stash list
    git stash apply stash@{1}					#Git will reapply the stash with the list number 1
    git stash save "Stash descriptive message"			#Saves the stash with a message

## Part three - Remotes

Origin is the default alias for the remote repo address (URL)

    git clone 							#Retrieves the remote repo, copy and downloads the remote repo
    git fetch							#Fetches updates from the remote repo
    git push 							#Upload updates to the remote repo
    git clone git@github.com:mornelroux/netauto.git		
    git config --local user.name "Betty"
    git config --local user.email "lerouxmorne7@gmail.com"
    git remote							#Displays remotes
    git remote -v							#Displays full locations
    git fetch origin						#Fetches commits to origin in remote repo
    git merge origin/master						#Merges changes from remote repo with current main branch in local repo
    git pull							#Combines the fetch and merge commands 
    git push origin master						#Pushing our changes to our remote named origin to the master branch at origin




