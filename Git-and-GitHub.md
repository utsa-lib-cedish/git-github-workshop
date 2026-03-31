# Remote collaboration with Git and GitHub

## Git

Git is the world's foremost Version Control System (VCS). This is a tool that tracks different versions of a code base as a project undergoes development. It keeps track of everyone's work, who is doing what, and what is or isn't included in the main project. It is at the same time a repository of file states, a project history, a tool for teamwork on complex products, and a backup system enabling recovery of past states if something goes wrong.

Git was invented by Linus Torvalds, the inventor of Linux, because he couldn't find a tool to enable development of Linux. Linux is an open-source project with thousands of people contributing from all around the world. Finding nothing suitable to enable that sort of work, Torvalds made his own tool. Some of the characteristics of Git are:

1. Facilitates distributed development: Allows for multiple developers in different locations to work in parallel without requiring constant network connections.
2. Scales to handle large numbers of developers: Linux has thousands of developers making millions of contributions.
3. Quick and efficient.
4. Enforces accountability. Keeps track of who changed files and why.
5. Maintains integrity and trust. Tracks files by their SHA1 hash, so any changes to files are immediately detected.
6. Supports and encourages branched development. Branches allow different developers and teams to work on their own features and eventually merge the features back into the project.
7. Free. You might eventually pay for hosting Git repos used by large teams, or for useful web-based graphical interfaces to manage Git repos, but Git itself is completely free.

## Your Git repo

Git works with *repositories*. Git users generally refer to a repository as a *repo*. To illustrate Git workflows I will be using a JetBrains IDE, WebStorm. The interface is similar to other JetBrains IDEs or to VSCode. The commands are the same whether or not one uses an IDE or a command line terminal.

When you start a project in JetBrains IDEs and add a new file, the IDE will ask you if you want to add the file to Git. Check "Don't ask again" and click cancel. You should handle Git interactions manually, at least while you are learning.

![A WebStorm window asking if the user wants to commit a file to Git](images/first-repo-1.png)

## Commits

## Feature branches for team projects

## Merging branches

## Handling merge conflicts

## GitHub credentials

## Setting your upstream remote

## Pushing main to GitHub

## Pushing a branch to GitHub

## The pull request

## Pulling the updated code