# Useful git commands

## Useful materials for learning git
- This is a website for learning git interactively: https://learngitbranching.js.org/

## Data structure used in Git
- The git is an implementation of linked lists

## Useful commands

### The difference between git merge and git rebase
- `git merge` and `git rebase` can both combine works from different branches
- While for `git merge`, it would create an extra commit, and the commit have two parents
  ![Before merge](/images/merge_1.png){: style="width:100px"}

  ![After merge](/images/merge_2.png){: style="width:100px"}
- `git rebase` takes a set of commits, "copies" them, and plop them down somewhere else, the advantage of rebasing is that it can be used to make a nice linear sequence of commits. The commit log / history of the repository will be a lot cleaner if only rebasing is allowed.
  ![Rebase](/images/rebase_1.png){: style="width:100px"}



