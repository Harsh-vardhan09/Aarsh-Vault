*By Jon Loeligier*

---
# GIT:-

- Git is a content tracker, What makes it special is its distributed version control.
- This means git is fast and scalable.
- It has a lot of commands which can be used for high level and low level use.

## GIT GUI:-
- This acts as the frontend for git command line tools.
- some connect with famous third party platform.
- They usually only work on local copy of your repo.
![[Pasted image 20260205012414.png]]


### GIT SERVER:
- A git server enables you to collaborate more easily because it ensures the availability of a central and reliable source of truth for the repo you work on.
- A Git server is also where your remote Git repo are stored.
- You have the option to install and configure your own Git server, or you can forgo the overhead and opt to host your Git repositories on reliable third-party hosting sites such as ***GitHub, GitLab, and Bitbucket***.
### Git clients:-
- Git clients interact with local repo, and you are able to interact with Git client.
- Using git command line is best as it allows to familarize with common git command.


# Git Characteristics:-

- ### Git stores revision changes as snapshots:-
	- This means Git does not track revisions changes as a series of modifications, commonly called *Deltas*
	- instead, It takes snapshot of changes made to the state of repo at specific time.
	- *This is called COMMIT in git, Like taking photgraph*

- ### Git is enhanced for local development:-
	- In git you work on a copy of the repo on your local development machine.
	- this is known as local repository, or clone of the remote repo on a git server.
	- local repo have the resources and snapshots of the revision changes made in one location.
	- Git calls this snapshots as _COMMIT HISTORY. 
- ### Git is definitive:-
	- This means git commands are ***explicit***
	- Git waits for instruction on what to do when to do.
	- Git does not automatically sync changes from your local repo to the remote repo.
	- Every action requires your explicit command or instruction to tell Git what is required, including adding new commits, fixing existing commits, pushing changes from your local repository to the remote repository, and even retrieving new changes from the remote repository.
- ### Git is designed to bolster nonlinear development:-
	- Git allows you to ideate and experiment with various implementations of features for viable solutions to your project by enabling you to diverge and work in parallel along the main, stable codebase of your project.
	- This is called branching, it ensures intergrity on main development line.



*In Git branching is considered lightweight and inexpensive because a branch in git is just a pointer to the latest commit in te series of linked commit.

# Git command Line:-












---
## gitignore

 - we use this to ignore files in the git hub to not put in the public repo this can include :-
	 1. env files
	 2. readme files
	 3. log files
- we add folders or files using the file name in it.

```
  *.log //will ignore all files with .log extension
  env/  //will ignore env folder
  
   folder/*.extension //to igonore all files inside the folder with that extension.
``` 
- we can check if the files are added or not using git status.
