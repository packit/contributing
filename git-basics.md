# Basics of the git workflow

These are the basics for how to use git to get you easily started with contributing. We suggest going through the official git manual as a follow-up:
[Pro Git](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository)

## How to clone a repository and set the remotes

Create a fork of the repository and then:

    $ git clone git@github.com/YOUR_USERNAME/REPO.git
    $ cd REPO
    $ git remote add upstream git@github.com:packit/REPO.git

At this point you should have:

    $ git remote -v

    origin  git@github.com:YOUR_USERNAME/REPO.git (fetch)
    origin  git@github.com:YOUR_USERNAME/REPO.git (push)
    upstream        git@github.com/packit/REPO.git (fetch)
    upstream        git@github.com/packit/REPO.git (push)

## How to create a new branch

    $ git checkout -b my-new-feature

Pushing this to your fork for the first time is:

    $ git push -u origin my-new-feature

## How to rebase your branch

If there are new changes added to `upstream/main` branch, you need to rebase your branch (to keep our git history clean and
easy to navigate we rebase our branches before merging).

You need to fetch the changes from upstream:

    $ git fetch upstream

And then do the rebase:

    $ git rebase upstream/main

If there are any conflicts, you need to resolve them manually in the editor (git will tell you this)
and then follow the steps in the output of the command (e.g. after resolving the conflicts `git rebase --continue`).
After the rebase, history of the branch is rewritten. Therefore, if the branch is already pushed, `git status` will tell
you that the local branch has diverged from the remote branch and you will need to force push the changes (`git push -f`).

## Suggested configuration options

    $ git config --global pull.rebase true
    $ git config --global rebase.autoStash true
