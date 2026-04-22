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

*Note:* If you have a `.gitignore` file make sure to mv it as well!