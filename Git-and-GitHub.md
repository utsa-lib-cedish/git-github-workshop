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

## Your Git repo and the Git index

Git works with *repositories*. Git users generally refer to a repository as a *repo*. To illustrate Git workflows I will be using a JetBrains IDE, WebStorm. The interface is similar to other JetBrains IDEs or to VSCode. The commands are the same whether or not one uses an IDE or a command line terminal.

When you start a project in JetBrains IDEs and add a new file, the IDE will ask you if you want to add the file to Git. Check "Don't ask again" and click cancel. You should handle Git interactions manually, at least while you are learning.

![A WebStorm window asking if the user wants to commit a file to Git](images/first-repo-1.png)

Open a terminal window.

![A WebStorm window showing how to open the Terminal](images/first-repo-2-terminal.png)

Enter the command `git status`. By default, JetBrains IDEs usually create a Git repo for you automatically whenever you create a new project. If this was the case, you should see an output from `git status`. Otherwise you will see an error message like `fatal: not a git repository (or any of the parent directories): .git`. This message tells you that a Git repository has not been created yet. If that is the case, enter `git init` to start your repository. Then, enter `git status`.

After the git repo is initialized and I have added some files, my `git status` command will show me the *index*. 

Working with Git is a three part process. You work on your files in your working directory. Then you *stage* your changes in the index. Finally, you *commit* your changes to the repository. The index keeps track of what you want to commit. 

There are three kinds of files in Git: tracked, ignored, and untracked. This is reflected in the output of `git status`:

![A WebStorm window showing the output of a git status command ](images/first-repo-3-git-status.png)

In green you can see there is a new file called `.idea/.gitignore`. This file is ready to commit. It is being tracked.

In red we have "Untracked files". These include several files in the `.idea` folder as well as our `index.html` file.

The `.idea` folder is where JetBrains IDEs keep their user preferences and project settings. These settings are not part of the application development process. They are usually specific to the user. If you commit them to the repository, when you share your work with other developers, they will get your preferences and settings and these may override theirs. For this reason, we do not commit the `.idea` folder to the repo. Instead, we want to *ignore* it.

It looks like our IDE automatically staged a file for us when the repo was initialized. This not desirable. The repo should only contain what we want it to contain. This is an example of why it is always important to examine the state of the Git index before committing, so you are sure that you are committing everything you want and only what you want.

Let's fix these problems before proceeding. First, we need to ignore the `.idea` folder and all its contents. To do this, we need to create a special file called `.gitignore`. We create this in our project's *root* directory.

![A WebStorm window showing a gitignore file](images/first-repo-4-gitignore.png)

Notice we have put `.idea` in the gitignore file. This tells Git to ignore the directory and all its contents. But wait ... the IDE already staged a file inside the directory. And that file is also called `.gitignore`. What is going on here?

This is one of the more confusing aspects of working with JetBrains IDEs. There is a `.idea` folder, and inside it there is a `.idea/.gitignore` file. But this gitignore belongs to the IDE and its settings. Since it is inside the `.idea` folder, it *only* affects the contents of that folder. It is *not* the same as your project gitignore. 

In fact, you should also personally ignore the contents of the `.idea` folder and treat it as a black box. Don't confuse that gitignore with your project gitignore.

Since the IDE already staged its own `.idea/.gitignore` file, and we don't want that, we are going to *unstage* it. To unstage a file prior to committing, run `git rm --cached <name-of-file>`. In this case, we run `git rm --cached .idea/.gitignore`. Do this for any file you have accidentally staged, or that you staged but then you realized you do don't want to commit it.

![A WebStorm window showing a git rm --cached command and the output of a git status command](images/first-repo-5-unstage.png)

Notice here we remove the `.idea/.gitignore` file from the index. Then we run `git status` again. Now, the index reflects our own work and only our own work. There are no files showing in green with the message "Ready to commit". This means our index is empty. There are two files that are untracked. Git sees that these files have been modified in our working directory but have not been staged. 

