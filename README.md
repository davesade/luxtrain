Welcome to the train in direction Luxembourg - GitHub
===========================
# luxtrain
#### <i class="icon-folder-open"></i> Switch to another document
There are few ways, how to start with Git. Let's have simplest possible example. You would like to start with brand new, merely empty repository. Best is to start locally on your PC, with GIT installed. Let's move to safe area, where we can prepare folder for our new repository.

```
mkdir luxtrain
cd luxtrain
git init -> Initialized empty Git repository in /luxtrain/.git/
```
#### <i class="icon-file"></i> Create a document
Now we have to create some content, let's have simple TXT file.
```
echo "Save our souls" >> help.txt
```
This file is saved in the folder on filesystem, but we have to inform Git about it. This can be done simply by adding new file to the repository.
```
git add help.txt 
```
> **Tip:** Or you could just write . to add all files in the folder

Next we have to commit our changes, to really let Git know, that our work with help.txt file is done.
```
git commit
```




