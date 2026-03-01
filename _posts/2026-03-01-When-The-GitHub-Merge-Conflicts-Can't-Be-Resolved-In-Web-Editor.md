---
title: When The GitHub Merge Conflicts Are Too Big To Resolve In The Web Editor
---

Sometimes if you push a bunch of commits to a repo and then try to make a PR for them, GitHub will say something like "The merge conflicts are too big to edit on the web." 
or "Use the CLI to resolve merge conflicts."

GitHub is saying the branch you’re trying to merge (e.g. updates) has diverged from the target branch (e.g. main), and there are conflicting changes. You need to bring the target branch into yours and fix conflicts locally, then push.


Do this on your machine:

**We're assuming that you're working in a branch called 'updates'**

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
  
- Open each file, remove the <<<<<<<, =======, >>>>>>> markers and keep the correct code.
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