Our next step is to stage the files. We can do this by naming the files individually as arguments to a `git add` command: `git add .gitignore index.html`. It is more typical to add all the unstaged files at once with the `git add -A` command. But sometimes you don't want to stage everything, so it is important to also be able to add them individually as needed. 

![A WebStorm window showing a git add command and the output of a git status command](images/first-repo-6-stage.png)

Here we can see that after staging our files, our .gitignore and index.html files are ready to be committed.

Best practice when working with Git is to stage files after you've modified them enough that you may want to save them. That doesn't mean committing immediately. You may want to work on several files before committing. You may commit right after staging, or you may work on a file, stage it, work on another file, stage it, realize you want to work on the first file again, stage the changes, and then commit. 

Staging a file copies it to the Git object store, so when time comes to commit, Git doesn't have to do the work of creating copies of your file contents. It's one of the many ways Git ensures that it is quick and efficient.

Remember: always check the state of your index before staging and committing. Run `git status` before running `git add`. 

## Commits

A commit is a snapshot of the index at a given moment. It records the changes made to a repository since the previous commit. A commit is the only way to introduce changes to the repository, and each commit carries a record of who made it, thus enforcing accountability and ensuring that the history of the project is recoverable through the logs.

To commit your changes to the repo, use the `git commit -m` command. The -m option stands for "message". Each commit must carry a commit message describing what the changes are. Optionally, more information can be added if you enter the interactive text editor.

![A WebStorm window showing a git commit command](images/first-repo-7-commit.png)

You now have a commit. You can see the commit history with the `git log` command. 

![A WebStorm window showing a git log command](images/first-repo-8-git-log.png)

The Git log command shows the commit history ordered from the latest commit backward. In Git, each commit knows its parent commit and that's it. This particular commit is special because it's the *root* commit so it has no parent.

The output of the `git log` command shows the *SHA1 hash* of the commit object. This is a unique identifier. Git takes SHA1 hashes of everything in its repo object stores. You generally reference a commit by its hash. You can usually use just the first few characters of the hash. After the hash, it shows the commit author, the commit date, and the first line of the commit message, which in this case is the complete commit message.

Let's keep developing now. Let's say I add some more content to my web page.

![A WebStorm window showing new content in an html file and a new git status, git add, and git commit sequence](images/first-repo-9-second-commit.png)

After making some changes to the web page, I am satisified with my work and I want to commit it to the repo again. How much work to do before committing is a matter of personal preference and team policy. The general rule of thumb is to commit when you've got a significant change ready, or when you've done enough work that you would not like to lose. Or, if you realize the project is in a stable condition and you are about to make some potentially hazardous or experimental changes. When you make a commit, it's like a save point and you can always recover that state at any time.

As always, check your index first by running `git status`. This shows me that the only untracked changes are my changes to `index.html`. I will stage these. Notice that I use the `git add -A` command. This means "add all untracked changes to the index". Then I make my second commit.

Now when I run `git log` again, I can see both commits. 

![A WebStorm window showing the output of a git log command](images/first-repo-10-second-git-log.png)

From now on my workflow will involve checking the index, staging changes, and committing my work to the repo.

# GitHub

If we want to work collaboratively in a team, we need to share our repo. The simplest and most common way to do that is through GitHub. GitHub was started originally to facilitate collaboration on open source projects, but it was so useful that private companies started using it for their own collaborative development projects.

## GitHub credentials

To log in to GitHub you just need an account with a password, although 2FA is always good to have. However, to do a *push* (a repo upload or update), GitHub prefers the SSH protocol, so you need to have an SSH key and share the public key with GitHub.

### SSH keys on a Mac

On a Mac, open a Terminal window. You can click on the magnifying glass on the top right of the menu bar to open the Mac Spotlight feature. Type Terminal to get to a Terminal icon and double click it or hit the return key to open it.

Your Terminal instance will look different because I have customized my terminal's appearance and I use bash instead of the default zshell program.

![A Mac desktop showing the cursor over the magnifying glass and a search box with the word "Terminal" typed in](images/github-1-terminal.png)

