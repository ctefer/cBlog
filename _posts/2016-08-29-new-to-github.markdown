---
published: true
layout: post
date: '2016-08-29'
categories: jekyll github
---
# GitHub for New Users
As a new user to GitHub the learning curve has been very steep. I’ve found very few projects that give a concise list of actions to take in order to start developing. So here’s the rundown of actions to perform when starting a new project.

1. Fork
2. Clone
3. Branch
4. Develop
5. Commit
6. Pull Request

Keep in mind that these steps are for open source projects that need active developers. Each project is a little different and has community created Workflow and Code of Conduct. Before beginning any project make sure you follow their guidelines. 

## Before we go into details
What makes this most confusing is that Git is ever changing and constantly being tweaked by a community of bleeding edge web developers (I’d like to think so). To my knowledge, there are five ways of performing Git specific tasks, and that’s just using Windows. There’s GitHub, [GitHub Desktop]( https://desktop.github.com/), GitHub Shell, and [Git]( https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) itself which installs the Git GUI (unrelated to GitHub) and Git Bash (unrelated to GitHub Shell). 
The general set of actions to take is to perform most of your upper level repository work on GitHub. Things like repository settings, deleting repositories, searching and forking can all be done with ease on the website. Simple lower lever repository actions like branching, cloning, committing, merging, can be performed at the GUI levels (GitHub Desktop, GitHub GUI). Commands that require specialized options are done at the shell levels (GitHub Shell, Git Bash). 

From the shells you can pretty much do anything, and some things are much simpler. This guide will go through my preferred methods of performing each task, but you (or the Git community) may develop easier methods to perform these operations.

### `Side Note`
*This is not an exhaustive list of actions that can be performed, just the most useful ones. As you continue to develop you `will` run into issues which require shell commands. Learn to be comfortable using the shell.*

## Fork Me!
The fork is how things get done on GitHub. One who understands Git will see that I started by explaining fork/branch using the top-down approach. Fork and branch are very similar, but where branch is used at the repository level (more on this later) Fork is used at the project level. Fork allows you to create your own separate repository which can be updated from the project’s repository. From [GitHub Help]( https://help.github.com/articles/fork-a-repo/):
*A fork is a copy of a repository. Forking a repository allows you to freely experiment with changes without affecting the original project.*
A Fork can easily be done from the project’s web space on GitHub. And many project documents and websites contain the red “Fork Me” ribbon which will automatically fork a repository to your page. 

## Clone
Once you have the project in your GitHub repository, you need to get it onto your machine for the actual development (which I discovered isn’t entirely true, but for developing code it’s the safest bet). This can be done in several ways, but the best I’ve found is to use the GitHub Desktop application. 
 
### `Side Note`
*If you’re working on something like a GitHub page or documentation that you don’t have a clone for you can use [prose.io]( http://prose.io/) which contains a simple markdown editor. GitHub has some very specialized features for editing and updating their GitHub page websites. Jekyll is a very popular static blogging framework which GitHub auto builds for deployment. Stay tuned for my blog post on gh-pages.*

## Branch
To reiterate, if forks are for repositories, branches are for projects. Forks and branches should remind you of real forks and real trees because that’s basically how they are treated. When you create a branch or a fork you pair off the main (or master) trunk. Forks and branches can be modified independently from the master without affecting it. 
Forks are slightly different from branches (aside from the project/repository aspect) in that they are never merged back into the master branch and are always kept as a separate repository. Forks are fed information from the master without ever pushing data back. 
A branch can be merged back into the master if it meets the ruleset established. This usually means all conflicts between the master have been cleared and that the branch before your changes were made was up-to-date with the master.  
The GitHub Desktop allows you to create branches and check them out. Additionally, opening up the Git Bash within the project directory (right-click->Git Bash in Windows) provides much more control. 

{% highlight C++ %}
#Create a new branch and checkout
(master)~$ git branch <branchname>
(master)~$ git checkout <branchname>
(branchname)~$
#optionally create a branch and checkout
(master)~$ git checkout –b <branchname>
(branchname)~$
{% endhighlight%}

## Develop
This is all on you on how this is achieved. Generally, a project will establish a development workflow which contains rules for installation/developing/testing. If you're comfortable with Git, then this is going to be your struggle point for every program you develop for. Environment variables, coding styles, and with new languages: syntax, syntax, syntax. 

Once you feel you're done coding, you get to do the one thing every coder loves. `Testing`. Followed by our second favorite endearment. `Documentation`. Print statements and debugging are fine for development, but if you want people to really see what you do you need to create test cases and supply ample commenting. 

With some project, testing is an absolute requirement. Testing for each block of code added, plus regression testing to make sure what you did didn't break anything. Don't be surprised if you're required to write test cases prior to you even being able to create a Pull Request (more on this later). 

Documentation may also be a requirement on a project especially if you're adding new functionality, new files, or redefining the architecture. The best way to fight [spaghetti code](https://en.wikipedia.org/wiki/Spaghetti_code) is to test and comment often. From an experienced software developer, creating test cases and developing documentation should take you almost as long as it took you to program. *Each*. 

If you haven't seen it before, I recommend checking out [Vagrant](https://www.vagrantup.com/). This is a great development tool for setting up your program with virtual machines. See my project blog [RoR Vagrant](https://ctefer.github.io/cBlog/jekyll/2016/08/28/RoR-Vagrant.html) for some details.

## Commit
If you've ever worked with versioning system, the concept of committing your work is easy. You have your repository some place online, you have your file system at home. The process of committing changes and synching changes are transactions between your home and your online files. Committing pushes the changes you created from your at home up into the cloud. Synching then is the reverse. If a change has been made online, then synching will pull those changes back. 

This is an important concept when working with a versioning system and having many different authors making commits and synching to online repositories. Committing your changes also produces a history of your workflow. It creates minor versions of your coding that can be returned to if required. 

### `Side Note`
*Don't merge here, it may be tempting to, but...*

## Pull Request
From here you may be wondering about merging your code back into your master. But if you're working on someone else's project, merging back into your master means that your master will not reflect the current main branch from the other, forked, repository. Before you can officially push your code into someone else's project, you need to let them review it. This is another main staple of Git, the ability for others to see and comment on your work. 

In order to do this, you create a Pull Request. The Pull Request is a generated list of all difference between your code and the main repository. The Pull Request allows you to describe why you're making the change, what changes you made, and a history of commits that are generated. 

A pull request is also tracked by Git like an issue. It's given a unique number and allows others to comment against your code. The project may also run automated tests on your pull request to ensure you meet with the coding standards and guidelines of the project. 

A Pull Request may be made from any of the Git tools, but I think this part is very important and makes the Pull Request much easier. From your `own` profile online. Select the repository you wish to merge to the main branch (`we are not actually going to merge, don't try`). Select the `Pull Request` button. If you've done everything right up till now, you should see the master base and the master fork selected. You should also see your own repository plus the branch you wish to compare to. If *master* is shown, you should select the branch you created in order to perform your edits. Fill in the description of your branch and create the new request. 


## Let's Wrap Up

  1. Fork
    * Select a repository from another project that you would like to work on
    * Fork the repository so that the main branch is linked and copied to your repository
  2. Clone
    * Clone the repository to your home system using the GitHub Desktop
    * Cloning from the desktop will automatically link your file system to the repository
  3. Branch
    * Branch and checkout a new branch for editing
    * Editing in the new branch will protect the master branch from being updated
  4. Develop
    * Develop your code according to the standards set forth by the project
    * Ensure to create your test cases and provide the necessary documentation
  5. Commit
    * Commit your code often so that if your PC is destroyed you can re-clone your work
    * Provide good documentation for commits so that others understand your process
  6. Pull Request
    * Create your pull request so others know when you're ready to push your changes to the main repository
    * Pull Requests also help others comment and your code and can accept or decline your changes


Above all, every project is different. Different rules, workflow, and management styles. Be sure to learn as much about your project as you can before you attempt to push changes. Happy Coding!



# Citations

| Name | Comment|
|---|---|
|[Git](https://git-scm.com/docs/) | The GIT main page | 
|[GitHub]() | The GitHub Repository - Provides free accounts for public development|
|[GitHub Desktop](https://desktop.github.com/)|Windows GitHub Desktop environment|
|[Vagrant](https://www.vagrantup.com/)| Automated development environment setup using VM's|
|[prose.io]( http://prose.io/)| Git Repository markdown editor|
