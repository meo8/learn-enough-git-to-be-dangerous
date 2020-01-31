# Branching and Merging

One of the most powerful features of Git is its ability to make _branches_, which are effectively **complete self-contained copies of the project source**. Together with the ability to _merge_ one branch into another, thereby incorporating the changes into the original branch.

The best thing about a branch is that you can make your changes to the project in **isolation from the master copy of the code**, and then merge your changes in only when they’re done.

This is especially helpful when collaborating with others; having a separate branch lets you make changes independently from other developers, reducing the risk of accidental conflicts.


<br>


## Creating a new branch

**Instance:** You currently have an index.html page, now you want to create a new branch called _about-page_ to work on a new HTML page for your 'About' section of your website.

**1.** `git checkout -b about-page` makes a new branch called _about_-page and checks it out at the same time.

  ```sh
  $ git checkout -b about-page
  Switched to a new branch 'about-page'
  ```

**2.** Visual representation of [current repository structure](https://softcover.s3.amazonaws.com/636/learn_enough_git/images/figures/git_branch.png).
   - The main repository evolution is a series of commits.
   - The branch represents a copy of the repo **at the time the branch was made**.

**3.** `git branch` lists all branches currently defined on the local machine, with an asterick `*` indicating the currently checked-out branch.

  ```sh
  $ git branch
  * about-page
    master
  ```

> How to list _remote_ branches in [later notes](https://www.softcover.io/read/28fdb94f/learn_enough_git/collaborating#sec-pushing_branches).



<br>


## Deleting a branch

**1.** To delete a branch, use `git branch -d <branch-name>`.

**2**. Use `git branch` to confirm the branch is no longer listed.

```sh
$ git branch -d about-page
Deleted branch about-page (was 4f99127).

$ git branch
 * master
```

> By design, `-d` option will not work to delete a branch _unless_ its changes/commits has been **fully merged**.

**3**. To delete an unmerged branch, use the option `-D`.

```sh
$ git branch -D test-branch
Deleted branch test-branch (was e5f90b8).
```

<br>


### Notes about branching

  - Commits made in _any_ branch will all appear when using `git log`
    - The difference will be the origin/branch name.
    - It will appear next to the SHA indicating where the commits were made.


  ```sh
  $ git log
  commit 8b02e12bb8407148f1fb8faeaf8e25df689aef29 (HEAD -> about-page)
  Author: Meo Huynh <meoh92@gmail.com>
  Date:   Wed Jan 29 15:35:55 2020 -0800

  Add About page

  commit b9956566089ba5ad95a4885bcda634da07485aa5 (master)
  Author: Meo Huynh <meoh92@gmail.com>
  Date:   Wed Jan 29 14:01:13 2020 -0800

  Add image

  commit 9ccbbf1fcab08aad6710ca41188c66992c7b55b8 (origin/master)
  Author: Meo Huynh <meoh92@gmail.com>
  Date:   Wed Jan 29 12:17:19 2020 -0800

  Add README file
  ```

<br>
<br>


## Git diff `<branch-1>` `<branch-2>`

When you're ready to merge one branch to another, use `git diff` to get a handle on which changes will be merged.

In [Section 1.4](https://www.softcover.io/read/28fdb94f/learn_enough_git/getting_started#sec-viewing_the_diff), this command can be used by _itself_ to see the difference between unstaged changes and our last commit, but the same command can be used to show diffs between branches.

This can take the form `git diff <branch-1> <branch-2>`.
> **Note: if you leave the branch unspecified Git automatically diffs against the current branch.**
>
> **Instance:** If you're currently in the **test-page** branch and run `git diff master`, Git will know to diff that _current branch_ (test-branch) against the **master** branch.
>
```sh
Meo (test-page) website
$ git diff master
 diff --git a/meow.txt b/meow.txt
 new file mode 100644
 index 0000000..d99b416
 --- /dev/null
 +++ b/meow.txt
 @@ -0,0 +1 @@
 +TESTINGGGGG
```


<br>
<br>


## Merge to master branches

**Instance**: You want to merge your _about-page_ branch into your _master_ branch.

> Before merging, checkout to the branch that you want other branches to merge into.

**1.** Checkout to master branch using `git checkout master`.
```sh
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
```

  > Note that, unlike the checkout command in [Listing 3.3](https://www.softcover.io/read/28fdb94f/learn_enough_git/intermediate_workflow#code-checkout_about_page), here we omit the `-b` option because the **master** branch already exists.


**2.** Merge the two branches using `git merge <branch-to-merge-into-master>`.

  ```sh
  $ git merge about-page
  Updating b995656..4f99127
  Fast-forward
   about.html | 14 ++++++++++++++
   index.html |  1 +
   2 files changed, 15 insertions(+)
   create mode 100644 about.html
```


**3.** At this point, the repository's branch structure should look something like [Figure 3.10](https://softcover.s3.amazonaws.com/636/learn_enough_git/images/figures/about_page_merged.png).

**4.** Having merged in the changes, we can sync up the local master branch with the version at GitHub (called **origin/master**) using `git push`; use `git status` to check that everything is up to date.

```sh
$ git push
Enumerating objects: 13, done.
Counting objects: 100% (13/13), done.
Delta compression using up to 4 threads
Compressing objects: 100% (10/10), done.
Writing objects: 100% (11/11), 180.11 KiB | 8.58 MiB/s, done.
Total 11 (delta 4), reused 0 (delta 0)
remote: Resolving deltas: 100% (4/4), completed with 1 local object.
To https://github.com/meo8/website.git
   9ccbbf1..4f99127  master -> master

$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```


<br>


### Things to note

**Instance:** The master branch didn’t have any changes/commits while we were working on the about-page branch.

Git excels even when the original branch has changed in the interim. This situation is especially common when collaborating with others [(Chapter 4)](https://www.softcover.io/read/28fdb94f/learn_enough_git/collaborating#cha-collaborating), but can happen even when working alone.

Suppose, for example, that we discovered a typo on master and wanted to fix it and push up immediately. In that case the master branch would change to look like [(Figure 3.11](https://softcover.s3.amazonaws.com/636/learn_enough_git/images/figures/master_branch_change.png)), but we could still merge in the topic branch as usual.

There is a possibility that changes on master would conflict with the merged changes, but Git is good at automatically merging content. Even when conflict is unavoidable, Git is good at marking conflicts explicitly so that we can resolve them by hand. We’ll see a concrete example of this in [Section 4.2](https://www.softcover.io/read/28fdb94f/learn_enough_git/collaborating#sec-merge_conflicts).



### Rebasing – another way to merge

The most common way to combine branches is `git merge`, but there’s a second method called `git rebase` that you’re likely to encounter at some point.

**Advice for now:** ignore `git rebase`. The differences between merging and rebasing are subtle, and conventions for using rebase differ, so use `git rebase` only when an advanced Git user tells you to; otherwise, use `git merge` to combine the contents of two branches.


<br>
<br>


## Recovering from errors

One of the most useful features of Git is its ability to let us recover from errors that would otherwise be catastrophic. The error-recovery techniques can be dangerous, so always implement them with care.

**Instance:** You made an unintentional change to a project and want to get back to the state of the repository as of the most recent commit (a state known as **HEAD**). Your goal was to append a new line break to the _about.html_ page using `echo`and the append operator, `>>` . Instead of running `echo >> about.html`, you accidently left out one of the two angle brackets and ran `echo > about.html` instead, thus, using the _redirect operator_ instead.

In a regular [Unix directory](https://www.learnenough.com/r/learn_enough_command_line/directories#cha-directories), you effectively wiped out the entire content of the _about.html_ page.

However, in a Git repository we can undo the changes by forcing the system to check out the most recently committed version.

**Current Instance:** about.html has been wiped.

**1.** Check that about.html has changed using `git status`.

  ```sh
  $ git status
  On branch master
  Your branch is up to date with 'origin/master'.

  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
  	modified:   about.html

  no changes added to commit (use "git add" and/or "git commit -a")
  ```

   - This doesn’t indicate the scope of the damage, though, which we can inspect using `git diff`.
   - Those minus signs indicate that all of the lines of content are now gone.

    ```sh
    $ git diff
     diff --git a/about.html b/about.html
     index b8b66bd..8b13789 100644
     --- a/about.html
     +++ b/about.html
     @@ -1,14 +1 @@
     -<!DOCTYPE html>
     -<html lang="en" dir="ltr">
     -  <head>
     -    <meta charset="utf-8">
     -    <title>About Us</title>
     -  </head>
     -  <body>
     -    <h1>About</h1>
     -    <p>
     -      This site is a sample project for the <strong>awesome</strong> Git
     -      tutorial <em>Learn Enough™ Git to Be Dangerous</em>.
     -    </p>
     -  </body>
     -</html>
     +
    ```

<br>


**2.** We can undo these changes by passing the `-f` (force) option to checkout, which forces Git to check out **HEAD**:
  - `git checkout -f` to undo changes.
  - The command `git reset --hard HEAD` is equivalent, but the version with checkout may be easier to remember.

  ```sh
  $ git checkout -f
  Your branch is up to date with 'origin/master'.
  ```
  > `--force` equivalent to `-f` (long form vs short form).
  >
  > `git checkout -f` itself is potentially dangerous, as it wipes out **all** the changes made, so use this trick only when you’re **100% sure** you want to revert to **HEAD**.


**3.** Confirm that the about.html page has been restored using `git status`.

```sh
$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```


> The status “working tree clean” indicates that there are no changes.


### Error recovery through branch

Another source of robustness against error is using branches; because changes made on one branch are isolated from other branches, you can always just delete the branch if things go horribly wrong.


### Error recovery using SHA

A final example of recovering from error involves the common case of a bug or other defect that makes its way into a project, origins unknown. In such a case, it’s convenient to be able to check out an earlier version of the repository

> The most powerful way to track down such errors is `git bisect`. This advanced technique is covered in the [Git documentation](https://git-scm.com/docs/git-bisect).

The way to do this is to use the SHAs from the Git log. For example, to restore the website project to the state right after the second commit, we would run `git log` and navigate to the beginning of the log.

> `git log` uses the less interface – get to the beginning of the log by typing `G` to go to the last line of the log.

<br>

**1.** To check out a specific commit, simply copy the SHA and check it out:

  - `git checkout <SHA>.`

```sh
$ git checkout 03aff34ec4f9690228e057a4252bcca169a868b4
Note: checking out '03aff34ec4f9690228e057a4252bcca169a868b4'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b new_branch_name

HEAD is now at 03aff34... Add content to index.html
Meo ((03aff34...)) website $
```

> Note that the branch name in the last line has changed to reflect the value of the SHA, and Git has issued a warning that we are in a ‘detached HEAD’ state.
>
> Use this technique to inspect the state of the project and figure out any necessary changes, then check out the master branch to apply them.

**2.** `git checkout master`
```sh
Meo ((03aff34...)) website $ git checkout master
Meo ((03aff34...)) website $
```

- At this point, you could switch to your text editor and make any necessary changes (e.g., fixing a bug discovered on the earlier commit).

**3.** Main takeaways:
- It’s possible to “go back in history” to view the project at an earlier state.
- It’s tricky to make changes, so if you find yourself doing anything complicated you should ask a more experienced Git user what to do.
    - In particular, the exact practices in such a case could be team-dependent.


<br>


## Ignoring files

A frequent issue when dealing with Git repositories is coming across files you don’t want to commit. These include files containing secret credentials, configuration files that aren’t shared across computers, temporary files, log files, etc.

For example, on macOS a common side-effect of using the Finder to open directories is the creation of a hidden file called **.DS_Store**. This file shows up in `git status` to be tracked.

This is annoying, as we have no need to track this file, and when collaborating with others, it could easily cause conflicts down the line.

In order to avoid this annoyance, Git lets us ignore such files using a special hidden configuration file called **.gitignore**. To ignore .DS_Store, create a file called **.gitignore** using your favorite text editor and fill it with files and directories you want to ignore.

- Use `*` as a wildcard.
  - ex) `*.txt`
- List a directory to ignore all of its contents.
  - ex) `temp/`


<br>
<br>


## Summary

| Command | Description | Example |
|--|--|--|
| .gitignore | Tell Git which things to ignore | $ echo .DS_store >> .gitignore |
| git checkout `<branch>` | Check out a branch | $ git checkout master |
| git checkout -b `<branch>` | Check out & create a branch | $ git checkout -b about-page |
| git branch | Display local branches | $ git branch |
| git merge `<branch>` | Merge in a branch | $ git merge about-page |
| git branch -d `<branch>` | Delete branch (if merged) | $ git branch -d about-page |
| git branch -D `<branch>` | Delete branch (even if unmerged) (dangerous) | $ git branch -D other-branch |
| git checkout -f | Force checkout, discarding changes (dangerous) | $ git add -A && git checkout -f |
