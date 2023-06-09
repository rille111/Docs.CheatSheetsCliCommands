# GIT REBASING

When having started a rebase - Never commit anything here, just fix conflicts and continue rebasing.

* git checkout main; git pull
* git checkout feature/myfeature
* git rebase main
Conflicts happens..
* git mergetool
* git rebase --continue

When rebasing on main (as the base), and having merge conflict
* LOCAL (source) is: your new branch
* REMOTE (target) is: is main (the base)

You will probably get this conflicts pop up even if there seems to be equal code.
* If so - then just accept LOCAL changes, save and close with your mergetool.

When having real conflicts:
* Most often, just pick LOCAL
* Save and close the file