# 7. Git Tutorial

Git is a source code version control system. It is useful to track changes in code.
Git is required for the labs in 3157.

## Configuring Git for COMS W3157

### Set `EDITOR` envionment variable

Type `echo $EDITOR` in the terminal. It should say `vim`. 

If it doesnt, add the following line to the end of the `.bashrc` file in the home directory:
`export EDITOR=[editor]`

### Configuring your Git Environment

We first need to tell git name and email. This information is stored in `~/.gitconfig`
    `git config --global user.name "your full name"`
    `git config --global user.email uni@columbia.edu`
    
### Creating a Project

Create a new directory ~/tmp/test1

Put the directory under git revision control:
    `git init`

`ll` will now show that there is a `.git` directory. The git repository for the current directory is stored here. 

Write and run a `hello.c` program.

`git status` will show that we have `a.out` and `hello.c` that are "Untracked". Let's move `hello.c` to the staging area using `git add hello.c`

Now, let's commit it: `git commit`. Include a one line commit message when prompted.

### Modifying Files

Modify `hello.c` to print "bye world" and run `git status`. It reports that the file is "changed but not updated." 

Before moving to staging, we can see what we changed in the file using `git diff` or `git diff --color`. 
This tells us that we took out the "hello world" line and added a "bye world" line. 

Move this to the staging area using `git add`. In git, "add" means this: move the change to the staging area. This could be a modification to a tracked file or the creation of a new file.

At this point, `git diff` will report no change. Our change has been moved to staging. `git diff` reports the difference between the staging area and the working copy of the file. To see the difference between the last commit and the staging area, use `git diff --cached`

Commit again with a one liner message: `git commit -m "changed hello to bye"`

To see commit history: `git log`

You can add a brief summary of what was done at each commit: `git log --stat --summary`

You can see the full diff at each commit: `git log -p` or `git log -p --color`

### The tracked, the modified, and the staged

A file in a directory under git revision control is either tracked or untracked. A tracked file can be unmodified, modified but unstaged, or modified and staged. 

There are four possibilities for a file: 

1. Untracked
    Object files and executable files that can be rebuilt are usually not tracked.
2. Tracked, unmodified
    The file is in the git repository, and it has not been modified since the last commit. "git status" says nothing about the file.
3. Tracked, modified, but unstaged
    You modified the file, but didn't `git add` the file. The change has not been staged, so it's not ready for commit yet.
4. Tracked, modified, and staged
    You modified the file, and did `git add` the file. The change has been moved to the staging area. It is ready for commit.
    
The staging area is also called the "index".

### Other Useful Git Commands

To rename a tracked file: `git mv old-filename new-filename`

To remove a tracked file from the repository: `git rm filename`

The mv or rm actions are automatically staged, but you still have to commit the actions.

If you make changes and want to go to the version last committed (the file must not be staged yet), then you can do: `git checkout -- filename`

If the file has been staged, you must unstage it first `git reset HEAD filename`

To pull up a manual page or a command, for example `git status`:
    `git help status` or
    `man git-status`

Search for specified patterns in all files in the repository: `git prep printf`

### Cloning a Project

Often times, a programmer starts with an existing code base. When the code base is under git version control, you can *clone* the repository.

Let's clone test 1 into test 2: `git clone test1 test2`

If we run `git log` you will see that the entire commit history is replicated here. The two repositories are indistinguishable after cloning.

You can run `git log origin` to see commit history after the clone.

### Generating a Patch Set

You can save the full details of everything done after cloning with: `git format-patch --stdout origin > mywork.mbox`

If you open `mywork.mbox` with editor, you will see that the file contains the full diffs of all commits. A diff is also called a patch. The file therefore contains a set of patches, one for each commit after cloning.

When you were looking at the mywork.mbox file in your editor, you might notice that each patch looks like an email message. That's because they are. `.mbox` is the UNIX mailbox format so it can be viewed using an email application. 

Use mutt, a terminal based email app, to view the file. Type `q` to exit out of mutt.
    `mutt -f mywork.mbox`
    
If you want to read the file with the diffs in color, run the following command. Use the arrow keys to scroll and `q` to quit. 
    `cat mywork.mbox | colordiff | less -R`
    
The patch file is what you submit when you complete a lab assignment. 

### Applying the Patch Set

The TAs will apply the patch to the original repository to grade the work. Let's go through that process.

Clone test1 into test 3. Then run `git log` to verify you have commits made in test1 but not the ones made in test2. 

Now, apply the patch using: git am ../test2/mywork.mbox

### Adding a Directory into your Repository

After the deadline, a solution will be added. Let's simulate that process. 

```
cd ../test1
mkdir solution
cd solution

cp ../hello.c .
echo 'hello:' > Makefile
```

Two files, `Makefile` and `hello.c` have been created in the solutions directory.
Now, commit the new directory.

### Pulling Changes from a Remote Repository

To view solutions, you have to pull the changes to your repository. Let's pull changes we just made in test1 into test2:

    cd ../test2/
    git pull

This looks at the original repository that you cloned from, fetches all changes made since clone, and merges the changes into the current repository.

## Learning More about Git

You can view the official git tutorial in `man gittutorial`

You can also go to the [Official Git Documentation](http://git-scm.com/documentation)

*edited T9/27*
