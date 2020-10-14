# 4. Git Lesson

## Setting up Git

All the following is in the context of *lab submission*. 

### Setting up git directory in home directory

`git init` initializes a directory as a git repository.
Do not run this for the labs in the homework. You must use `git clone` instead.

### Staging

`git diff` compares the current version of the file with the version that's in the staging area.

Three versions of README
- A. README in git repo --> committed
- B. README in staging --> to be committed
- C. README in working directory

`git status` shows the status of the files being tracked/untracked by git.
`git log` shows commit log for git. 

`git diff` shows the difference between B and C. 
`git diff --cached` will show the difference between A and B.

`git commit` will pop up a Vim editor to put a commit message.
`git commit -m "[...]"` can be used to commit a single line message. 

### Git Workflow

You may be implementing one small feature but it requires you to modify many files. You want to work carefully but don't want to make 20 commits.
In this case, you will make a change to a file, `git diff` to show the change, `git add` to put it to staging. Collect all of the changes to staging and then you can commit after the feature is committed.

Type the following to submit a lab:
`/home/w3157/submit/submit-lab lab[N]`





*edited L9/22 L9/24*