Enter the command `cd ~/.ssh` to navigate to your SSH directory. Then enter `ls -a` to see if you have any key pairs. You would see something like `id_rsa` and `id_rsa.pub` or `id_ed25519` and `id_ed25519.pub`. If you do not see any key pairs you will need to generate one by entering `ssh-keygen -t ed25519 -C "your_email@example.com"`.

GitHub prefers EdDSA keys (with the ed25519 signature) but RSA will also work. 

Copy your *public* key signature to your clipboard. Use this command: `pbcopy < ~/.ssh/id_ed25519.pub`. 

![A Mac Terminal window showing the commands to change directory to the ssh key directory, list the contents, and copy the contents of an ssh key to the clipboard](images/github-2-ssh-2a.png)

### SSH keys on a Windows

On a Windows system, open a PowerShell. Select "Run as Administrator". 

![A Windows desktop showing a search for Powershell with the option to open Powershell as administrator on the right](images/github-3-ssh-3.png)

Navigate to your home directory with the commmand `cd ~`. Now check if you have an SSH directory using the `Get-ChildItem -Hidden` command.

![A Windows Powershell open with the commands issued to navigate to the home folder and to show hidden files](images/github-3-ssh-4.png)

If there is no `.ssh` folder, create it by typing `mkdir .ssh`. Now you are ready to create your SSH key. Type `ssh-keygen -o -a 100 -t ed25519 -f $env:USERPROFILE\.ssh\id_ed25519 -C`. When prompted to enter a passphrase, just hit return. You will now have a key pair in a folder called `.ssh` in your home directory.

![A Windows Powershell open with the commands to create a .ssh directory and to generate an EdDSA key pair](images/github-3-ssh-5.png)

To open the file in a text editor, type `notepad ~\.ssh\id_ed25519.pub`. Make **sure** you add the `.pub` ending so you are copying the public key, not the private key. Then type Ctl+A to select the contents of the file and Ctl+C to copy the contents to your clipboard.

### Add the SSH key to your GitHub account

Now that you have generated an SSH key, you need to add it to your GitHub account. First navigate to your Account Settings. 

![A browser window showing the GitHub website with the cursor hovering over the Account Settings link](images/github-2-ssh-2.png)

Then, select "SSH and GPG keys".

![A browser window showing the GitHub website with the cursor hovering over the SSH and GPG keys link](images/github-3-ssh-6.png)

Click on "New SSH key".

![A browser window showing the GitHub website with the cursor hovering over the SSH and GPG keys link](images/github-3-ssh-7.png)

Name your SSH key. SSH keys are specific to devices, so I usually name mine with the name of the device and its purpose. If you use a different computer, this SSH key will not work. Then paste the contents of the SSH public key into the box labeled "Key".

![A browser window showing the GitHub website with the cursor hovering over the SSH and GPG keys link](images/github-3-ssh-8.png)

## Creating a GitHub repo

You are now ready to set up your repo on GitHub. Click on the plus sign and select "New repository".

![A browser window showing the GitHub website with the cursor hovering over the SSH and GPG keys link](images/github-4-repo-1.png)

Give the repository a name. Decide whether you want the code to be public (generally for open source projects or for your portfolio projects that you want people to know about), or private. Leave the README off for now, we'll add this later if we decide we need it.

![A browser window showing the GitHub website with Create a new repository dialog open](images/github-4-repo-2.png)

Once you create your repo, you will see some windows with some code. These are instructions for how to connect your local and remote repos. We already have an existing local repository, so we are interested in the lower set, to "push an existing repository from the command line".

![A browser window showing the GitHub website with instructions to connect local and remote repos](images/github-4-repo-3.png)

## Setting your upstream remote

Now we will go back to our IDE. Copy the **first line** of the instructions. It should start with `git remote add`.

We use the `git remote` command to control various aspects of our remote repo configuration. Under the hood, the `git remote` command alters the configuration file contents. Here we are going to add a reference to a remote repository. 

