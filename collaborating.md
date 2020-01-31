# Collaborating

Git’s greatest strength is **making it easier to collaborate with other people**. This is especially the case when using repository hosts like GitHub or Bitbucket, but it is also possible to host Git repositories on private servers (sometimes using software like GitLab to get many GitHub-like benefits).

There are many different collaboration scenarios, and they vary significantly by team and by project.

**Instance:** We'll focus on the important case of multiple collaborators who all have commit rights to a particular repo. This model is appropriate for teams where everyone can make changes without explicit approval from a project maintainer.

Open-source projects typically use a different flow involving **forking** and **pull requests**, but the details differ enough that it’s best to defer to the collaboration instructions of each particular project. Consider, for example, the instructions for contributing to Ruby on Rails. With the commands from this tutorial and your technical sophistication, you’ll be in a good position to understand and follow such instructions if you decide to get involved in contributing to open-source software or other projects under version control with Git.


## Clone, push, pull

**Instance:** As an example of a common collaboration workflow, we’ll simulate the case of two developers working on the same project, in this case the simple website developed in this tutorial by fictional developers named Alice and Bob. Alice is working in the original **website** directory, and we’ll create a second directory (**website-copy**) for her collaborator, Bob.

#### Alice's tasks

1. Alice must first use `git push` to make sure all her changes are on the remote repository.

