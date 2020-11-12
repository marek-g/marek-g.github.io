---
layout: page
title: GIT
parent: Tips & Tricks
---

# GIT

## Useful commands

Here is a list of most useful GIT commands for me when working with projects on github.

Get latest changes from origin:

```sh
git pull / git pull origin
```

Update fork on github:

```sh
git push myfork
```

Create new branch:

```sh
git branch mybranch / git checkout -b mybranch
```

Update branch on github:

```sh
git push myfork mybranch
```

Rebase branch to master:

```sh
git checkout mybranch
git rebase master
```

Remove branch:

```sh
git branch -d mybranch
git push myfork :mybranch
```

### Bundles

Create bundle:

```sh
git bundle create repo.bundle master
```

Create bundle from last 10 days:

```sh
git bundle create repo.bundle --since=10.days master
```

Create repository from bundle:

```sh
git clone repo.bundle -b master repo
```

Verify the bundle:

```sh
git bundle verify repo.bundle
```

Pull from the bundle:

```sh
git pull repo.bundle master
```

## Branching

This post describes very useful branching model that can be used with GIT:

http://nvie.com/posts/a-successful-git-branching-model/

## Converting between tabs and spaces

It is very easy to convert between tabs and spaces automatically when committing and check-outing text files with GIT. GIT allows it with the attributes mechanism:

http://git-scm.com/book/ch7-2.html

http://stackoverflow.com/questions/2316677/can-git-automatically-switch-between-spaces-and-tabs

To configure this behavior all you need is:

1. Place 'expand' and 'unexpand' commands in your 'PATH' environment variable. For Windows they are part of GnuWin32 CoreUtils package:
        http://gnuwin32.sourceforge.net/packages.html

2. Define needed GIT filter names. These are examples if you want tabs locally and two or four spaces in the repository:
   - `git config --system filter.tabspace2.clean "expand --tabs=2 --initial"`
   - `git config --system filter.tabspace2.smudge "unexpand --tabs=2 --first-only"`
   - `git config --system filter.tabspace4.clean "expand --tabs=4 --initial"`
   - `git config --system filter.tabspace4.smudge "unexpand --tabs=4 --first-only" `

3. Apply settings to your local repository.
   - Create .git/info/attributes file inside your local repository directory and add following line(s) to it: `*.<extension> filter=tabspace2` where `<extension>` is file extension than will be filtered.

4. If you have any changes, commit them and then checkout all files to have filtered version on a disc: `git checkout HEAD -- **`