The network address uses a format specific to Git, called a Git URL. It will look something like `git@github.com:jruedas1/awesome-site.git`. Since this is really long and hard to remember, Git implements aliases. Our command `git remote add origin git@github.com:jruedas1/awesome-site.git` means to add a reference to `git@github.com:jruedas1/awesome-site.git` with the alias `origin`. Origin is the default and traditional alias for the repository that a team pulls from and pushes to.

![A WebStorm window showing a git remote add command, a git branch command, and a git push command](images/github-5-remote-1.png)

After adding the remote reference, we want to make sure that our default branch is named `main`. Some versions of Git will name the default branch `master`. GitHub prefers the term `main`. Use the `git branch` command to see the name of the branch. If it is `master`, you should rename it to avoid conflicts with GitHub's default naming. Use the command `git branch -M main` to ensure the default branch is called `main`. 

## Pushing to a GitHub repo

Finally, we are ready to *push* our code to GitHub. The first time we push, we use the command `git push -u origin main`. This means we want to push the `main` branch to the remote referenced by the alias `origin`. The `-u` argument means to set this push branch/remote combination as the default "upstream". From now on, `origin main` will be the default configuration, so we only need to enter `git push`. 

Submit the command, and you should see output showing the transfer of Git's object store to the remote repository. 

![A WebStorm window showing the output from a git push command](images/github-5-remote-2.png)

Go back to GitHub and refresh the page. You should now see the repository contents. These are the two files you've committed so far, the `.gitignore` and the `index.html`. 

![A browser window showing a GitHub repository view with two files](images/github-5-remote-3.png)

## Cloning a repo

The next step in collaboration is for someone else to get a copy of your repo. This operation is called *cloning*. The clone operation is very simple with JetBrains IDEs and GitHub. We will also see how to clone a repo on the command line, which is very straightforward.

Navigate to the GitHub repo you want to clone. Click on the green "Code" button. 

![A browser window showing a GitHub repo and the cursor hovering over a Green button that reads "Clone"](images/clone-1.png)

Ensure that the "SSH" option is selected, **not** HTTPS or GitHub CLI. Although Git does allow for HTTPS operations, GitHub strongly prefers SSH and you may not be able to clone over HTTPS.

![A browser window showing a GitHub repo and the cursor hovering over the SSH option for a repository URL](images/clone-2.png)

Copy the Git URL to your clipboard.

![A browser window showing a GitHub repo and the cursor hovering over a copy icon with the text "Copy URL to clipboard" showing](images/clone-3.png)

### Cloning with a JetBrains IDE

Open your IDE. Click on "Clone Repository". If it opens to your last project, just close that project to get to this view.

![A WebStorm window showing the option to clone a repository](images/clone-4.png)

Click on the "Browse" button to select the destination folder for your project.

![A WebStorm window showing an open file browser dialog. The option to create a new folder has been selected and the new folder is being named](images/clone-5.png)

Once you've found where you want your project, create a new folder to hold the files. You can name it the same as the repo or give it a custom name.

![A WebStorm window showing an open file browser dialog. The option to create a new folder has been selected and the new folder is being named](images/clone-7.png)

Once you have the folder created, click "Open".

![A WebStorm window showing an open file browser dialog. The cursor is hovering over the "Open" button](images/clone-8.png)

You're ready to clone the repo now. Paste the copied Git URL in the "URL" form field and click "Clone".

![A WebStorm window showing the Clone Repository dialog. The URL and Directory fields have been filled and the cursor hovers over a blue button that reads "Clone"](images/clone-9.png)

You'll be asked if you trust the project or prefer to previews in safe mode. In this case, you are collaborating with a known small team on a web project, so it is safe to trust the project.

![A WebStorm window showing a confirm dialog asking the user if they want to trust and open a project](images/clone-10.png)

You are now the proud owner of a copy of the original repository. Note that this means not only the files, but also the commit history.

![A WebStorm window showing a a project open with two files, and displaying the content of an html file. The terminal is open showing two commits as the output of a git log commanbd](images/clone-11.png)

## Feature branches for team projects

## Merging branches

## Handling merge conflicts







## Pushing a branch to GitHub

## The pull request

## Pulling the updated code