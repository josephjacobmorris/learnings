# Git

## Introduction

## Commands

|      Command       | Usage                                                       | Description                                                                                                                                     |
|:------------------:|:------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
|   ```git init```   |                                                            | Initializes a new Git repository. Creates a `.git` folder in the current directory.                                                             |
|  ```git config```  |                                                            | Sets Git configuration values, like user name and email.                                                                                        |
|                    | ```git config --global user.name "<name>"```               | Sets global username for all Git repositories.                                                                                                  |
|                    | ```git config --global user.email "<email>"```            | Sets global email for all Git repositories.                                                                                                     |
|  ```git clone```   | ```git clone <url>```                                      | Clones a remote repository to your local machine.                                                                                               |
|  ```git remote```  | ```git remote add origin <url>```                          | Adds a remote repository with the name `origin`.                                                                                                |
|                    | ```git remote -v```                                        | Shows the list of remote repositories and their URLs.                                                                                           |
|   ```git add```    | ```git add <filename>```                                   | Adds file changes to the staging area.                                                                                                          |
|                    | ```git add .```                                            | Adds all changes in the current directory to the staging area.                                                                                  |
|  ```git commit```  | ```git commit -m "message"```                              | Commits staged changes with a message.                                                                                                          |
|  ```git fetch```   | ```git fetch <remote> <branch>```                          | Fetches changes from a remote repository branch without merging.                                                                                |
|  ```git merge```   | ```git merge <branch-name>```                              | Merges a specified branch into the current branch.                                                                                              |
|   ```git pull```   | ```git pull <remote> <branch>```                           | Fetches and merges changes from a remote branch. (`git pull = git fetch + git merge`)                                                           |
|  ```git branch```  |                                                            | Lists all local branches.                                                                                                                       |
|                    | ```git branch -a```                                        | Lists all branches, both local and remote.                                                                                                      |
|                    | ```git branch --no-merged```                               | Lists branches that haven’t been merged into the current branch.                                                                                |
|                    | ```git branch <new-branch>```                              | Creates a new branch.                                                                                                                           |
|   ```git push```   | ```git push <remote> <branch>```                           | Pushes local branch changes to the specified remote branch.                                                                                     |
|                    | ```git push -u origin <branch>```                          | Pushes and sets upstream tracking for a branch.                                                                                                 |
|   ```git diff```   |                                                            | Shows the difference between working directory and the index (staged changes).                                                                 |
|                    | ```git diff --staged```                                    | Shows the difference between the index and last commit.                                                                                         |
|  ```git status```  |                                                            | Shows the status of changes (staged, unstaged, untracked files).                                                                                |
|   ```git show```   |                                                            | Shows details of a specific commit, including the diff.                                                                                         |
|   ```git log```    |                                                            | Shows commit history.                                                                                                                           |
|                    | ```git log --oneline```                                    | Displays a compact view of commit history.                                                                                                      |
|  ```git reset```   | ```git reset --hard <commit>```                            | Resets the repository to a specific commit, discarding all changes.                                                                             |
|                    | ```git reset --soft <commit>```                            | Resets to a commit but keeps changes staged.                                                                                                    |
|    ```git rm```    | ```git rm <file>```                                        | Removes a file from the working directory and the index.                                                                                        |
|   ```git fsck```   |                                                            | Verifies the integrity of the Git object database.                                                                                              |
| ```git checkout``` | ```git checkout <branch>```                                | Switches to a different branch.                                                                                                                 |
|                    | ```git checkout <file>```                                  | Discards changes in a file.                                                                                                                     |
|  ```git stash```   |                                                            | Temporarily saves changes that are not ready to be committed.                                                                                   |
|                    | ```git stash pop```                                        | Applies the last stashed changes and removes them from stash.                                                                                   |
|  ```git bisect```  |                                                            | Uses binary search to find the commit that introduced a bug.                                                                                    |
|                    | ```git bisect start```                                     | Begins a bisect session.                                                                                                                        |
|                    | ```git bisect good <commit>```                             | Marks a known good commit.                                                                                                                      |
|                    | ```git bisect bad <commit>```                              | Marks a known bad commit.                                                                                                                       |
|  ```git rebase```  | ```git rebase <branch>```                                  | Re-applies commits from the current branch on top of the specified branch.                                                                      |
|                    | ```git rebase -i <base>```                                 | Starts an interactive rebase to squash, reorder, or edit commits.                                                                               |
|  ```git revert```  | ```git revert <commit>```                                  | Creates a new commit that reverses the changes made by a specific commit.                                                                       |
|```git cherry-pick```| ```git cherry-pick <commit>```                            | Applies the changes from the specified commit onto the current branch.                                                                          |
|  ```git switch```  | ```git switch <branch>```                                  | Changes to another existing branch (modern alternative to `checkout`).                                                                          |
|                    | ```git switch -c <new-branch>```                           | Creates and switches to a new branch.                                                                                                           |
## References

* Chatgpt
* https://dev.to/devland/20-git-commands-that-will-make-you-a-version-control-pro-149p
* https://git-scm.com/docs/git-bisect
* https://blog.algomaster.io/p/top-20-git-commands?utm_source=share&utm_medium=android&r=40ln3j&triedRedirect=true
* https://blog.algomaster.io/p/top-20-git-commands?utm_source=share&utm_medium=android&r=40ln3j&triedRedirect=true