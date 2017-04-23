# Git Cheatsheet

This page contains a few helpful git commands that I've picked along the way whilst learning git on the command line.

## Helpful Commands
### Information

| Command | Description |
| --------| ----------- |
| git log --pretty=oneline | Shows logs on one line |
| git log --pretty=format:"%h %s" --graph | Shows logs as a graph |
| git log --oneline master..&lt;branch&gt; | Shows log of commits on a branch |
| git log --pretty='%C(red)%h %C(cyan)[%an (%cr)]%C(reset) %s' master..&lt;branch&gt; | Shows log of commits with colours on a branch from master |
| git cherry -v master | Shows log of commits on a branch from master |
| git remote | Shows remote connections + URLs |
| git status | Shows status of working repository |
| git branch | Lists local branches |
| git branch -a | Lists all branches |
| git branch -vla | Lists all branches |
| git diff | Diff unstaged files |
| git diff --cached | Diff staged files |
| git config --global -e | Edit git settings |

### Updating

| Command | Description |
| --------| ----------- |
| git fetch --all | Gets all new stuff and updates local repository |
| git fetch origin &lt;branch&gt; | Gets new stuff from branch and updates local repository |
| git pull | Gets new stuff from origin and merges it with working copy |
| git pull --rebase &lt;branch&gt; | Gets new stuff from branch and rebases working copy on it |

### Branching

| Command | Description |
| --------| ----------- |
| git branch &lt;branch&gt; | Creates a new branch |
| git checkout &lt;branch&gt; | Swaps to specified branch |
| git checkout -b &lt;branch&gt; | Creates and checkout a new branch |
| git branch -d &lt;branch&gt; | Deletes a branch if it has been merged into the current one |
| git branch -m &lt;newname&gt; | Renames the current branch |
| git push origin --delete &lt;branch_name&gt; | Deletes a remote branch |

### Adding / Comitting

| Command | Description |
| --------| ----------- |
| git add . | Recursively adds all files from the current directory to staging |
| git commit -m '&lt;message&gt;' | Commits file to staging with the specified message |
| git commit -am '&lt;message&gt;' | Adds files to staging and commits them with the specified message |
| git push | Pushes staged files to the remote origin |
| git push -f | Force pushes staged files **over** the remote origin |

### Clean Up

| Command | Description |
| --------| ----------- |
| git reset | Removes files from staging |
| git reset --hard | Reverts all staged files |
| git reset --soft HEAD^ | Removes staged but un-pushed changes |
| git clean -xdf | Removes all untracked/ignored files and directories |
| git checkout &lt;file&gt; | Reverts a single file |
| git revert &lt;commit&gt; | Reverts to a previous commit. If you want to revert multiple commits you have to do it one commit at a time |
| git revert -n &lt;commit&gt; | Reverts to a previous commit but does not automatically commit it |
| git push -f origin &lt;last_good_commit&gt;:&lt;branch&gt; | Changes back to the selected last good commit on a branch |

### Stashing

| Command | Description |
| --------| ----------- |
| git stash list | Displays all stashes |
| git stash | Stashes staged files |
| git stash apply stash@{'&lt;n&gt;'} | Applies a single stash |
| git stash drop stash@{'&lt;n&gt;'} | Drops a single stash |
| git stash pop | Applies and drops the latest stash |

### Tagging

| Command | Description |
| --------| ----------- |
| git tag -l | Lists all tags |
| git tag -am '&lt;message&gt;' | Creates a tag with a message |
| git push --tags | Pushes tags to the remote |
| git tag -d &lt;tag name&gt; | Delete a local tag |
| git fetch --tags | Fetch remote tags |

### Git Submodules

| Command | Description | Notes |
| --------| ----------- | ----- |
| git submodule add &lt;Suri&gt; &lt;Spath&gt; | Creates submodule at the set path | Make sure the path in .gitmodules contains / instead of \\ |
| git rm --cached &lt;path&gt; | Removes a submodule directory | Use this command before reinitialising the submodule |
| git submodule update --init | Initialises a new submodule | |
| git submodule sync | Syncs a submodule with the remote | |
| git submodule update --remote &lt;path&gt; | Pulls in the changes from a remote submodule to the local working copy | |


## Helpful Aliases

| Command | Alias | Description |
| ------- | ----- | ----------- |
| git hist | git config --global alias.hist 'log --pretty=format:\"%C(auto)%h %ad \| %s%d [%an]\" --graph --date=short --color' | Shows history of the repository |
| git last | git config --global alias.last 'log -1 HEAD' | See the last commit |
| git dump &lt;file&gt; | git config --global alias.dump 'cat-file -p' | Print out a file |
| git alias | git config --global alias.alias '!git config -l \| grep alias \| cut -c 7-' | List aliases |
| git bl master..HEAD | git config --global alias.bl "log --no-merges --pretty='%C(red)%h %C(cyan)[%an (%cr)]%C(reset) %s'" | Branch log |
| git blm develop..HEAD | git config --global alias.blm "log --pretty='%C(red)%h %C(cyan)[%an (%cr)]%C(reset) %s'" | Branch log with merges |
| git usm | !git config --file .gitmodules --get-regexp path \| awk '{ git submodule update --remote $2 }' | Updates all submodules |

## Useful Flows
### Squash commits in a branch
1. Find a list of commit you want to squash
    * git log --pretty='%C(red)%h %C(cyan)[%an (%cr)]%C(reset) %s' master..&lt;branch&gt;
    * git cherry -v master
2. it rebase -i HEAD~&lt;n&gt;
    * &lt;n&gt; is the number of commits you want to squash together (Count number of commits from previous step)
3. 'pick' or 'reword' the 1st line and change all the rest to 'fixup'
4. git push -f

### Preparing for pull request
1. Update master branch
    * git fetch --all
2. Squash all commits in your branch
3. Push your squashed commits
    * git push -f
4. Rebase your feature branch on master
    * git rebase master
5. Fix conflicts (if any)
    * git rebase --continue
6. Push your changes
    * git push

### Clean up after pull request
1. git checkout master
2. git fetch --all -p
3. git branch -d &lt;merged_feature_branch&gt;
