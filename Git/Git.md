# Git

## Introduction

## Commands

|Command | Usage | Description |
|:-------:|:----|:-------------|
|```git config```|| Used to set various configs for gits|
||```git config --global user.name "<first-name>"```| Used to set username globally i.e. across all local repositories|
||```git config --global user.email "<mail-id>"```| Used to set user mail globally i.e. across all local repositories|
|```git init```| |Used to initialize a new git repository. It created the .git folder in the current working directory which is hidden by default|
|```git clone```| ```git clone <url>```| Used to clone a git repository from the url and create a local copy  of the git repository|
|```git add```| ```git add <filename>```| Used to add changes to the staging area|
|```git commit```| ```git commit -m "message"```| Used to commit the changes in the staging area as a bundle |
|```git fetch```| ```git fetch <remote> <branch>```| Used to fetch branch from the remote repository|
|```git merge```| ```git merge <branch-name>```| used to merge the given branch to the current branch|
|```git pull```| ```git pull <remote> <branch>```| pulls the changes from the given branch in the remote repository and merges it to the current branch i.e. ```git pull= git fetch + git merge```|