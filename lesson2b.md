# Lesson 2, Part 2




## Git Log
Now that we've made a few commits, we can start to look at how our project is evolving. Git log is a good way to do this; we've seen this before, but now we'll see a bit more


```bash
git log
```

This is a very flexible command, however, and there are a few ways we can clean it up to make the output a bit tidier:


```bash
git log --oneline --decorate --graph
```

![](images/gitLog.png)
So what all have we asked for? We've said, show us the log, but in a compact form, decorate it to have it show where the current branch (master) is pointing (HEAD), and show us the graph. Recall that git is a directed graph, where snapshots (commits) are the nodes. On the graph above we have 4 nodes, with the master branch pointing to the latest commit. We can move this pointer, but that's a topic we'll take up later.


## File Management with Git - rm and mv
One thing that is good to remember is that you've added a file to git, then it's being tracked until you delete it. And actually even if you delete it, it gets stored in previous commits - because it _was_ in that repo at one point in time. Though the file is just a file, once it's tracked, don't be tempted to remove it via the operating system. 

If you want to delete a file, or if you want to change it's name, git offers utlities to do this. Not surprisingly, they are ```git rm``` and ```git mv```. Let's see them in operation. First we'll add two files - one to move/rename and one to delete. We'll do this in the ```src``` directory again.


```bash
touch prediction.R
touch simData.R
git add .
git commit -m "Add Two Files for Experimenting with Moving and Deleting"
```

With these setup and added, let's delete it at the command line just to see what happens:


```bash
rm prediction.R
```

All looks good right from the OS point of view - right? What happens when you ask ```git``` what the status is?


```bash
git status
```

What to do now? Well we have options. One thing we could do is to add the deleted file. 

![](images/gitRm2.png)

Or we could back out the change. We do that with the git checkout command. (**n.b.** again - look at the help from git status - it tells you a lot.)


```bash
git checkout -- prediction.R
git status

```

Now we're back to a clean working directory. So we can delete with ```git```. Not that when you do this, and look at the status, it looks very different - the first one makes a change (the deletion) that hasn't been staged, where as the second (```git rm```) makes a change that has been committed and is ready to stage:

![](images/gitrm.png)
Basically to get a clean working directory if you go the ```rm``` route, you have to add the file to the staging area, only to then commit it. ```git rm``` just does that for you in one step. 

Now is a good time to commit the deletion.


```bash
git commit -m "Delete prediction.R File"
```


Now that file is gone and the repo is clean, let's rename the other file - this time using git. As above, if you use git's utilities for this, you don't have to add the changes


```bash
git mv simData.R simXYData.R
git commit -m "Rename Simulation Script for Clarity"
```


## The .gitignore File
One person in the responses to the survey asked about what to version control and what not to. I tend to keep just the code versioned, and things that can be easily created on the fly, I ignore. If you create a lot of these files, e.g. intermediate ```*.Rdata``` files, or ```*.png``` files, etc., your repo can get pretty messy as you create them. Even if you aren't tracking them, git is aware of them, so typing git status can yield a lot of information. 

Thankfully, there's a mechanism to handle this, and it's called the ```.gitignore``` file. All you have to do is create it, put in a few patterns to match (and thus have git ignore whatever file type satisfies the pattern), and then place the .gitignore into version control. Let's try it with a pdf file. First add the plotting device code to your ```plotData.R``` script.


```r
pdf(file = '../results/firstHistogram.pdf')
hist(whales$SST)
dev.off()
```

Add and commit these changes, and then let's run the code from the command line.

![](images/plotHist.png)
What do you think we'll have now? How many new files? Where and what will they be?

If you answered 2 files - gold star for you. There's the .pdf file in the ```results``` directory, but there's also the .Rout file in the ```src``` directory. But if you type ```git status``` what do you think you'll see? 

Probably not what you expected to see, at least it wasn't what I thought we'd see. 

![](images/rout.png)

But here's a good thing to be aware of. We only have the repo initialized in ```src```, and not in results. At any rate, there's a chance we might create the odd pdf file here in addition to the .Rout files, so let's exclude them both. We do this with the   ```.gitignore``` file, which we have to make, and which, we have to place under version control.

To do this, make a new text file, and add these two lines:


```bash
*.Rout
*.pdf
```

Save it as .gitignore, then add it, and then commit it.

Now let's rerun the ```R``` script that will make the ```.Rout``` file and we'll see what happens. Before you type ```git status``` what do you think you'll see?

If all goes well you'll not see anything - git will indicate that you have a clean working directory.


## Time for a Break
Before we move on to replicating this in RStudio, it's probably time for a break.

![](images/Its-Time-For-a-Break.jpeg)