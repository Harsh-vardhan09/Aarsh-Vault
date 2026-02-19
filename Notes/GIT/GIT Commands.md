### 🔥 GIT MASTER ROADMAP
- We’ll cover:
	1. 🧠 How Git actually works (very important)
	2. 🟢 Basic commands (must know)
	3. 🟡 Intermediate commands (real developer level)
	4. 🔴 Advanced (rebase, reset, fix history)    
	5. 📁 `.gitignore` mastery
---
# How git actually works:-

- Git has three main parts

```mathematica
Working Directory → Staging Area → Repository

```
*Here*
	- working directory =our files
	- staging area=files ready to commit
	- Repository=Saved history

***When we do `git add.` we move changes to the staging area.
When we do `git commit -m "message"` we move staged changes to the Repository.

## *Basic command:-

- `git init` : Creates a `.git` folder which we don't see but is hidden brain of git
- `git status` : checks untracked files, modified files, Staged files 
- `git add file.txt` : Adds a single file
- `git add .` : Adds everything for commit
- `git commit -m "message"`:
	- here we commit all changes to the remote repository.
	- It is similar to saving snapshot of our project on remote repo.
	- here `-m` is shortened for `-message`
- `git log` : It shows history of the git commits
- `git log --oneline` : Shows only oneline of commit 

```csharp
git remote add origin <url>
git push -u origin main
```
***We use these commands to connect to remote repository and push the code there.

## Intermediate command:-

- ### `Git Restore`
	- Restores the file to the last committed version.
	- undo my current changes.
	- This only works for the Modified but not committed changes.

| Situation            | Command                   |
| -------------------- | ------------------------- |
| Undo changes in file | git restore file          |
| Remove from staging  | git restore --staged file |
	
- ### `git reset`
	- Reset moves HEAD(current commit pointer).
	- This makes it possible to undo various git operation.
	- for example commit, merge, rebase and pull.
	- ``git` `reset` [_<mode>_] [_<commit>_]`
	- #### `git reset --soft `
		- Removes last commit
		- keeps changes staged
		- ==Undo commit, but keep my code ready==
	- #### `git reset HEAD~ `
		- Removes last commit
		- keeps changes 
		- but unstaged
		- ==Undo commit, I’ll decide what to add again.==
	- #### `git reset --hard HEAD~1 `
		- Deletes commit
		- Deletes changes
		- Cannot recover easily
		- ==Delete everything. I don’t care.==
- #### `git stash`
	- Temporarily saves your changes without committing.
	- It doesn't  commit but pauses it.
	- So if we need to fix a bug in main we can stash switch to main and work there.
	
- #### Branching:- 
	- Branch = separate timeline.
	- This allows to work on a project while letting the main work as it is.
	- `git switch -c feature-login`
- #### `git merge`
	- combine two branches 
	- `git merge feature-login`
	- ==This allows a user to use feature login and put it in main code ==


### Advanced Commands:-

- #### `git rebase main` 
	- Move your branch on top of another branch
	
```matematica
main:      A --- B --- C
feature:         D --- E

main:      A --- B --- C --- D --- E

```

- #### `git cherry-pick 9f8a21`
	- Takes one commit from another branch 
	- it copies that commit into current branch

|Merge|Rebase|
|---|---|
|Keeps history|Rewrites history|
|Safe|Cleaner|
|Adds merge commit|Linear history|

---
# .gitignore:-

- `.gitignore` is a **text file** that tells Git:
>
> “Do NOT track these files or folders.”
>
- It prevents 
	- secret files 
	- large folders 
	- build files 
	- log 
	
**From being pushed to the git repo.

- ***We make .gitignore file in the main root of the project.
- Git reads these file and if any file matches that rule *It ignore it*
### NOTE :-
*==`.gitignore`  file only works for untracked files ==. if git is already tracking a file ignoring will not work*

**To stop tracking
```bash
git rm --cached filename
```

## Patterns of gitignore:

- #### Ignore One Specific File

```
secret.txt
```


- #### Ignore Specific File Inside Nested Folders

```
backend/config/private.txt
```


- #### Ignore Any File With That Name Anywhere

```
**/secret.txt
```


- #### Ignore Entire Folder

```
node_modules/
```



- #### Ignore Folder Inside Folder Inside Folder

```
backend/uploads/images/
```


- #### Ignore Any Folder With That Name Anywhere

```
 **/build/
```


- #### Ignore All Files With Extension Anywhere

```
*.log
```


## Advanced Patterns:-

- #### Ignore Everything

```markdown
*
```

Then allow specific ones:
```
*
!.gitignore
!src/
```


- #### Ignore Everything Inside a Folder But Keep Folder

```
build/*
```


- #### Ignore Multiple Patterns

```bash
	*.log
	*.env
	node_modules/
	dist/
```

***
