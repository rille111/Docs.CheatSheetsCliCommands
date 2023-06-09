## A common feature branch flow

The short version:

* `git checkout -B feature/mystuff` Check out a separate branch
* `git push` Push that branch to the origin, to be safe
* .. make changes 
* `git add . --all` Stage your changes to the coming commit
* `git commit --all -m "Fixed typo in shoppingcart"` Create a commit
* `git checkout master` Switch back to master
* `git pull --rebase` Get the latest changes, using rebase instead of creating a merge commit
* `git checkout feature/mystuff` Switch back to your branch
* `git rebase master` Rebase unto master, fix potential conflicts by adding changes commit by commit (NOTE: DO NOT CREATE ANY COMMITS, ONLY FIX CONFLICTS AND ADD TO STAGING!)
* `git push -f` Since you rewrote the history, you need to force push your updates to the origin
* `git checkout master` Switch back to master
* `git merge feature/mystuff --no-ff` Merge into master, keeping a merge-commit as a summary of your branch
* `git push` And we're done!

More details about this flow:

## Avoid merge commits that result from git pull, using git pull --rebase instead

When you want to push your changes to a branch, but someone else already pushed before you, you have to pull in their changes first. Normally, git does a merge commit in that situation.
You can avoid this by doing a rebase (putting your commits AFTER the latest master):

* `git pull --rebase` Do it manually
* `git config --global --bool pull.rebase true` Or configure your .gitconfig to always to this so you don't forget

References:

* http://kernowsoul.com/blog/2012/06/20/4-ways-to-avoid-merge-commits-in-git/
* https://mislav.net/2013/02/merge-vs-rebase/

## Prefer recording a merge commit when a feature lands into master

Say you have worked on a feature branch with a number of commits. You want to merge into master, and keep that summarizing merge commit for easier review.
This is called a merge without fast-forward. Fast-forward creates a linear history, whereas No fast-forward keeps your feature commits in its own line, and making a merge commit instead.

* `master > git merge feature/mystuff --no-ff`

## Prefer keeping your feature synced from the master often

To avoid lengthy merge conflicts and to handle small changes instead of larger changes that can break your stuff.

* `git checkout master` Switch back to master
* `git pull --rebase` Get the latest changes, using rebase instead of creating a merge commit
* `git checkout feature/mystuff` Switch back to your branch
* `git rebase master` Rebase unto master, fix potential conflicts by adding changes commit by commit (NOTE: DO NOT CREATE ANY COMMITS, ONLY FIX CONFLICTS AND ADD TO STAGING!)
* `git push -f` Since you rewrote the history, you need to force push your updates to the origin

## Prefer keeping small commits

So that: 
* it's easier to handle merge conflicts
* it's easier to cherrypick
* it's easier to create reverse commits

## Commit messages best practices

* Short summary
* In the format: Imperative presens
* First letter is capital
* Do not end in dot

Example: `Fix typo in shoppingcart`
References:

https://chris.beams.io/posts/git-commit/