# Backing Up and Sharing

After creating a git repository for the selected projects, it's time to _push_ a copy of the project to a remote repository. This will serve as a backup of the project with its history and will also make it easier for collaborators to work on the project.

## Setting up repository to GitHub

1. Create new repository
2. Name repository
3. Add description
4. Click next
5. Run the commands in Figure 1.0:
6. Refresh page

> **Figure 1.0**

```sh
$ git remote add origin https://github.com/<name>/website.git
$ git push -u origin master
```
- `git remote add origin` sets GitHub as the _remote origin_.
- `git push -u` sets GitHub as the _upstream repository_, which means you are able to download any changes automatically when `git pull` is used later on.
  - With `-u`, you are able to run `git pull` without additional arguments (e.g., remote/branch name).
  - Technically, the `-u` option adds a tracking reference to the upstream server you are pushing to. Otherwise, you'd have to type `git pull origin master` every time.
  - When used later, Git knows that `git pull` actually means `git pull origin master`.
- `origin` is the default remote repository name.
- `master` is the branch name.

> It’s important to note that the example has a repository URL that uses HTTPS, **the secure version** of the HyperText Transfer Protocol [(HTTP)](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol). This is the current GitHub default, but there’s another version that uses [SSH Keys](https://help.github.com/articles/generating-ssh-keys/), which has the advantage of remembering passwords automatically.
>
> HTTPS is simpler to use and configure. The biggest downside is that you have to type your password every time you want to push any changes, which can be inconvenient. Luckily, there are ways to get your computer to remember, or _cache_, your password; see the article [“Caching your GitHub password in Git”](https://help.github.com/en/articles/caching-your-github-password-in-git) for information on how to set this up on your system.




## Git Push

After setting up the repository, any changes to the project directory will be added/commited as usual.

Once all changes has been commited, the local repository will be ahead of the remote repository by **x** amount of commits made.

```sh
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

Now the commit(s) need to be pushed to GitHub. Do so by using the `git push` command in the terminal.

```sh
$ git push
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 660 bytes | 660.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/meo8/website.git
   b8616f9..9ccbbf1  master -> master
```


## Summary

| Command | Description | Example |
|-|-|-|
| git remote add | Add remote repo | $ git remote add origin |
| git push -u `<loc>` `<br>` | Push branch to remote | $ git push -u origin master |
| git push | Push to default remote | $ git push |
