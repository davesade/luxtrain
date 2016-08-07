Training instructions for GitHub (Enterprise)
===========================

##Contents:
* [Git Overview](#gitOverview)
* [Starting from Scratch](#fromScratch)
* [Starting from existing repository on a server](#startingWithExisting)
* [Branching](#branching)
* [Tagging](#tagging)
* [Pull Requests](#pullRequests)
* [Migration Notes](#migrationNotes)
* [Important Literature](#literature)

##<a name="gitOverview"></a>Git Overview

* Git in a nutshell
 * Explain Master/Origin
 * <img src="https://github.com/jeromewagener/luxtrain/blob/master/diagrams/OriginRemote.png" width="300">
 * Explain difference between REL and DEV organization
 * Right now, on REL repositories for all projects already exist, but are empty. These empty repositories are then forked to dev where they have to be populated with the files from your Clearcase LATEST branch. Once this is done, you can push the files to the DEV. With the next EPR, the code is then copied (pulled) to REL.
* Show branching example and why it is useful
 * <img src="https://github.com/jeromewagener/luxtrain/blob/master/diagrams/branching.png" width="300"> 
* Gitflow-Workflow

##<a name="fromScratch"></a>Starting from Scratch

#### Installing Git
* Where to get installation files
```
https://git-scm.com
https://desktop.github.com/
```
* How do I setup Git in Eclipse, Netbeans or IntelliJ
 <img src="https://github.com/jeromewagener/luxtrain/blob/intellij/screenshots/intellij_integration/git_integration_done.png" width="500"/>
 <img src="https://github.com/jeromewagener/luxtrain/blob/intellij/screenshots/intellij_integration/preferences_version_control_add.png" width="500"/>
 <img src="https://github.com/jeromewagener/luxtrain/blob/intellij/screenshots/intellij_integration/git_integration_done.png" width="500"/>

* How do set the Git proxy in case you want to access an external Github repository (Not needed for the internal Enterprise installation):
```
git config --global http.proxy http://webproxy.deutsche-boerse.de:8080
```

#### Create a new folder
There are a few ways how to start with Git and we will first have a look at the simplest possible example. In fact, we would like to start with brand new merely empty repository locally on your PC where Git is installed. Let's move to safe area, where we can prepare a folder for our new repository.

```
mkdir luxtrain
cd luxtrain
git init -> Initialized empty Git repository in /luxtrain/.git/
```
#### Create a new document
Now we have to create some content, let's have simple TXT file.
```
echo "Save our souls" >> help.txt
```
This file is saved in the previously created folder on the filesystem, but we have to inform Git about it. This can be done simply by adding new file to the repository.
```
git add help.txt 
```
> **Tip:** Or you could just write **git add .** to add all files in the folder

Next we have to commit our changes, to really let Git know, that our work with help.txt file is done.
```
git commit -m "First commit"
 [master (root-commit) c032da3] First commit
 1 file changed, 1 insertion(+)
 create mode 100644 help.txt
```
>**Tip:** Consider commit messages as an important part of your workflow. It WILL greatly help, when tracking changes in the past. It is oftentimes a good idea to also put SIRe numbers or generally ticket numbers in this message. __(E.g. SIR 12345 : Fix CSS display bug of dropdown menu)__

#### Publish a document
Let's say we are happy with our work, and it is now time to publish our work on the main server. To do that, we have to add a remote location, by default called **origin**. In our example, let's use public GitHub.com, but it works the same also for GitHubEnterprise.

```
git remote add origin https://github.com/davesade/luxtrain
```
>**Tip:** In Git vocabulary, "origin" indicates the default place for your future commits, which is usually shared by whole team. By the way: You can set multiple remotes to support pushing your code to multiple servers. (e.g. Heroku Git)

>**IMPORTANT:** You have to prepare an empty repository on the GitHub server in advance! This is usually not a big deal and if you follow the instructions on the webgui properly, it will actually show you all commands you have to do on your workstation, to push data from your local PC.

Last thing to do is actually to push our changes. That follow this syntax:
```
git push [remote] [branch]
```
Branch? What is a branch? It is an important part of your development strategy and we will talk about it later. To make it simple now, let's continue. In case you are not sure, you can simply list all branches in your repository. By default, you should see a master branch.
```
git branch
*master
```
Asterix indicates your current branch. In case you have too many branches to look around, you can use also status command:
```
git status
On branch master
nothing to commit, working directory clean
```
Alright, so finally now we can push our work to the server!
```
git push origin master
Username for 'https://github.com': davesade.42@gmail.com
Password for 'https://davesade.42@gmail.com@github.com': 
Counting objects: 3, done.
Writing objects: 100% (3/3), 228 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/davesade/luxtrain
 * [new branch]      master -> master
```
**Congratulations**, you just pushed your first commit!

###<a name="startingWithExisting"></a>Starting from existing repository on a server

With Git, you **ALWAYS** work with a complete repository, even on your local PC! That's why it's called **distributed version control system**. There is usually a single point/place - main server - where the project is being located. This main server is then shared between many developers which push their contributions to this remote origin. In short: The easiest way to start contributing to an existing project is to **clone** it from a server. Let's do it right now!
```
git clone https://github.com/davesade/luxtrain
Cloning into 'luxtrain'...
remote: Counting objects: 37, done.
remote: Compressing objects: 100% (31/31), done.
remote: Total 37 (delta 4), reused 20 (delta 1), pack-reused 0
Unpacking objects: 100% (37/37), done.
Checking connectivity... done.
```
As you can see, this command created a folder and fetched all necessary data. This includes the complete repository, all files, all branches and all tags.

Alternatively, you could do a **fetch** from the remote repository. This implies that you will add a remote origin manually, which might be useful in case you work with multiple servers (ie. GitHub and HerokuGit). The **origin** is usually single common place shared by all developers in the team, where they contribute to.

```
mkdir luxtrain
cd luxtrain
git init
git add remote origin https://github.com/davesade/luxtrain
git fetch
remote: Counting objects: 12, done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 12 (delta 0), reused 6 (delta 0), pack-reused 0
Unpacking objects: 100% (12/12), done.
From https://github.com/davesade/luxtrain
 * [new branch]      master     -> origin/master
```
Last but not least, we could simply **pull** from a remote repository. The difference is, that this will not only **fetch** as in the example above, but also it also does a **merge** with your current folder. Depending on what you want, this might be better in some cases.

```
mkdir luxtrain
cd luxtrain
git init
git remote add origin https://github.com/davesade/luxtrain
git pull
remote: Counting objects: 12, done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 12 (delta 0), reused 6 (delta 0), pack-reused 0
Unpacking objects: 100% (12/12), done.
From https://www.github.com/davesade/luxtrain
 * [new branch]      master     -> origin/master
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> master
```
But generally speaking, **clone** will cover most of your use cases.

##<a name="branching"></a>Branching
####Basics
With branches, you can organise your work in case you have to synchronise with more colleagues that are contributing to the same project. You can create new branch at any time by typing:
```
git branch love
git branch
  love
* master
git checkout love
git branch
* love
  master
```
Now you are ready to work in your **love** branch, without being afraid of interrupting your colleagues working on different features.

But there is actually a faster way of creating branch. In fact, when you are checking out a branch which doesn't exists, you can add the **-b** argument to create a branch automatically based on another branch you specify. (typically master)
```
git checkout -b love master
```

>**Tip:** There is no single winning strategy about branching. GitHub workflow is heavily relying on the assumption that:
1. each developer commits to it's own branch and 
2. then pushes it into main branch (master or main feature branch) and 
3. then raise a pull request
In case of small changes or quick bugfixes, you might easily delete a branch after you fix your issue. (typically SIRe fixes) However in case of feature branches, it is recommended to keep it for easier tracking.

Now, do not forget to (1) add (2) commit and (3) push your changes when you are done working:
```
git add .
git commit -m "Second commit"
git push origin love
```
Each team should come up with its own strategy on branching. In case you would like to delete a branch __(after you pushed your changes to origin server)__, you can type:
```
git branch love -d
```
##<a name="tagging"></a>Taging
Now we know how to start working in our local repository, how to do branching and how to push our branches to main server. At one point of time, we would like to ask CFM to actually pick our work and send it to the pipeline for further testing and deployment, up to production. To do that, you have to provide a **tag**, which is the equivalent of a **label** in Clearcase. To add a new tag, type the following command:
```
git tag -a v.0.2 -m "Please test this in production"
```
Tag message is not mandatory, but it could help. Now you can look for this information, for example:
```
git tag
v.0.2
git show v.0.2
-> tag v.0.2
Tagger: David Kubec <davesade.42@gmail.com>
Date:   Mon Aug 1 23:35:18 2016 +0200

Please test this in production

commit 4f0c35b344480f5e7b69e52c6c1f805f79bdc18d
Merge: ad9f48e 29dbb5b
Author: David Kubec <davesade.42@gmail.co,m>
Date:   Mon Aug 1 23:22:47 2016 +0200

    OKMerge branch 'love' of https://www.github.com/davesade/luxtrain into love

```
In case you would like to add a lightweight tag, you can skip the **-a** parameter. You can still add a tag message.

>**IMPORTANT:** Tags are NOT pushed to origin server by default!

You have to push to origin with name of the tag, instead of branch name.
```
git push origin v.0.2
Counting objects: 1, done.
Writing objects: 100% (1/1), 180 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To https://www.github.com/davesade/luxtrain
 * [new tag]         v.0.2 -> v.0.2
```
>**Tip:** You cannot create a tag via the webgui of GitHub Enterprise.

If you would like to remove a tag, just type:
```
git tag [tag_name] -d
-> Deleted tag [tag_name] (was 4f0c35b)
```
And if you need to remove a tag from remote origin, type this:
```
git push --delete origin [tag_name]
```
Now you are ready to raise your **EEPR** in Amadeus. CFM expects a **tag** indicating an unchangable point in time (revision) for their pipeline.

##<a name="pullRequests"></a>Pull requests

####Raising new pull request
It is not uncommon that bigger teams have to organise their contributions using multiple branches. Before taging the final product to be sent down the pipeline, teams have to merge all changes from all relevant branches. To have an overview and control over the merge process, it is usual to have a main coordinator (e.g. head of team) which is responsible for merging all the code while the developer might be responsible for fixing the conflict. From developer perspective, this is best to be done by raising a pull request. This way developer the says: "My work is done, everything is on the server, please merge it into master or main feature branch".

Let's assume, that we just pushed the **love** branch to main server.
```
git push origin love
```
Now have a look at your repository via web browser, you will notice a new button for raising pull request. 

<img src="https://github.com/jeromewagener/luxtrain/blob/master/screenshots/pull_love.png" width="300">

Select **master** as a base and compare it with **love**. GitHub will show you differences and allows you to write a comment and start discussion.

<img src="https://github.com/jeromewagener/luxtrain/blob/master/screenshots/pull_love_2.png" width="300">
<img src="https://github.com/jeromewagener/luxtrain/blob/master/screenshots/pull_love_3.png" width="300">

>**IMPORTANT:** Pull requests in GitHub should be close to an actual discussion about the changes you would like to merge. Use this opportunity to ask colleagues for opinions, help or review. Use @mentions to reach other people from totally different projects in order to have them contributing into your work. Use #numbers to relate your changes to existing GitHub issues. Consider this as cultural thing, to show your work publicly and to share and exchange ideas. Obviously there might be smaller teams, which are going to commit directly to the master branch or maybe they will merge their own pull requests in case of minor changes.
>**IMPORTANT:** While Git does offer a pull request command as well, it serves merely as a check for potential merges. It is simply not possible to raise pull request for GitHub via the command line interface of Git. Pull requests are a dedicated feature of GitHub, where you have to use the webgui.

There might be situation to refuse the pull request. Simply click on the **close** button in these cases.

Last but not least, if a branch is no longer needed, you can delete it via the webgui or the command line.

<img src="https://github.com/jeromewagener/luxtrain/blob/master/screenshots/pull_love_4.png" width="500">

##<a name="migrationNotes"></a>Migration Notes
* All old/existing code will remain on Clearcase for at least another ten years. This way you can always go back to check what exactly happened before the migration.
* Before your first EEPR, give CFM a heads up as they need to adapt their scripts. This will take a few days.
* There is a script that helps you migrating Clearcase VOBs to Github Repositories
* You need to adapt your Jenkins to fetch the code from the new locations. For this you need to use the following plugins installed on Jenkins
 * GitClientPlugin
 * GitPlugin
 * Github API Plugin
* You will also need a technical user for Jenkins as well as other administrative tasks. Otherwise somebody from the team will have to use his personal account to do the initial migration etc.. In which case, Jenkins would also use these credentials...
* How to migrate in a nutshell *(assuming you want to migrate the old Clearcase VOB called WebApplicationVob to the new Github Repo called WebApplicationRepo)*
```bash
# First, create a new empty reposity called WebApplicationRepo using GitHub
# Copy the repository URL as we need it later. It will end in .git and can be retieved using the clone button
# Normally, all repositories have already been created for us and we just need to populate them with our files

# Change into the command line and type
mkdir WebApplicationRepo
cd WebApplicationRepo
git init

# Copy everything you want to migrate into the newly initialized repository
cp -rf ../WebApplicationRepo .
git add .
git commit -a - m "initial commit"

# Set the remote location using the URL from the webgui
git remote add https://github.com/davesade/WebApplicationRepo.git

# push to the origin and you are done!
git push -a origin master
```

##<a name="literature"></a>Important Literature
* https://www.atlassian.com/git/tutorials/what-is-git *(For every topic covered above, additional explanation can be found here)*
* https://www.atlassian.com/git/tutorials/comparing-workflows/ *(Really recommanded!)*
