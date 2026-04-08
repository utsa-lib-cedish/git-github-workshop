# Remote collaboration with Git and GitHub

## Contents

- [Git](#git)
- [Install](#install)
- [Commits](#commits)
- [GitHub](#github)
- [GitHub Credentials](#github-credentials)
- [SSH Keys on a Mac](#ssh-keys-on-a-mac)
- [SSH Keys on Windows](#ssh-keys-on-windows)
- [Add the SSH Key to Your GitHub Account](#add-the-ssh-key-to-your-github-account)
- [Creating a GitHub Repo](#creating-a-github-repo)
- [Setting Your Upstream Remote](#setting-your-upstream-remote)
- [Pushing to a GitHub Repo](#pushing-to-a-github-repo)
- [Cloning a Repo](#cloning-a-repo)
- [Cloning With a JetBrains IDE](#cloning-with-a-jetbrains-ide)
- [Cloning on the Command Line](#cloning-on-the-command-line)
- [Feature Branches](#feature-branches)
- [Merging Branches](#merging-branches)
- [Handling Merge Conflicts](#handling-merge-conflicts)
- [Pulling from GitHub](#pulling-from-github)

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

## Install

In MacOS, Git should come pre-installed. You can test by opening the Terminal application and entering `git --version`. If you get a version number, it's installed. If you get a message telling you the command is not recognized, it's not installed. In that case, install [XCode Developer Tools](https://developer.apple.com/xcode). After installation, restart your Terminal and test again. Git should now be installed. 

In Windows, you can install directly from the [Git website](https://git-scm.com/install/windows). After downloading, double-click on the `.exe` file. When asked "Do you want to allow this app to make changes to your device?" click on "Yes". After this, the install dialogue will open and ask a bunch of questions. You can accept the defaults in most cases. There are a couple of places where you may want to change the defaults.

The first option you may want to change is the default text editor. Git will often open up text for you to read in a text editor, or ask you to enter some information in a text editor. By default Git uses a text editor called vim. Vim is very popular in among Linux users and developers, and Git's history is closely connected to the history of Linux. Many people like vim. However, for beginners who are not familiar with the command line, it can be challenging. If you already like vim, are you are familiar with the command line and enjoy learning old-school Linux stuff, or if you just want the authentic Git experience, then just accept the default. If you prefer a slightly more user-friendly and modern experience, you can now select a different editor. By default, Windows systems come with a program called Notepad that is a bit easier to use. You can always change this preference later in the Git config file.

![A Git install dialogue in Windows asking the user to select the default text editor. Notepad is selected as the default editor](images/install-windows-8.png)

A change you should make is to the default branch name for new repositories. By default this is set to "master" but years ago, the GitHub website decided its default name should be "main". Since we will be using GitHub, as many people do, it is simpler to go along with GitHub's default and name our default branch "main". 

![A Git install dialogue in Windows asking the user if they want to override the default branch name for new repositories. The radio button to do so is selected and the default branch name set to "main"](images/install-windows-10.png)

For the rest of the install process you can just leave the default settings and click on "Next" until the process is complete. When it's done installing, you can uncheck the box labeled "View Release Notes" unless you really, really want to view the release notes. Then click "Finish".

![A Git install dialogue in Windows that reads "Completing the Git Setup Wizard". The cursor hovers over a box labeled "View Release Notes". The box is unchecked.](images/install-windows-20.png)

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

## GitHub

If we want to work collaboratively in a team, we need to share our repo. The simplest and most common way to do that is through GitHub. GitHub was started originally to facilitate collaboration on open source projects, but it was so useful that private companies started using it for their own collaborative development projects.

### GitHub credentials

To log in to GitHub you just need an account with a password, although 2FA is always good to have. However, to do a *push* (a repo upload or update), GitHub prefers the SSH protocol, so you need to have an SSH key and share the public key with GitHub.

#### SSH keys on a Mac

On a Mac, open a Terminal window. You can click on the magnifying glass on the top right of the menu bar to open the Mac Spotlight feature. Type Terminal to get to a Terminal icon and double click it or hit the return key to open it.

Your Terminal instance will look different because I have customized my terminal's appearance and I use bash instead of the default zshell program.

![A Mac desktop showing the cursor over the magnifying glass and a search box with the word "Terminal" typed in](images/github-1-terminal.png)

Enter the command `cd ~/.ssh` to navigate to your SSH directory. Then enter `ls -a` to see if you have any key pairs. You would see something like `id_rsa` and `id_rsa.pub` or `id_ed25519` and `id_ed25519.pub`. If you do not see any key pairs you will need to generate one by entering `ssh-keygen -t ed25519 -C "your_email@example.com"`.

GitHub prefers EdDSA keys (with the ed25519 signature) but RSA will also work. 

Copy your *public* key signature to your clipboard. Use this command: `pbcopy < ~/.ssh/id_ed25519.pub`. 

![A Mac Terminal window showing the commands to change directory to the ssh key directory, list the contents, and copy the contents of an ssh key to the clipboard](images/github-2-ssh-2a.png)

#### SSH keys on Windows

On a Windows system, open a PowerShell. Select "Run as Administrator". 

![A Windows desktop showing a search for Powershell with the option to open Powershell as administrator on the right](images/github-3-ssh-3.png)

Navigate to your home directory with the commmand `cd ~`. Now check if you have an SSH directory using the `Get-ChildItem -Hidden` command.

![A Windows Powershell open with the commands issued to navigate to the home folder and to show hidden files](images/github-3-ssh-4.png)

If there is no `.ssh` folder, create it by typing `mkdir .ssh`. Now you are ready to create your SSH key. Type `ssh-keygen -o -a 100 -t ed25519 -f $env:USERPROFILE\.ssh\id_ed25519 -C`. When prompted to enter a passphrase, just hit return. You will now have a key pair in a folder called `.ssh` in your home directory.

![A Windows Powershell open with the commands to create a .ssh directory and to generate an EdDSA key pair](images/github-3-ssh-5.png)

To open the file in a text editor, type `notepad ~\.ssh\id_ed25519.pub`. Make **sure** you add the `.pub` ending so you are copying the public key, not the private key. Then type Ctl+A to select the contents of the file and Ctl+C to copy the contents to your clipboard.

In order to use your ssh key with GitHub on a Windows system, you must add it to your ssh-agent. To do this, make sure you opened your PowerShell by clicking "Run As Adminstrator" as indicated above. Enter the following commands:

`Get-Service -Name ssh-agent | Set-Service -StartupType Manual`

`Start-Service ssh-agent`

Then enter the command `ssh-add` followed by the path to the ssh key you just created. This is a path to the *private* key, the one that does **not** have the `.pub` file ending. It should look something like `C:\Users\your-user-name\.ssh\id_ed25519`.

![A Windows Powershell open with the commands to start the ssh-agent and add a key to it](images/github-3-ssh-10.png)

#### Add the SSH key to your GitHub account

Now that you have generated an SSH key, you need to add it to your GitHub account. First navigate to your Account Settings. 

![A browser window showing the GitHub website with the cursor hovering over the Account Settings link](images/github-2-ssh-2.png)

Then, select "SSH and GPG keys".

![A browser window showing the GitHub website with the cursor hovering over the SSH and GPG keys link](images/github-3-ssh-6.png)

Click on "New SSH key".

![A browser window showing the GitHub website with the cursor hovering over the SSH and GPG keys link](images/github-3-ssh-7.png)

Name your SSH key. SSH keys are specific to devices, so I usually name mine with the name of the device and its purpose. If you use a different computer, this SSH key will not work. Then paste the contents of the SSH public key into the box labeled "Key".

![A browser window showing the GitHub website with the cursor hovering over the SSH and GPG keys link](images/github-3-ssh-8.png)

### Creating a GitHub repo

You are now ready to set up your repo on GitHub. Click on the plus sign and select "New repository".

![A browser window showing the GitHub website with the cursor hovering over the SSH and GPG keys link](images/github-4-repo-1.png)

Give the repository a name. Decide whether you want the code to be public (generally for open source projects or for your portfolio projects that you want people to know about), or private. Leave the README off for now, we'll add this later if we decide we need it.

![A browser window showing the GitHub website with Create a new repository dialog open](images/github-4-repo-2.png)

Once you create your repo, you will see some windows with some code. These are instructions for how to connect your local and remote repos. We already have an existing local repository, so we are interested in the lower set, to "push an existing repository from the command line".

![A browser window showing the GitHub website with instructions to connect local and remote repos](images/github-4-repo-3.png)

### Setting your upstream remote

Now we will go back to our IDE. Copy the **first line** of the instructions. It should start with `git remote add`.

We use the `git remote` command to control various aspects of our remote repo configuration. Under the hood, the `git remote` command alters the configuration file contents. Here we are going to add a reference to a remote repository. 

The network address uses a format specific to Git, called a Git URL. It will look something like `git@github.com:jruedas1/awesome-site.git`. Since this is really long and hard to remember, Git implements aliases. Our command `git remote add origin git@github.com:jruedas1/awesome-site.git` means to add a reference to `git@github.com:jruedas1/awesome-site.git` with the alias `origin`. Origin is the default and traditional alias for the repository that a team pulls from and pushes to.

![A WebStorm window showing a git remote add command, a git branch command, and a git push command](images/github-5-remote-1.png)

After adding the remote reference, we want to make sure that our default branch is named `main`. Some versions of Git will name the default branch `master`. GitHub prefers the term `main`. Use the `git branch` command to see the name of the branch. If it is `master`, you should rename it to avoid conflicts with GitHub's default naming. Use the command `git branch -M main` to ensure the default branch is called `main`. 

### Pushing to a GitHub repo

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

![A WebStorm window showing a project open with two files, and displaying the content of an html file. The terminal is open showing two commits as the output of a git log commanbd](images/clone-11.png)

### Cloning on the command line

Cloning on the command line is even simpler. The CLI clone operation automatically creates a folder for you with the same name as the repository.

![A Mac Terminal window showing the commands to change directory to the Sites directory, then the output of a git clone command, and a command to list the contents of the newly created directory](images/clone-12.png)

Here, I navigated to the Sites directory, then just typed `git clone` and the Git URL for the repo I wanted to clone. I don't have to create the `awesome-site` directory, the clone operation will create it for me. After the objects have been transferred to my system, I can see that the directory has been created and it contains three things: an `index.html` file, a `.gitignore` file, and the `.git` directory.

Note that this would not work if I already had a directory called `awesome-site` at this location in my file system. 

Now I can open Webstorm and, instead of "New project" or "Clone repository", select "Open". 

![A WebStorm window showing a cursor hovering over the option to open a project](images/clone-12b.png)

Navigate to the location of the newly cloned repo in your file system.

![A WebStorm window showing a file browser dialogue. The cursor hovers over the "Open" button](images/clone-14.png)

You can open the project and you have access to all the files as well as the commit history.

![A WebStorm window showing a project open with two files, and displaying the content of an html file. The terminal is open showing two commits as the output of a git log commanbd](images/clone-15.png)

## Feature branches

Git allows you to launch a separate line of development without affecting the main project state. When you create a *branch* you split the project into two separate lines of development. The main branch continues to exist on its own, but you can now do your work in a *feature* or *topic* branch. This allows multiple team members to work on different features or aspects of the project without colliding with each other. When a feature is ready to test, you can then *merge* it back into the main line.

Although branches are most often used in team settings, they are also very useful when working on individual projects. This is because you can test things out and experiment without affecting the stable main line of the project. If you mess up or things don't work, you can just go back to the main branch and start over. Because of this, whenver you have a project in a solid, stable, state and you are about to start a new feature that could potentially introduce bugs, it is best practice to create a branch and work in the branch.

Let's go back to your website. Let's say I now want to start styling my project. I can create a branch called `style`. To do this I can create the branch and then switch to it, but the most common procedure is to create a branch and switch to it at the same time. The command is `git switch -c style`. The `-c` argument means "create". This means "create a branch called *style* and switch to it.

If for some reason you want to create a branch but not switch to it and stay in the branch you are in, just do `git branch name-of-branch`. Then you can switch branches with the `git switch name-of-branch` command.

![A WebStorm window showing a project open with two files, displaying the content of an html file. The terminal is open showing the output of a git switch -c command: "Switched to a new branch 'style'"](images/branching-1.png)

Once I'm in my new branch I can start working. I create a new file and start styling my project. I create a stylesheet, add some CSS rules, and link the style sheet in my HTML file. Then I check my `git status`.

![A WebStorm window showing a project open with two files open, displaying the content of one html file and one css file. The terminal is open showing the output of a git status command](images/branching-2.png)

Notice one file is completely untracked. That is the new CSS file I just added. The HTML file is already in the index, but `git status` tells me it has been modified and the changes have not been staged. I can stage all the changes with a `git add -A` command. Then I make a commit on this branch.

![A WebStorm window with the terminal showing the output of git add, git commit, and git status commands](images/branching-3.png)

At this point, the commit history can be visualized as follows:

![A diagram showing two circles arranged one above the other connected by a line. From the lower circle a line goes diagonally right and down to a third circle. Each circle is labeled with a hash and a commit description](images/branching-4.png)

I can continue working in my style branch. I'll now add some header style and add some more structure to enable more complex styles. Then I can add and commit again.

![A WebStorm window with the terminal showing the output of git add, git commit, and git status commands](images/branching-5.png)

At this point, the commit history can be visualized as adding more commits on the style branch:

![A diagram showing two circles arranged one above the other connected by a line. From the lower circle a line goes diagonally right and down to a third circle, which has a line going straight down to a fourth circle. Each circle is labeled with a hash and a commit description](images/branching-6.png)

Now, let's say I am not feeling like this is a successful strategy. Maybe I want to start over again with a CSS framework like Bootstrap or Material Design, or maybe I want to stop working on style to start bringing in some interactivity. I can just switch back to the main branch.

![A WebStorm window with the terminal showing the output of git switch command](images/branching-7.png)

Notice, after switching back to main, the contents of my working directory are set back to the way they were before I created my style branch. There is no style sheet file, and my `index.html` file does not have the changes I made with the more complex structure.

## Merging branches

If I am satisfied that my changes are solid and are ready to bring into the main line of project development, I can *merge* them. To do this I switch to the main branch -- as I just did -- and just issue the command `git merge name-of-branch`. 

![A WebStorm window with the terminal showing the output of git switch, git branch, and git merge commands](images/branching-8.png)

Here, I use `git branch` to list the names of the branches so I can make sure to get the next command right. Then I say `git merge style`. This takes the `style` branch and *merges* it into the branch I am currently on. The output of this command states "Fast-forward" -- this is the *merge strategy* that Git used to combine the contents of the branches.

In this case, the entire history of the current (main) branch was contained in the history of the target (style) branch. To make the contents match, all Git had to do was *fast forward* the main branch to where the style branch is. This is the simplest type of merge for Git. If we had done more work in the main branch before merging, it would be more complicated.

We can visualize the commit history like this:

![A diagram showing three circles arranged one above the other connected by lines. From the second circle a second line goes diagonally right and down to a fourth circle on the side, which has a line going straight down to a fifth circle. From the fifth circle on the side branch, a line leads to the third circle in the main trunk. Each circle is labeled with a hash and a commit description](images/branching-9.png)

At this point, the last commit is in both the main and the style branches, and the commit history of the style branch has been incorporated into the main branch.

## Handling merge conflicts

From time to time, Git cannot automatically determine how to carry out a merge. This most often happens when a file has been altered differently in multiple branches, and in particular when the same line of a file is altered in a different way in different branches.

Let's say I continue working in the main branch. Keep in mind, this is a bad idea -- once you start working in feature branches, you should only work in feature branches, never directly in the main branch. But this problem could just as easily occur if I work in a separate "content" feature branch and it is simpler to demonstrate this way.

In the main branch, I add a second type of content, and I add links to the content sources. Then I stage my changes and commit. 

![A WebStorm window with the index.html page showing new edits, and the terminal showing the output of git status, git add, and git commit commands](images/merge-conflict-1.png)

Now I'm going to switch to my style branch. Notice that after switching, the new additions to my `index.html` page are not there. Those edits exist only in the main branch, not in the style branch. 

![A WebStorm window with the terminal showing the output of git switch command and the contents of index.html reset to a different state from the main branch](images/merge-conflict-2.png)

I decide I want each content item to have some sort of style to separate it from other content items, so I create a `section` element wrapping the `h2` and `p` elements. This is going to be trouble because there is already a second content item in the main branch, and Git will not know how to merge the two files automatically.

![A WebStorm window showing edits made to the index.html file in the style branch that are different from those in the main branch](images/merge-conflict-3.png)

Now I add my CSS rules for the `section` element. Satisfied, I stage and commit.

![A WebStorm window showing edits made to the style.css file and a terminal open showing the output of git add and git commit commands](images/merge-conflict-4.png)

Thinking it's time to merge my styles, I switch back to the main branch. Notice the `style.css` file content is reset to what it was before I created the section styles.

![A WebStorm window with the terminal showing the output of git switch command and the contents of style.css reset to a different state from the style branch](images/merge-conflict-5.png)

I issue the command `git merge style` but the output reads "CONFLICT (content): Merge conflict in index.html. Automatic merge failed; fix conflicts and then commit the result". 

![A WebStorm window with the terminal showing the output of a git merge command, indicating a merge conflict](images/merge-conflict-6.png)

The command output tells me where I need to look for the conflict: in `index.html`. If for whatever reason there are a lot of conflicted files or the message has scrolled off the top of the terminal, or you lost the terminal window, you can see a list of files with conflicts by issuing a `git status` command. Git marks the files in the index as "unmerged". 

![A WebStorm window with the terminal showing the output of a git status command, listing "unmerged paths"](images/merge-conflict-7.png)

When you open the file that has conflicts in it, you will see *merge marker lines*. These consist of less than signs, equals signs, and greater than signs. The less than signs indicate the start of the content that Git doesn't know what to do with. Between the less than signs and the equal signs is the version of the content that exists in the current branch, in this case the main branch. 

![A WebStorm window with the index.html file showing merge marker lines](images/merge-conflict-8.png)

Between the equals signs and the greater than signs is the version of the content that exists in the branch that is being merged, in this case the style branch.

![A WebStorm window with the index.html file showing merge marker lines](images/merge-conflict-9.png)

To resolve the conflict, you will usually pick one or the other version. Select that version and delete the other one and all the merge marker lines. 

In this case, we actually want to keep the content from the main branch, but also the section wrapper from the style branch. So we will have to manually edit the file rather than just picking one or the other version. First, I delete the content from the style branch, then I manually add the section wrappers to the content in the appropriate places.

![A WebStorm window with the index.html file showing content between merge marker lines highlighted for deletion](images/merge-conflict-10.png)

![A WebStorm window with the index.html file showing content edited to the intended version. The top still has a warning that "File has unresolved merge conflicts"](images/merge-conflict-11.png)

Make sure you've deleted all the merge marker lines! In HTML they will just show up as content, but in most computer files they will cause runtime errors. Don't mark the conflict resolved until you've properly resolved the conflict and deleted the marker lines.

With the conflict resolved, I can now stage the `index.html` file (the other file that is being merged, `style.css`, is already staged since it had no conflicts). Notice when I stage the file, the IDE warning, that "File has unresolved conflicts", goes away.

![A WebStorm window with the index.html file showing content edited to the intended version. The waring message is gone. The terminal shows the output of git add -A](images/merge-conflict-12.png)

Now I can commit. This is the result of a merge, so my commit message will reflect that. I will also explain that I solved a merge conflict and where. To accomplish this I am going to enter the text editor rather than entering my message with the `-m` argument on one line. To enter the text editor, just type `git commit` with no arguments.

![A WebStorm window with the terminal showing the vim text editor open and a merge message entered](images/merge-conflict-12a.png)

Many people find merge conflicts confusing. But they are not really that bad. Just look for the merge marker lines. Find the content that is in your current branch and the content that is in the merging branch and choose between the two, then delete the one you don't want. It is rare that you need to combine content from the two versions as we just did. Make sure all the merge marker lines are deleted, then stage the files that are unmerged, and commit.

Most of the time all you have to do is select the version from the merged branch and delete the one in the current branch and you're done. Take it one step at a time, resolve the files one at a time, and you'll soon be done. 

The key to successful merge conflicts is understanding the meaning of the merge marker lines:

```
<<<<<< HEAD
This is the content in the branch I'm on
=====
This is the content in the branch I'm merging
>>>>>>>> style
```

The word *HEAD* is a Git reference to the last commit on the current branch. So it always means "current branch". The starting merge marker lines will almost always say *HEAD*. The ending merge marker lines will have the name of the branch the content is from. The equals signs mark the dividing line between the version of the content in each branch. 

## Pulling from GitHub

Now all the pieces are in place for a fully collaborative feature branch workflow. Let's go back to our cloned repo. Assume that the repo was cloned by our designer. In addition we have a content author. We might also have something like a web programmer, and in more complex projects many more team members.

In a collaborative framework, whenever there is work to be shared with the team, it should be pushed. Here we are ready to start a feature branch workflow, so we should push the main branch. In the future, we will mainly push feature branches.

![A WebStorm window with the output of a git push command](images/branch-workflow-1.png)

Next, our designer will update their repo by pulling from GitHub. The first time this is done, Git may ask you to define a strategy for reconciling differences. You can choose to merge, to rebase, or to restict yourself to fast-forward merges only. 

![A WebStorm window with the output of a git pull command asking the user to specify how to reconcile civergent branches.](images/branch-workflow-2.png)

Rebasing will take the remote changes and just tack them on to the end of your commit history. Selecting "fast-forward" only will only allow merges in which your current branch's history is completely contained in the other branch's history. It will not work if you have added commits in your current branch that are not in the other branch.

For our workflow, we want Git to merge, not rebase, and we don't want to restrict ourselves to fast-forward only. So we need to set the Git configuration to merge divergent branches after pull operations.

I pull, and have merge conflicts in `index.html` and `style.css`. Here, I know for sure that I want to take the remote content over my local content, so the resolution should be simple.

![A WebStorm window with the output of a git config command and a git pull command, showing merge conflicts in two files](images/branch-workflow-3.png)

I start with `index.html`. First I locate the content that is local to me. This begins where the less than merge marker lines are (`<<<<<<< HEAD`) and ends where the equals signs are.

![A WebStorm window the index.html page with merge markers showing the local content](images/branch-workflow-4.png)

I highlight the local content that I want to replace. I include the merge marker lines.

![A WebStorm window the index.html page with merge markers showing the local content between less than signs and equals signs, highlighted for deletion](images/branch-workflow-5.png)

I delete the local version, since I know I want the remote version. Now I just have to remember to scroll down to find the last merge marker line and delete it.

![A WebStorm window the index.html page with merge markers showing all the content from the HEAD gone and the closing merge marker line highlighted](images/branch-workflow-6.png)

My `index.html` file is up to date with the remote. If I wanted, I could do `git add index.html` now, but I will do that all at once when I have resolved all conflicts.

![A WebStorm window the index.html page with merge markers showing all conflicts in the index.html file resolved](images/branch-workflow-7.png)

I switch to the `style.css` file. I will use the same strategy of preferring the remote content over my local content, which makes resolution simple. First I highlight all the content from my current branch, labeled "HEAD" and located between the less than and equals-sign merge marker lines.

![A WebStorm window the style.css page open and the content between the less than and equals sign merge marker lines highlighted](images/branch-workflow-8.png)

I delete all my local content.

![A WebStorm window the style.css page open and all the content between the less than and equals sign merge marker lines deleted](images/branch-workflow-9.png)

I scroll down to find the closing merge marker line.

![A WebStorm window the style.css page open and all the closing merge marker line with greater than signs highlighted](images/branch-workflow-10.png)

I'm done with my conflict resolution. I run `git status` to make sure.

![A WebStorm window the style.css page open and the terminal showing the output of the git status command](images/branch-workflow-11.png)

I stage the two files that had conflicts, then run `git commit` without the `-m` argument, so I can enter the text editor and work with Git's default merge message.

![A WebStorm window the style.css page open and the terminal showing the output of git add, and a git commit command entered but not run](images/branch-workflow-12.png)

I uncomment some of the lines from the commit message so as to preserve the history of where I fixed conflicts.

![A WebStorm window the style.css page open and the terminal showing a text editor open and a merge commit message](images/branch-workflow-12a.png)

I'm all done with merge conflict resolution. It is really a lot simpler than many people think to go through these and resolve the conflicts.

If I am absolutely sure that I want to accept the remote content, and in every case override my local content, there is another strategy I can use.

Here's another pull where I run into a merge conflict with the `style.css` file. Instead of manually resolving it, I say `git checkout --theirs style.css`. This automatically updates the content of `style.css` in my working directory to the remote branch, or to the branch that I am merging into my current branch -- the "other" content, not mine.

![A WebStorm window the style.css page open and the terminal showing a git pull command, with a merge conflict in style.css, and a git checkout --theirs command issued](images/branch-workflow-14.png)

Now I can just add and commit as before. I don't have to resolve the conflicts manually, edit the files, or remove the merge marker lines. Simple.

Although `git checkout --theirs` and its opposite, `git checkout --ours`, are huge time-savers, it is essential when working with Git to be comfortable with manual resolution of merge conflicts, since you may not always be certain what version needs to be preserved.

## Pushing a branch

Now my designer has the current content of the project and is ready to start working. The first thing to do is to switch to a feature branch. They already have such a branch, `site-style`. They can switch to it with a `git switch site-style` command. If they didn't already have the branch, they could create it using `git switch -c site-style`. 

![A WebStorm window the style.css page open and the terminal showing output from a git status and git branch command](images/branch-workflow-15.png)

When they switch to the `site-styles` branch, this branch is not up to date with the main that they just pulled. They update it by merging main into it.

![A WebStorm window the style.css page open and the terminal showing output from a git switch command](images/branch-workflow-16.png)

![A WebStorm window the style.css page open and the terminal showing output from a git switch command](images/branch-workflow-17.png)

They make some changes to the style sheet, stage the changes, and commit.

![A WebStorm window the style.css page open and the terminal showing the output of git status, git add, and git commit commands](images/branch-workflow-18.png)

Now, it is good practice to make sure there aren't going to be any merge conflicts when I share with the team. To do that I need to merge main into my branch. If I can merge main into my branch, there won't be any conflict when someone else merges my branch into main.

![A WebStorm window the style.css page open and the terminal showing the output of git merge main: "Already up to date."](images/branch-workflow-19.png)

No problems here. I am ready to share my work with the team. I push my branch to GitHub. Instead of the usual `git push`, which by default would do `git push origin main`, I need to be specific. I need to say I want to push my `site-style` branch to the remote that has the alias `origin`. The command for this is `git push origin site-style`.

![A WebStorm window the style.css page open and the terminal showing the output of git push origin site-style](images/branch-workflow-20.png)

Notice it offers me a link to click on to make a pull request ... let's click on that link and make our pull request!

## The pull request

## Pulling the updated code