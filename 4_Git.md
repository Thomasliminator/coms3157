# 4. Git Lesson

## Setting up Git

All the following is in the context of *lab submission*. 

### Setting up git directory in home directory

`git init` initializes 

### Staging

`git diff` compares the current version of the file with the version that's in the staging area.

Three versions of README
- A. README in git repo --> committed
- B. README in staging --> to be committed
- C. README in working directory

`git diff` shows the difference between B and C. 
`git diff --cached` will show the difference between A and B.

### Git Workflow

You may be implementing one small feature but it requires you to modify many files. You want to work carefully but don't want to make 20 commits.
In this case, you will make a change to a file, `git diff` to show the change, `git add` to put it to staging. Collect all of the changes to staging and then you can commit after the feature is committed.





*edited L9/22 L9/24*
