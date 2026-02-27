---
title: "Using RMarkdown"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- Do you know what a version control system is?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- This worksheet provides a slightly simplified view of how git is used "in real life".
- You will create a remote repository on GitHub, clone it to your local machine, make some changes, and push those changes back to GitHub.

::::::::::::::::::::::::::::::::::::::::::::::::
## Prerequisites

Before you start, you should have the following things set up:

1. Your computer needs to have `git` installed; Google Colab terminal has git;
2. You need to be able to type commands using a terminal on your computer;
4. You need to have created a [GitHub](https://github.com) account for yourself

## 1. Create a remote repository

The first thing we're going to do is to create an empty remote repository using GitHub.  We'll use this repository for practice purposes, so be sure of making it *private*. You'll be also asked to create a *name*, which should be something you can easily track. Also, allow GitHub to create a README file. We suggest a (later) carefull reading about licenses.

Go to your GitHub account, and click on the "+" icon at the top right of the page, then select "New repository". You should see a page like this:
![Creating a new repository](images/git_new_repo.png)
Fill in the repository name (e.g., `test-may26`), add a description if you want, select "Private", and check the box to add a README file. Then click on the green "Create repository" button.

Now that you've created your 1st repository, we'll clone it on your local machine. Note that you may clone any public repository in the exact same way.

::::::::::::::::::::::::::::::::::::: callout

Cloning a repository means making a local copy of a remote repository. Any changes you make to your local copy can later be pushed back to the remote repository. You can clone any repository you have access to, including your own repositories and public repositories created by other users.

::::::::::::::::::::::::::::::::::::::::::::::::

# 2. Clone the remote repo and make it a local repository

At your new repository, find and click at the green box where it says "<>Code". A dropdown menu box will appear, and you'll be able to copy the HTTPS adress.

This session will be mainly completed at the terminal. Use the `git clone` command with the HTTPS address you just copied:

```bash
$ git clone <HTTPS_ADRESS>
```

This command has created a new directory in your machine named `test-may26 at your currect working directory.
As this is a empty repository, it contains only one file: `README.md`. The `git clone` command has also created a hidden directory named `.git` inside the new directory. This directory contains all the information about the repository, including its history and configuration.
To check that the `.git` directory exists, change into the new directory and list its contents:
```bash
$ cd test-may26
$ ls -a
./ ../ .git/ README.md
# As it starts with a dot, it is hidden (at least in bash), so we ened to use the `-a` argument to `ls` to see it:
```

Now check the status of the repository:

```bash
$ git status

On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

> The above message is saying that there is a repository here with one file only: README.md.

## 3. Let's populate our local repo

Now that we have a repository set up it is time to add some data.
This can be anything, but here we'll use a simple python script.

We'll create an empty file first.

~~~console
$ touch myCode.py
~~~

Now that we've created a file, let's make it print the classical "Hello, world!" message.

~~~console
$ code myCode.py
~~~

Note that If you're on a different terminal, you'll need a different text editor (like vim).

Now, at the text editing are, we'll add the following code to the file:

~~~python
#!/usr/bin/env python3
print("Hello, world!") 
~~~

Save the file and run the code on the terminal.

~~~console
$ python myCode.py
~~~

## 4. Make our first commit

We now need to add this new file to the index, and commit it to our repository.
Check the status first, then add it to the index:

~~~console
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
~~~

> This message tells us that the new file is now in the index ready to be committed.

We can now store this set of changes into a commit:

~~~console
$ git commit -m"My first commit"
[main fcf698d] My first commit
 1 file changed, 3 insertions(+)
 create mode 100644 myCode.py
~~~

Now, let's check the log:

~~~console
$ git log
commit fcf698dbaffbc01a839d32a34e90282ea1ca7be1 (HEAD -> main)
Author: fabiano-pais <fabiano.pais@york.ac.uk>
Date:   Tue Mar 26 13:22:11 2024 +0000

    My first commit

commit cccf6f34a28efbd926e15205384242a0fe7c1236 (origin/main, origin/HEAD)
Author: Fabiano Pais <100767268+fabiano-pais@users.noreply.github.com>
Date:   Mon Mar 25 11:04:56 2024 +0000

    Initial commit
~~~

> Your log will look different.
> Your commit will have a different ID, and your username should be different to this one!
> This is the "standard loop" of git: generate content; fill up an index of modified files; commit those changes to the repository.

## 5. Let's test the repository

One use of git is as a method of backup.
Let's test this by deleting the file.

~~~console
$ rm myCode.py
$ ls
~~~

Oops!  We've accidentially deleted our code!
Not to worry; we have a copy in the repository.
First, let's check what git thinks about this:

~~~console
$ git status
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	deleted:    myCode.py

no changes added to commit (use "git add" and/or "git commit -a")
~~~

> This message is telling us that a file that is recorded in the repository is missing.
> We have the option of recording its deletion in the index and committing that (we'd use `git rm myCode.py`), but in this case the deletion was accidental so we'll simply get the file back from the latest commit.

We can retrieve the file from the latest commit:

~~~console
$ git checkout myCode.py
Updated 1 path from the index
$ ls
~~~

> Great! We've got our code back again.
> This is a major benefit of `git`.

## 6. Adding a feature

Now, let's say we need to update our code.
We'll do this in a new branch in case our modifications break something.

First, let's create a new branch my checking out the latest commit into a new branch (called `more coding`):

~~~console
$ git checkout -b more-coding
Switched to a new branch 'more-coding'

$ git status
On branch more-coding
nothing to commit, working tree clean
~~~

Update the code by replicing and adding lines:

Update line 2:

~~~python
print("Hello, UoY!")
~~~

Add a new line (3):

~~~python
print("I'm learning GIT!")
~~~

Now that we've made some modifictions, save the file, and then make a new index and commit is as before:

~~~console
$ git add myCode.py
$ git commit -m"Adding more code"
[more-coding 81d2bed] Adding more code
 1 file changed, 2 insertions(+), 2 deletions(-)
~~~

> Obviously, in the real world, more complex features will be implemented, and would likely require multiple rounds of code editing and committing.

Although we've now edited myCode.py, we can still get the unedited version by switching branches back to main:

~~~console
$ git switch main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

$ more myCode.py
#!/usr/bin/env python3
print("Hello, world!")
~~~

> As you can see, this version of myCode.py is the old version!

Once we're happy with the new code, we can merge the branch back into the main branch:

~~~console
$ git merge more-coding
Updating fcf698d..81d2bed
Fast-forward
 myCode.py | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)
~~~

> OK, we've now merged the code back into our main branch. We're back in the main branch, and our code has been correctly updated.

## 7. Push the local repository to GitHub

Now that the local repository is linked to a remote location, we can push it to the cloud:

~~~console
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
~~~

Congratulations! You've just pushed your local repository to GitHub!
If you go back to the GitHub and refresh the repository page, you should see that your code has been uploaded.

## 8. Adding a README file

Currently, the repository is not very well documented.
By convention, a file named `README` or `README.md` in the root of your workspace is taken to contain a description of the project.
If such a file exists, GitHub will display it on the main repository page.
Let's add a brief description.

Go back to your text editor, and replace the existing text by the following:

```markdown
# Analytics 2 

Introduction to Version Control.
```

As we've added a file, we need to make a new commit:

~~~console
$ git status

$ git add README.md

$ git commit -m"Adding info to README"
[main fb725f2] Adding info to README
 1 file changed, 3 insertions(+), 1 deletion(-)
~~~

We can now push this new commit to GitHub:

~~~console
$ git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 334 bytes | 334.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/fabiano-pais/test-march25.git
   81d2bed..fb725f2  main -> main
~~~

Go back to Github and refresh the page again.
Hopefully, you'll see the new README documentation appear.

## 9. Markdown

> The `README.md` file we've just created uses a format called Markdown.
> This is a very simple text file format that allows a computer to render our text nicely.
> You can dig into the format by reading the [documentation on GitHub](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/about-writing-and-formatting-on-github).
