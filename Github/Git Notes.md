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

    ```bash
git pull something main
    ```

- **To rename a remote later:** If you realize "something" isn't a descriptive enough name:

	```bash
git remote rename something staging-server
    ```