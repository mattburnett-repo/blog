---
title: When The GitHub Merge Conflicts Are Too Big To Resolve In The Web Editor
---

**(We're assuming that you're working in a branch called 'updates')**

Sometimes if you push a bunch of commits to a repo and then try to make a PR for them, there will be too many merge conflicts. GitHub will say something like "The merge conflicts are too big to edit on the web." 
or "Use the CLI to resolve merge conflicts."

GitHub is saying the branch you’re trying to merge (e.g. updates) has diverged from the target branch (e.g. main), and there are conflicting changes. You need to bring the target branch into yours and fix conflicts locally, then push.

This means you 
- go to your working branch (again, for our example here it's called 'updates')
- get the 'target' branch (usually called 'main')
- merge the 'target' into your working branch
- list all of the conflicted files and fix them.
- commit and re-push

Here's how:

- Fetch the latest from GitHub
  ``` bash
   git fetch origin
  ```
- Make sure you’re on your PR branch
  ``` bash
   git checkout updates
  ```
- Merge the target branch into yours
  ``` bash
   git merge origin/main
  ```

- Run 'git status' to see conflicted files.
  
- Open each file, remove the <<<<<<<, =======, >>>>>>> markers and keep the correct code.  This often is a simple click of "Accept current change", but make sure you're removing the code that should go away, and keeping the code that should stay.
  
- Don't.Forget.To.Save.The.Files.
  
- Then:
  ``` bash
    git add .
    git commit -m "Resolve merge conflicts with main"
  ```
  
- Push your branch
  ``` bash
   git push origin updates
  ```
  
- After that, refresh the PR on GitHub; the conflict message should be gone and you should be able to complete the PR.
