---
title: "Version Control with Git and GitHub"
teaching: 4
exercises: 4
---

:::::::::::::::::::::::::::::::::::::: questions 

- Do you know what a version control system is?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- This introduction provides a slightly simplified view of how git is used "in real life".
- You will create a remote repository on GitHub, clone it to your local machine, make some changes, and push those changes back to GitHub.

::::::::::::::::::::::::::::::::::::::::::::::::
## Prerequisites

Before you start, you need to:

1. Have `git` installed; Google Colab terminal has git;
2. Be able to type commands using a terminal on your computer;
4. Have created a [GitHub](https://github.com) account for yourself

## 1. Create a remote repository

The first thing we're going to do is to create an empty remote repository using GitHub.  We'll use this repository for practice purposes. A reposiory needs a *name*, which should be something you can easily track and related to your work. You can (and should) also add a brief *description* in the corresponded field.

To create your first repository go to your GitHub account, and click on the "+" icon at the top right of the page, then select "New repository". Fill in the repository name (e.g., `test_may26`), add a description to it. After this initial set up, please select "Private" (to make it invisible to other users) and "No template" (as we don't need this). Please check the box to add a README file to your repository. Leve "No .gitignore" and "No licence" as it is, and then click on the green "Create repository" button. That's all you need to create a repository.

When creating a repository, we suggest to do it private first as you can change it to publicly available anytime later. We also suggest reading about licenses sometime later.

Now that you've created your 1st repository, we will clone it on your local machine. Note that you may clone ANY public repository in the exact same way.

::::::::::::::::::::::::::::::::::::: callout

Cloning a repository means making a local copy of a remote repository. Any changes you make to your local copy can later be pushed back to the remote repository. You can clone any repository you have access to, including your own repositories and public repositories created by other users.

::::::::::::::::::::::::::::::::::::::::::::::::

## 2. Clone the remote repository and make it a local repository

At your new remote repository, find and click at the green box where it says "<>Code". A dropdown menu box will appear, and you'll be able to copy the HTTPS adress.

For now on, this session will be mainly executed at the terminal. We'll use the `git clone` command and paste the HTTPS address you just copied:

```bash
$ git clone <HTTPS_ADRESS>
```

This command has created a new directory in your machine named `test_may26` at your currect working directory.
As this is a empty repository, it contains only one file: `README.md`. However, the `git clone` command has also created a hidden directory named `.git` inside the new directory. This directory contains all the information about the repository, including its history and configuration.
To check that the `.git` directory exists, move into the new directory and list its contents:

```bash
$ cd test_may26
$ ls -a
./ ../ .git/ README.md
# Files and folders starting with a `.` are hidden in bash, so we need to add the `-a` argument to the `ls` command to see them
```

Now check the status of the repository:

```bash
$ git status

On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

> The above message is saying that you're in the main branch of the repository, no changes were commited so far, and there's nothing to commit.

## 3. Let's populate our local repo

Now that we have a local repository ready (cloned from a remote one), it is time to add data to this repo.
This can be anything, but here we'll create a file with a simple python script.

Let's create an empty file first.

```bash
$ touch myCode.py
```

Now that we've created a file, let's write a simple code in Python for it to print a "Hello, world!" message.
We'll need a text editor inside the terminal, and there are many. Here we'll use vim.

::::::::::::::::::::::::::::::::::::: callout

Vim is a text editor released in 1992. To start adding text to a file you must first press `I` to begin `INSERT` mode after executing the program in the Terminal.
Whist in `INSERT` mode, add the two lines below. Then, to save the content, you must press `ESC`, then `:`, then `wc` and `ENTER`.

::::::::::::::::::::::::::::::::::::::::::::::::

```bash
$ vim myCode.py
```
Once you've started the text editor, write the two strings below:
`#!/usr/bin/env python3`
`print("Hello, world!")`
Save the content and exit the text editor. Test your code by execute the python code on the terminal.

```bash
$ python myCode.py
Hello, world!
```

## 4. Make your first commit

We now need to add the new file to the index, and commit it to our repository.
Check the status first, then run git add, and run a git status again.

```bash
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	myCode.py

nothing added to commit but untracked files present (use "git add" to track)

$ git add myCode.py

$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   myCode.py
```

> The first message tells about an "untracked" file, that will be tracked after adding it to the index area;
> The second message tells us that the new file is now in the index area and ready to be committed.

Let's do our first commit:

```bash
$ git commit -m"My first commit"
[main fcf698d] My first commit
 1 file changed, 3 insertions(+)
 create mode 100644 myCode.py
```

Now check the log:

```bash
$ git log
commit fcf698dbaffbc01a839d32a34e90282ea1ca7be1 (HEAD -> main)
Author: fabiano-pais <fabiano.pais@york.ac.uk>
Date:   Tue Mar 26 13:22:11 2024 +0000

    My first commit

commit cccf6f34a28efbd926e15205384242a0fe7c1236 (origin/main, origin/HEAD)
Author: Fabiano Pais <100767268+fabiano-pais@users.noreply.github.com>
Date:   Mon Mar 25 11:04:56 2024 +0000

    Initial commit
```

> Your log will look different because of the different commit ID, along with names, date and author name;
> Every commit has a different ID, and Git uses this ID to track any change previously committed.
> This is the basic way of working with git: you generate content; add the files to the index area; then commit those changes to the local repository; and finally push to the remote repository.

## 5. Run a "test" of your new repository

Once your repository has being created, any file(s) accidentally deleted can be recovered. If you've commited to your local repository only, you may recover deleted files when using the same computer originally used to create the repository.
Let's test this feature by deleting the code you've created.

```bash
$ rm myCode.py
```

Oops!  We've "accidentially" deleted our code!
Not to worry; we have a copy in the repository.
First, let's check what git thinks about this:

```bash
$ git status
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	deleted:    myCode.py

no changes added to commit (use "git add" and/or "git commit -a")
```

> This message is telling us that a file that is recorded in the repository is missing.
> There's a command line to delete and retain deletion in the index in the repository. This can be done by entering `git rm myCode.py`.

Since the deletion was "accidental", we can retrieve the file with the following commands:

```bash
$ git checkout myCode.py
Updated 1 path from the index
$ ls
```

> We've got our code back again.
> This is a major benefit of `git`.

## 6. Adding a feature

Now, let's say we need to update our code.
We'll do this in a new branch in case our modifications break something.

First, let's create a new branch my checking out the latest commit into a new branch (called `more coding`):

```bash
$ git checkout -b more-coding
Switched to a new branch 'more-coding'

$ git status
On branch more-coding
nothing to commit, working tree clean
```

Now update the code by replicing and adding lines.

::::::::::::::::::::::::::::::::::::: callout

We'll do this again with Vim. So remember to press `I` to begin `INSERT` mode after running the program.
Whist in `INSERT` mode, make the edits below. Then, to save the content, press `ESC`, then `:`, then `wc` and `ENTER`.

::::::::::::::::::::::::::::::::::::::::::::::::

Update line 2:
print("Hello, UoY!")

Add a new line (3):
print("I'm learning GIT!")

Now that we've made some modifictions, save the file, and then make a new index and commit is as before:

```bash
$ git add myCode.py
$ git commit -m"Adding more code"
[more-coding 81d2bed] Adding more code
 1 file changed, 2 insertions(+), 2 deletions(-)
```

> Obviously, in the real world, more complex features will be implemented, and would likely require multiple rounds of code editing and committing.

Although we've now edited myCode.py, we can still get the unedited version by switching branches back to main:

```bash
$ git switch main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

$ more myCode.py
#!/usr/bin/env python3
print("Hello, world!")
```

> As you can see, this version of myCode.py is the old version!

Once we're happy with the new code, we can merge the branch back into the main branch:

```bash
$ git merge more-coding
Updating fcf698d..81d2bed
Fast-forward
 myCode.py | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)
```

> OK, we've now merged the code back into our main branch. We're back in the main branch, and our code has been correctly updated.

## 7. Push the local repository to GitHub

Now that the local repository is linked to a remote location, we can push it to the cloud:

```bash
$ git status
On branch main
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean

$ git push
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 8 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 641 bytes | 641.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/fabiano-pais/test-march25.git
   cccf6f3..81d2bed  main -> main
```

Congratulations! You've just pushed your local repository to GitHub!
If you go back to the GitHub and refresh the repository page, you should see that your code has been uploaded.

## 8. Adding a README file

At this stage, the repository you've created is not documented. You've asked Git to create a READme file, and you will a file named `README` or `README.md` in the root of your workspace. The READme file should contains what you've added at the  "description" field.


```markdown
# Analytics 2 

Introduction to Version Control.
```

As we've added a file, we need to make a new commit:

```bash
$ git status

$ git add README.md

$ git commit -m"Adding info to README"
[main fb725f2] Adding info to README
 1 file changed, 3 insertions(+), 1 deletion(-)
```

We can now push this new commit to GitHub:

```bash
$ git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 334 bytes | 334.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/fabiano-pais/test-march25.git
   81d2bed..fb725f2  main -> main
```

Go back to Github and refresh the page again.
Hopefully, you'll see the new README documentation appear.

## 9. Markdown

> The `README.md` file we've just created uses a format called Markdown.
> This is a very simple text file format that allows a computer to render our text nicely.
> You can learn more about this format by reading the [documentation on GitHub](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/about-writing-and-formatting-on-github).
