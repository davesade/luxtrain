Training instructions for GitHub (EE)
===========================

###Contents:

* [Starting from Scratch](#fromScratch)
* [Starting from existing repository on a server](#startingWithExisting)
* [Branching](#branching)
* [Tagging](#tagging)
* [Pull Requests](#pullRequests)

###<a name="fromScratch"></a>Starting from Scratch

#### <i class="icon-folder-open"></i> Create new folder
There are few ways, how to start with Git. Let's have simplest possible example. You would like to start with brand new, merely empty repository. Best is to start locally on your PC, with GIT installed. Let's move to safe area, where we can prepare folder for our new repository.

```
mkdir luxtrain
cd luxtrain
git init -> Initialized empty Git repository in /luxtrain/.git/
```
#### <i class="icon-file"></i> Create new document
Now we have to create some content, let's have simple TXT file.
```
echo "Save our souls" >> help.txt
```
This file is saved in the folder on filesystem, but we have to inform Git about it. This can be done simply by adding new file to the repository.
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
>**Tip:** Consider commit messages as important part of your workflow. It WILL greatly help, when tracking changes in the past.

#### <i class="icon-upload"></i> Publish a document
Let's say we are happy with out work, so it's time to publish our work on main server. To do that, we have to add remote location, by default called **origin**. In our example, let's use public GitHub.com, but it works the same also for GitHubEnterprise.

```
git remote add origin https://github.com/davesade/luxtrain
```
>**Tip:** In Git vocabulary, "origin" indicates default place for your future commits, which is shared by whole team usually. You can have multiple remotes set by the way, if you push your code to multiple servers, i.e. Heroku Git.

>**IMPORTANT:** You have to prepare empty repository on server in advance! That is usually not a big deal and if you follow instructions on webgui properly, it will actually show you all commands you have to do on your workstation, to push data from your local PC.

Last thing to do is actually to push our changes. That follow this syntax:
```
git push [remote] [branch]
```
Branch? What is a branch? It is important part of your development strategy and we will talk about it later. To make it simple now, let's continue. In case you are not sure, you can simply list all branches in your repository. By default, you should see a master branch.
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

With Git, you **ALWAYS** work with complete repository, even on your local PC - that's why it's called **distributed version control system**. There is usually single point of place - main server - where project is being held, which is then shared between many developers, pushing their contributions to remote origin. So easiest way to start contributing to existing project is to **clone** it from a server. Let's do it right now!
```
git clone https://github.com/davesade/luxtrain
Cloning into 'luxtrain'...
remote: Counting objects: 37, done.
remote: Compressing objects: 100% (31/31), done.
remote: Total 37 (delta 4), reused 20 (delta 1), pack-reused 0
Unpacking objects: 100% (37/37), done.
Checking connectivity... done.
```
As you can see, this command created a folder and fetched all necessary data, complete repository, all files, all branches and tags.

Alternatively, you could do a **fetch** from remote repository. This implies, that you will add remote origin manually, which might be usefull in a case you work with multiple servers (ie. GitHub and HerokuGit). The **origin** is usually single common place share by all developers in the team, where they contribute to.

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
Last but not least, we could simply **pull** from remote repository. The difference is, that this will not only **fetch** as in example above, but also to **merge** with your current folder. Depending on what you want, this might be better in some cases.

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

###<a name="branching"></a>Branching
####Basics
With branches, you can organise your work in case you have to synchronise with more colleagues contributing to the same project. You can create new branch at any time by typing:
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

There is actually faster way of creating branch. When you are checking out a branch which doesn't exists, you can add **-b** to create a branch, based on another branch you name, typically master.
```
git checkout -b love master
```

>**Tip:** There is no single winning strategy about branching. GitHub workflow is heavily relying on a fact, that each developer commits to it's own branch, push it into main branch (master or main feature branch) and then raise a pull request. In case of small changes or quick bugfixes, you might easily delete a branch, after you fix your issue (typically SIR fixes), however in case of feature branches, it is recommended to keep it for easier tracking.

Do not forget to add and commit and push your changes, when you are done working:
```
git add .
git commit -m "Second commit"
git push origin love
```
Each team should come up with his own strategy on branching. Actually, you can't destroy that much, as with Git you simply work with complete project at any given point of time. So in case you would like to delete a branch (after you pushed your changes to origin server), you can type:
```
git branch love -d
```
###<a name="tagging"></a>Taging
Now we know how to start working in our local repository, how to do branching and how to push our branches to main server. At one point of time, we would like to ask CFM to actually pick our work and send it to the pipeline for further testing and deployment, up to production. To do that, you have to provide a **tag**, which is equivalent of **label** in Clearcase. To add a new tag, type following command.
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
In case you would like to add lightweight tag, you can skip **-a** parameter. You can still add a tag message.

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
>**Tip:** You cannot create a tag via webgui of GitHub Enterprise.

If you would like to remove a tag, just type in:
```
git tag [tag_name] -d
-> Deleted tag [tag_name] (was 4f0c35b)
```
And if you need to remove a tag from remote origin, type this:
```
git push --delete origin [tag_name]
```
Now you are ready to raise your **EEPR** in Amadeus, as CFM expects **tag** indicating unchangable point in time for their pipeline.

###<a name="pullRequests"></a>Pull requests

####Raising new pull request
It is usual, that bigger teams have to organise their contributions via multiple branches. Before taging final product to be sent down the pipeline, team has to merge all changes from all relevant branches. To have an overview and control over merge process, it is usual to have main coordinator (head of team perhaps?) responsible for merging all the code (however developer might be responsible for fixing the conflict). From developer perspective, this is best to be done via raising a pull request. This way developer says: "My work is done, everything is on the server, please merge it into master or main feature branch".

Let's assume, that we just pushed **love** branch to main server.
```
git push origin love
```
Now have a look at your repository via web browser, you will notice new button for raising pull request. Select **master** as a base and compare it with **love**. GitHub will show you difference, allow you to write comment and start discussion.
>**IMPORTANT:** Using pull requests in GitHub has a form of actual discussion about changes you would like to merge. Use this opportunity to ask colleagues for opinion, help or review. Use @mentions to reach other people from totally different projects, to have them contributing into your work. Use #numbers to relate your changes to existing GitHub issues. Consider this as cultural thing, to show your work publicly, to share and exchange ideas. Obviously there might be smaller teams, which are going to commit directly to master branch or maybe they will merge their own pull requests in case of minor changes.
>**IMPORTANT:** While Git does offer pull request command as well, it serves merely as a check for potential merge. It is to be not possible to raise pull request for GitHub via command line interface of Git. Pull requests are in this sense purely function of GitHub, where you have to use webgui.

There might be situation to refuse the pull request. Simply click on **close** button.
