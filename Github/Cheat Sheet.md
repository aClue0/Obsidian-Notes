- This is a reference list of the most commonly used Git commands.
- Try to familiarize yourself with the commands so that you can eventually remember them all:
# Commands 
## Remote Repository
```bash
git clone git@github.com:USER-NAME/REPOSITORY-NAME.git
git push or git push origin main (Both accomplish the same goal) in this context)
```
## Committing
use this command to use vscode as the main committing message maker:
```bash
git config --global core.editor ""application /bash" --wait"
```
for example
```bash
git config --global core.editor "code --wait"
```
- and `code` here can be `vscodium`, etc.

This makes it so if you do **`git commit`** it opens a file in vscode it opens a new file:  

![[Pasted image 20260422035047.png]]
It has a colored column at 50 chars and 72 chars for the known reasons at [[2. Git basics#The Seven rules of a great Git commit message|Committing Rules]], So this is really cool.
## Normal Workflow
```bash
git add .
git commit or git commit -m "message"
```
#### If you want to remove the added files you can do 
```bash
git reset or git restore
```

## Commands related to checking status or log history
```bash
git status
git log
git shortlog
git blame
```
The basic Git syntax is `program | action | destination`.

For example:

- `git add .` is read as `git | add | .`, where the period represents everything in the current directory;
- `git commit -m "message"` is read as `git | commit -m | "message"`; and
- `git status` is read as `git | status | (no destination)`.
## Commands related to changing root 
What happens if you have a git init in one root let's say:
You have a repo called "Odin Projects" on github and the root is `~/Odin`
and you want to change the root to `~/Odin/Projects` how can you do that?
Basically, you want to keep the commit logs and the history of the project which is inside `~/Odin/.git` so what you can do is just move that `.git` file to the directory you want to make root so basically:

```bash
cd ~/Odin
mkdir Projects
mv .git /Projects
cd Projects
```

And then add all the files from `~/Odin` to `~/Odin/Projects` Voila!

but there is a **problem**,  when moving the files, it basically acts as if all the files have been created again. so we have to add the history of the files to commit.

Tell Git to update its index with all the new file locations by staging _everything_ (both the deletions of the old paths and the additions of the new paths):

```bash
git add -A
git commit -m "Moved root to Projects directory"
```

*Notes:* 
- If you have a `.gitignore` file make sure to mv it as well!
- If any of the files has a path related to the path you changed, it won't work so make sure after you change the root that you check the paths inside the project

# Push & Pull Errors
Sometimes the main branch on your remote is ahead of the local main.

It will sometimes give you an error that looks like this:

```shell
❯ git push
To github.com:aClue0/Obsidian-Notes.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:aClue0/Obsidian-Notes.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

This means that:
- Your local `main` has **1 commit** that the remote doesn’t have.
- The remote `origin/main` has **2 commits** that you don’t have.
- Git refuses to push because it would overwrite history.
## How to solve this?
To answer this question you have to understand 3 things:
1. `git fetch`
2. The difference between `rebase` and `merge`
3. `git pull --rebase origin main`
## Firstly what does **`git fetch`** do?
It basically means, download the changes made in the origin/main (remote repository) but **don't** merge them. 

After `git fetch`:

Git updates a **separate pointer** called:

```
origin/main
```

So now you effectively have **two versions** of the branch:

- `main` -> your local branch (your 1 commit)
- `origin/main` -> the remote version (those 2 commits)

> These are just two different timelines you can compare.

**So, How can you compare?**
#### 1. See commits you don’t have

```shell
git log main..origin/main
```

```shell
❯ git log main...origin/main
commit ab2fc29f2956adf7e397d238f8bcaa1a1ddacfa4 (HEAD
 -> main)
Author: Hazem Magdy <1hazemmagdy@gmail.com>
Date:   Wed Apr 29 07:05:06 2026 +0300

    This commit is on Local

commit c8c126e991a143da61db6c415de95efce79084af (origin
/main, origin/HEAD)
Author: Hazem Magdy <1hazemmagdy@gmail.com>
Date:   Wed Apr 29 07:03:42 2026 +0300

    This is a commit ahead of local
```
- This shows commits that exist on the remote but not locally
#### 2. See what changed (actual code)

```shell
git diff main origin/main
```

 - This shows line-by-line differences between your branch and the remote
#### 3. Inspect a specific remote commit
From the `git log` output, grab a commit hash:

```shell
git show <commit-hash>
```
### So to do this you:

```shell
git fetch
# git log main..origin/main (optional)
# git show <commit-hash>
git diff main origin/main
```

 > After that you decide what to do!

**If it looks good:**

```shell
git merge origin/main
```

This is the output:
![[Pasted image 20260429071750.png]]
## **`git pull`**
Okay now back to `git pull`, if you understand the concept of `git fetch`, what this basically does is **git fetch + git merge** both at the same time, but there's a problem:

git pull fails in your case because Git doesn’t know how you want to combine two different histories.

So Git is like:

- “Do I merge them, or rebase them? You didn’t tell me.”

So you do `git pull --rebase origin main` aaand you're done! 

now do your `git push` And there should be no errors.

`rebase` basically means:
	Put my commit that already exists aside, **fetch** the commits that are ahead of me on the remote repository and then get back the commit that I have. 

So, if you have changes made on the remote "Fix typo in readme" and you have "Add style.css" in the local commit, if you `git pull --rebase origin main`

This is the output:
![[Pasted image 20260429070147.png]]

Then you do git push normally!
# Initializing a Repo from a folder 
1. When initializing a repo you firstly cd into the folder you want to push
2. Do the first command `git init` 
3. Then initialize the remote url with 

4. ```bash
   git remote add origin <url with ssh or html>
   git add . 
   git commit -m "Initial commit"
   ```
   
5.  `git push -u origin main` , for this command the **`-u`** is essential for the first push, **`-u` (short for `--set-upstream`)**: is a crucial flag for your first push. It tells Git to link your local branch to the remote branch. Because you used `-u` this time, the _next_ time you have updates, you can simply type `git push` or `git pull` without having to   `origin` here is the name of the remote, we will talk about remotes in [Remotes](#remotes)
# Remotes
When you first initialize the url of the repo with `git remote add origin <url>` you simply tell github that the url you is named **origin**, understanding that you can do some neat stuff!

If you forked a repo for example an open source project you can name:
- **`origin`**: Your personal fork (where you push your changes).
    
- **`upstream`**: The original project (where you pull the latest updates from so you don't fall behind).

```bash
git remote add origin https://github.com/YOUR_NAME/cool-project.git
git remote add upstream https://github.com/ORIGINAL_AUTHOR/cool-project.git
```

you can simply then push the code you want to either the upstream original author code, or the code you forked!
### How to Manage Your Remotes
Once you've added multiple remotes, here is how you interact with them:

- **To see all your connected remotes:** Use the verbose flag to see exactly what nicknames go to what URLs:

 ```bash
git remote -v
 ```

- **To push to your new remote:** Instead of pushing to `origin`, you just swap out the nickname in your command:

```bash
git push something main
```

- **To pull from your new remote:**

```
git pull something main
```


- **To rename a remote later:** If you realize "something" isn't a descriptive enough name:

```bash
git remote rename something staging-server
```