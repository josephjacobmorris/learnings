# Git

## Introduction

## Commands

|      Command       | Usage | Description                                                                                                                                     |
|:------------------:|:----|:------------------------------------------------------------------------------------------------------------------------------------------------|
|  ```git config```  || Used to set/list various configs for gits                                                                                                       |
|                    |```git config --global user.name "<first-name>"```| Used to set username globally i.e. across all local repositories                                                                                |
|                    |```git config --global user.email "<mail-id>"```| Used to set user mail globally i.e. across all local repositories                                                                               |
|   ```git init```   | | Used to initialize a new git repository. It created the .git folder in the current working directory which is hidden by default                 |
|  ```git clone```   | ```git clone <url>```| Used to clone a git repository from the url and create a local copy  of the git repository                                                      |
|   ```git add```    | ```git add <filename>```| Used to add changes to the staging area                                                                                                         |
|  ```git commit```  | ```git commit -m "message"```| Used to commit the changes in the staging area as a bundle                                                                                      |
|  ```git fetch```   | ```git fetch <remote> <branch>```| Used to fetch branch from the remote repository                                                                                                 |
|  ```git merge```   | ```git merge <branch-name>```| used to merge the given branch to the current branch                                                                                            |
|   ```git pull```   | ```git pull <remote> <branch>```| pulls the changes from the given branch in the remote repository and merges it to the current branch i.e. ```git pull= git fetch + git merge``` |
|  ```git branch```  | | lists all the branches in the local repository                                                                                                  |
|                    |```git branch - a```| list all branches even the ones in the remote repositories                                                                                      |
|                    |```git branch --no-merged```| lists all branches that has not merged                                                                                                          |
|   ```git push```   |```git push <remote> <branch>```| pushes the commits from the local repository to the remote repository                                                                           |
|   ```git diff```   || To see the difference between your changes and the last commit                                                                                  |
|  ```git status```  || command would list out untracked, modified, and staged files                                                                                    |
|   ```git show```   || shows the log message and textual diff                                                                                                          |
|   ```git log```    || Shows the commit logs                                                                                                                           |
|  ```git reset```   || Resets the commit to the specified state                                                                                                        |
|    ```git rm```    || Remove files from the index, or from the working tree and the index                                                                             |
|  ```git remote```  || used to manage the remote repositories                                                                                                          |
|   ```git fsck```   || used to identify corrupted objects in the repository                                                                                            |
| ```git checkout``` || used to change branch or restore files                                                                                                          |
|  ```git stash```   || used to create/manage stash                                                                                                                     |
|  ```git bisect```  || used to find buggy commit between two commits using binary search                                                                               |


## References

* https://dev.to/devland/20-git-commands-that-will-make-you-a-version-control-pro-149p
* https://git-scm.com/docs/git-bisect