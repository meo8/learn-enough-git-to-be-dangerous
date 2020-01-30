# Getting Started

## Intro

Git is a version control system that's broadly. RCS, CVS and Subversion led to Git; there are other alternatives such as Perforce, Bazaar, and Mercurial.

Version control systems like Git automatically track the changes within a project including any versions of the project along with all files and directories. Developers can add new code and fix bugs without disrupting the main development until the new code and issues have been fully resolved.

The projects are securely backed up along with any history. Developers can also collaborate with others. Version control also makes deploying websites and web applications much easier.

### Need Help?
- `git help <command-name>` works like `man <command-name>`
  - ex) `git help add` will show a manual for git's `add` command




### Global Setup
Users only need to do this once per computer. These configuration settings allow Git to identify your changes by name and email address, which is especially helpful when collaborating with others.

- `git config --global user.name "Your Name"`
- `git config --global user.email your.email@example.com`


> The name and email you use will be viewable in any projects you make public, so don’t expose any information you’d rather keep private.




### Hash / SHA
  - Unique numbers and letters generated by the Secure Hash Algorithm.
  - Generated each time a new commit is created.
  - ex) `20e5cce5543368c4a6c8b5cc237cf1d2050611af`




### Initialize, add, and commit repo
In order to use git on a project, the project must first be initialized.

- Go to the project's home directory (the folder that contains all of the projects files).
- Use the command `git init` within the terminal.
  ```sh
  $ `git init`
  $ Initialized empty Git repository in /Users/mhartl/repos/website/.git/
  ```
- Use `git status` to check for untracked files.
- Use `git add <filename>` to add untracked files to be staged.
  - `git add -A` to add **all** untracked files.
  - `git add . ` to add all files in **current** directory.
- Use `git commit -am <message>` to commit the staged files.
  - `-a` option arranges to commit all the changes in currently existing files and includes changes only to files already added to the repository.
  - `-m` option is used to include a message indicating the purpose of the commit.
    - Write in the present tense using the [imperative mood](https://en.wikipedia.org/wiki/Imperative_mood).
- Use `git log` to see the records of the commit.
  ```sh
  $ git log
  commit 20e5cce5543368c4a6c8b5cc237cf1d2050611af (HEAD -> master)
  Author: Meo Huynh <meoh92@gmail.com>
  Date:   Tue Jan 28 14:21:58 2020 -0800

      Initialize repository
  ```
- The `git log` command shows only the commit messages, which makes for a compact display but isn’t particularly detailed.
- `git log -p` shows the full diffs represented by each commit.



### Viewing the diff
It’s often useful to be able to view the changes represented by a potential commit before making it.

> Note that `git diff` only works on files and directories that Git tracks.

- `git diff` By default, just shows the difference between the last commit and unstaged changes in the current project:
   ```sh
    $ git diff index.html
    diff --git a/index.html b/index.html
    index 3b18e51..393614f 100644
    --- a/index.html
    +++ b/index.html
    @@ -1 +1,2 @@
     hello world
    +hello, again world
    ```

- `git diff --staged` To see the difference between staged changes and the previous version of the repo, use git diff --staged.)
  ```sh
  $ git diff --staged
  diff --git a/index.html b/index.html
  index 3b18e51..393614f 100644
  --- a/index.html
  +++ b/index.html
  @@ -1 +1,2 @@
   hello world
  +hello, again world
  ```




## Summary

| Command | Description | Example |
|---|---|---|
| git help | Get help on a command |$ git help push |
| git config | Configure Git |$ git config --global … |
| source `file` | Activate Bash changes |$ source ~/.bash_profile |
| mkdir -p | Make intermediate directories as necessary |$ mkdir -p repos/website |
| git status | Show the status of the repository |$ git status |
| touch `name` | Create empty file |$ touch foo |
| git add -A | Add all files or directories to staging area |$ git add -A |
| git add `name` | Add given file or directory to staging area |$ git add foo |
| git commit -m | Commit staged changes with a message |$ git commit -m "Add thing" |
| git commit -am | Stage and commit changes with a message |$ git commit -am "Add thing" |
| git diff | How diffs between commits, branches, etc. |$ git diff |
| git commit --amend | Amend the last commit |$ git commit --amend |
| git show `SHA` | Show diff vs. the SHA |$ git show fb738e… |




Notes taken from [Learn Enough Git To Be Dangerous](https://www.learnenough.com/git-tutorial/getting_started) by Michael Hartl.