2. In real life, Alice would now need to add Bob as a collaborator on the website repository.
  - Do so by going to GitHub and click on **Settings > Collaborators**.
  - Put Bob’s **GitHub username** in the **Add collaborator** box [(Figure 4.3)](https://softcover.s3.amazonaws.com/636/learn_enough_git/images/figures/github_add_collaborator.png).
  - However, in this instance, we're collaborating with ourselves, so we can skip this step.


#### Bob's tasks

1. Once Bob gets the notification that he’s been added to the **website** repository.
   - He can go to GitHub to get the clone URL.
   - This URL lets Bob make a full copy of the repository (including its history) using `git clone`.

2. Create a repository to store the cloned repository.
3. Clone the repository using `git clone <clone URL> <optional-argument>`
   - `git clone https://github.com/meo8/cat-clicker.git`
     - This will clone the repository with all of its directories and files along with its default namings.
   - `git clone https://github.com/meo8/cat-clicker.git my-personal-cat-clicker`
     - This allows you to use a different name than the original repo.
4. Make changes to the files.
5. `git commit -am 'Add content to about page'`
6. `git push` to push the changes to GitHub
7. At this point, Bob might send Alice a notification that there’s a change ready, or Alice might just be diligent about checking for changes. In either case, Alice can get the changes from the remote origin by running `git pull`.
8. Refresh browser and run `git log -p` to confirm the repo has been properly updated.
9. If there's a chance Alice made other changes, Bob should also run a `git pull` to stay up to date.

## Pulling and merge conflicts

**Instance:** So far, Alice didn’t make any changes while Bob was making his commit, so there was no chance of conflict, but this is not always the case. In particular, when two collaborators edit the same file, it is possible that the changes might be irreconcilable. Git is pretty smart about merging in changes, and in general conflicts are surprisingly rare, but it’s important to be able to handle them when they occur. In this section, we’ll consider both cases in turn.

### Non-conflicting changes

**Instance:** We’ll start by having Alice and Bob make non-conflicting changes in the same file.

##### Alice's tasks

1. Alice decides to change the top-level heading on the About page from “About” to “About Us”.
2. After making this change, Alice commits and pushes as usual.
   - `git commit -am 'Change page heading'`
   - `git push`

##### Bob's tasks

1. Bob decides to add an image to the About page.
2. He downloads it using `curl`
   - `curl -o images/polar_bear.jpg \-OL https://cdn.learnenough.com/polar_bear.jpg`
   - `-o <file>` outputs the image to the `<file>` directory.
   - `images/polar_bear.jpg` names it polar_bear.jpg; it could be named something else.
   - The `\` means line continuation.
3. Having made his changes, Bob commits as usual.
   - `git add -A`
   - `git commit -am "Add an image"`
4. When he tries to push the commit to the remote repository (GitHub), an error occur:

  ```sh
  [website-copy (master)]$ git push
  To https://github.com/mhartl/website.git
   ! [rejected]        master -> master (fetch first)
  error: failed to push some refs to 'https://github.com/mhartl/website.git'
  hint: Updates were rejected because the remote contains work that you do
  hint: not have locally. This is usually caused by another repository pushing
  hint: to the same ref. You may want to first integrate the remote changes
  hint: (e.g., 'git pull ...') before pushing again.
  hint: See the 'Note about fast-forwards' in 'git push --help' for details.
  ```
  - Because of the changes Alice already pushed, Git won’t let Bob’s push go through. As indicated by the highlighted line above, the solution to this is for Bob to pull before trying to push again:
  - `git pull`


#### Merging the conflicts

Even though Alice made changes to about.html, there is no conflict because Git figures out how to combine the diffs. In particular, `git pull` brings in the changes from the remote repo and **uses merge to combine them automatically**, adding the option to add a commit message by dropping Bob into the default editor, which on most systems is Vim.

1. After Bob runs `git pull`, an editor will appear.
2. To get the merge to go through, simply quit out of Vim using `q` or close the editor.
3. We can confirm that this worked by checking the log `git log`.
   - It shows both the merge commit (86dccd...) and Alice’s commit (5ca69e...) from the original copy:

    ```sh
    [website-copy (master)]$ git log
    commit 86dccde63ac15331a068ce79fa9c83d8b784b28b
    Merge: 9b7eda1 5ca69e4
    Author: Michael Hartl <michael@michaelhartl.com>
    Date:   Mon Dec 28 13:14:44 2015 -0800

        Merge branch 'master' of https://github.com/mhartl/website

    commit 9b7eda1b0a95740a241684b82d4474aa8f16ae45
    Author: Bob <bob@michaelhartl.com>
    Date:   Mon Dec 28 13:13:37 2015 -0800

        Add an image

    commit 5ca69e4dca9487b5cd7e1be52222c5389392527d
    Author: Alice <alice@michaelhartl.com>
    Date:   Mon Dec 28 13:02:42 2015 -0800

        Change page heading
    ```

> After Bob finish his tasks of pulling and merging, he can now push his changes to the remote repo.
> Alice can then `git pull` Bob's changes to her local repo.




### Conflicting changes

Even though Git’s merge algorithms can often figure out how to combine changes from different collaborators, sometimes there’s no avoiding a conflict.

**Instance:** Both Alice and Bob notice that the required alt attribute is missing from an image on the website and both decide to correct the issue by adding one.

1. Alice goes into the html and adds "Breaching whale" to the alt attribute, then commits and push as usual; everything goes through successful.

2. Bob then goes into his html and adds "Whale" to the same image's alt attribute, he then commits and tries to push the commit to the remote repo, but the same error as above in the 'non-conflicting changes' section occur.

   - Which means he should follow the same steps and run `git pull`, however, he's met with a different error:

    ```sh
    $ git pull
    remote: Counting objects: 3, done.
    remote: Compressing objects: 100% (1/1), done.
    remote: Total 3 (delta 2), reused 3 (delta 2), pack-reused 0
    Unpacking objects: 100% (3/3), done.
    From https://github.com/mhartl/website
       5ca69e4..7ada3b5  master     -> origin/master
    Auto-merging index.html
    CONFLICT (content): Merge conflict in index.html
    Automatic merge failed; fix conflicts and then commit the result.
    [website-copy (master|MERGING)]$
  ```

  - As indicated on line 141 - 143, Git has detected a merge conflict from Bob’s pull, and his working copy has been put into a special branch state called **master|MERGING**.

3. Bob can see the effect of this conflict by viewing index.html in his text editor. Supposing Bob prefers Alice’s more descriptive alt text, he can resolve the conflict by deleting all but the line with alt="O Great Breaching Whale".

  ```
  <!DOCTYPE html>
  <html lang="en" dir="ltr">
    <head>
      <meta charset="utf-8">
      <title>A whale of greeting</title>
    </head>
    <body>
      <h1>Hello, world</h1>
      <a href="about.html">About this project</a>
      <p>Call me Meo</p>
  <<<<<<< HEAD
      <img src="images/breaching_whale.jpg" alt="Gorgeous Breaching Whale">
  ||||||| bad2d77
      <img src="images/breaching_whale.jpg" alt="Breaching whale">
  =======
      <img src="images/breaching_whale.jpg" alt="O Great Breaching Whale">
  >>>>>>> 67bc739c4011b8804dfed2be692e428eeaef4de8
    </body>
  </html>
  ```

4. After saving the file, Bob can commit his change, which causes the prompt to revert back to displaying the master branch, and at that point he’s ready to push.
5. After Bob runs `git push`, Alice’s and Bob’s repos now have the same content, but it’s still a good idea for Alice to pull in Bob’s merge commit.

> Because of the potential for conflict, it’s a good idea to do a git pull before making any changes on a project with multiple collaborators.
>
> Even then, on a long enough timeline some conflicts are inevitable, and they can be handled with the techniques used in this section.


<br>
<br>


## Pushing branches

**Instance:** We’ll apply our newfound collaboration skills to get Alice to request a bugfix from Bob, who will make the correction and then share the result with Alice. In the process, we’ll learn how to collaborate on branches other than master.

Say, the trademark character ™ is currently broken on the About page and Alice suspects the fix for this involves adding some markup to the HTML template for the website’s pages, but she’s already agreed to attend a tea party.

##### Alice's tasks

**1.** Alice only has time to add a couple of HTML comments requesting for Bob to add the relevant fix.

  ```
  <!DOCTYPE html>
  <html>
    <head>
      <title>About Us</title>
      <!-- Add something here to fix trademark -->
    </head>
    .
    .
    .
  </html>
  ```


**2.** Alice has decided to follow a common convention and use a separate branch for the bugfix, which in this case she calls **fix-trademark**:
  - `git checkout -b fix-trademark`

> This shows something important: it’s possible to make changes to the working directory before creating a new branch, **as long as those changes haven’t been committed yet**.


**3.** Having made the new branch for the fix, Alice can make a commit and push up the branch using git push:

  ```sh
  $ git commit -am 'Request for TM trademark fix'
  $ git push -u origin fix-trademark
  ```

- Here Alice has used the same push syntax used to push the local repo to GitHub in the first place, with fix-trademark in place of master.


##### Bob's tasks

**1.** Alice should send Bob a note before she heads off to her tea party, Bob will know to do a git pull to pull in Alice’s changes:

  ```sh
  $ git pull
  remote: Counting objects: 4, done.
  remote: Compressing objects: 100% (1/1), done.
  remote: Total 4 (delta 3), reused 4 (delta 3), pack-reused 0
  Unpacking objects: 100% (4/4), done.
  From https://github.com/mhartl/website
   * [new branch]      fix-trademark -> origin/fix-trademark
  Already up-to-date.
  ```

**2.** Bob can check his local working directory for the **fix-trademark** branch that Alice created and pushed, but only the **master** branch is listed:

  ```sh
  $ git branch
  * master
  ```

- The reason is that the branch is associated with the remote **origin**, and such branches aren’t displayed by default. To see it, Bob can use the `-a` or `--all` option:

**3.** `git branch -a`

  ```sh
  $ git branch -a
  * master
    remotes/origin/HEAD -> origin/master
    remotes/origin/fix-trademark
    remotes/origin/master
  ```

**4.** To start work on **fix-trademark** on his local copy, Bob just needs to check it out. By using the same name (i.e., fix-trademark), he arranges for it to be associated with the upstream branch on GitHub, which means that git push will automatically push up his changes:

  ```sh
  $ git checkout fix-trademark
  Branch fix-trademark set up to track remote branch fix-trademark from origin.
  Switched to a new branch 'fix-trademark'
  ```
**5.** `git diff master`

  ```sh
  $ git diff master
  diff --git a/about.html b/about.html
  index 2cd3513..17053e8 100644
  --- a/about.html
  +++ b/about.html
  @@ -3,6 +3,7 @@
     <head>
       <meta charset="utf-8">
       <title>About Us</title>
  +    <!-- Add something here to fix trademark -->
     </head>
     <body>
       <h1>About Us!</h1>
  ```

**6.** He can now make the necessary changes, save, commits and push the fix to the remote server.

##### Alice's tasks affter the trademark bugfix

1. `git pull`
   - Pull the changes Bob made.
2. `git checkout master`
   - Setting up to merge fix-trademark branch into the master branch.
3. `git merge fix-trademark`
   - Merging branches will not create a new commit nor show up on `git log` as merging commits does.
4. `git push`
   - Push the merge to the remote server.
5. `git branch -d fix-trademark`
   - Delete the now merged fix-trademark branch.

> All that's left is for Bob to sync up the master branch.


<br>
<br>


## Additional learning materials

- [Pro Git](https://git-scm.com/book/en/v2) by Scott Chacon and Ben Straub
- [Learn Git](https://www.codecademy.com/learn/learn-git) at Codecademy
- [Git tutorials](https://www.atlassian.com/git/tutorials/) by Atlassian (makers of Bitbucket)
- [Tower Git](https://www.git-tower.com/learn/) tutorials
