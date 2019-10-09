Local repository
================
This is a version control system (VCS) tutorial using 
[git](https://git-scm.com). No previous knownledge is required. To begin 
the tutorial, create a new directory and access it from terminal.

## Creating a repository
Create or choose a directory for your project and create a file as follows.

    $ echo "Hello, world" >> simple_file.txt

Now, let's create a local repository. Execute the command below on your
repository directory.

    $ git init

You may add files or directories in order to provide version control 
atributes. From a VCS point of view, your are staging or indexing the file to 
be used for a future operation.

    $ git add simple_file.txt

Let's check what we have done so far.

    $ git status

In order to permanently change the project, we must perform our first commit.
The commit operation is like saving your actual project state and organizing it
as a history tree. Thus, you will have a history of commits with different 
files or file versions and we will be able to compare, change and more VCS
actions over this timeline.

Thus, let's commit our first file. I recommend thar any important commit 
action should be associated to a specific message using `-m` option.

    $ git commit -m "This is the first commit. Including file on git VCS"

Now let's see the status.

    $ git status

## More files...
If more files are created, the repositoy will see but won't track it.

    $ echo "Hi, world" >> local_file2.txt
    $ git status

 We can add `local_file2.txt` to our project now.

    $ git add local_file2.txt

Please note that we have only put the file on a index area. We did not applied
any commit yet. Let's check the status again.

    $ git status

We can use option -s shows the status in short format where

* ?? - untracked file
* !! - ignored file
* A  - added file
* M  - modified file
* D  - deleted file
* R  - renamed file
* C  - copied file
* U  - updated but unmerged file

    `$ git status -s`

Until now, we have learned that if we want a history version of the project, we
need to first send any new or modified elements (files, directories) to a stage
(or index) area. Only after this step, we can apply our changes and create a
version of our project (repository) using the commit action.

## Changing files on repository
If we rename the `simple_file.txt` to `local_file1.txt`, git will show that
there is an untracked file (`local_file1.txt`) and a deleted file 
(`simple_file.txt`) from your repository.

    $ mv simple_file.txt local_file1.txt
    $ git status -s

Thus, we must add the new and remove the old files from the repository. After
this, we can commit the changes and create one more step on the repository 
history tree. Let's check the repository status after every modification in 
order to understand better the functionality of git.

    $ git add local_file1.txt
    $ git status -s

You may see that `local_file1.txt` is now on the stage area but
`simple_file.txt` could not be tracked. Let's remove it to be tracked.

    $ git rm simple_file.txt
    $ git status -s

Now we don't have any problems, with one file on a stage area. It is time
to commit the changes and create one more version on the repository tree.

    $ git commit -m "Add and delete files"
    $ git status

## Review all actions
You can also review your changes until now and more. Some options are:

* Show specific repository objects (commits, tags, trees). Until now, we
only have commits. Options can be `--oneline` for condensed output, 
`--author="<pattern>"` for specific author objects and more. A common usage is
shown below.

    `$ git show`

* Check all commits (user, messages etc) in detail. `git log -N` will show
only the last N commits.

    `$ git log`

It is important to understand that the project we are working on  is identified
as the `master` tree (by default) and `HEAD` is a pointer to where the VCS
is now. We may check our commits log as a history graph using `-graph` option.

    $ git log --graph

You will find that every commit is marked with a `*`, shown sequentially from
the oldest to the newest, and the later is followed by `(HEAD -> master)`. 
It means that we are on the master tree and the project is now based on the
latest commit. 

You can also show less information using `--oneline` option.

    $ git log --graph --oneline

If you want to include information about the files and lines altered, use
`--stat` option.

    $ git log --graph --oneline --stat

For the last tip, you can also change the output format

    $ git log --graph --oneline --stat --pretty=format:"%h - %an, %ar : %s"

There are several other output options and you should refer to the manual for
more information.

## Working with differences
We will now change some of the contents of `"local_file1.txt"`. As you may
remember, it has the characters `"Hello, world"`. Let's change it and check
the repository status.

    $ echo "Hello, moon" > local_file1.txt
    $ git status -s

Wow! Git identified that the file has changed. More importantly, I want to
know **how** it changed.

    $ git diff

The output might be a bit complicated now but let's detail it better.

### Comparison input
This line displays the input sources for comparison using `diff`.

    diff --git a/local_file1.txt b/local_file1.txt

### Internal meta data
This is just an internal meta data for Git. Probably you will not need this.

    index 18249f3..157ef88 100644

### Legend for the difference
These lines represent the legend and symbols for each input file on 
comparison. 

    --- a/local_file1.txt
    +++ b/local_file1.txt

### Comparison portions
Git only shows what have changed. The first line (with `@@` symbols) is the
header showing the number of changes. On this case, 1 line was deleted (-)
and 1 line was added (+). The following shows the changes.

    @@ -1 +1 @@
    -Hello, world
    +Hello, moon

For a more realistic example, consider the following.

    @@ -23, 6 +23, 8 @@

On this example, 6 lines were deleted (-) starting from line 23. In addition, 
8 lines were added (+) starting from line 23.

If you are satisfied with your changes (we are!), let's commit it.

    $ git commit -m "I like the moon better"
    $ git status

Oops, it didn't commit your changes... Why? Well, any modification for commits
must be added to the index (or stage) instance first! So let's add it before
trying to commit anything.

    $ git add local_file1.txt
    $ git status
    $ git commit -m "I like the moon better"
    $ git status

Instead, you could use `-a` option on the commit command to automatically
index and commit the modified files on a single operation.

## And if I want to change something?
For real, this is *how to undo things?*. Sometimes you may want to go back or
change something. You need to be careful here because there are some actions
that can not be undone. This is one aspect on git that Uncle Ben was right:
"with great powers, comes great responsabilities".

### Commiting too soon
Sometimes you commit your work too soon and you forgot to include some
modification. For this, you can use `--amend` option on the commit command. For
instance, you want to include a more information on `local_file1.txt`.

    $ echo "And the stars also" >> local_file1.txt
    $ git add local_file1.txt
    $ git commit --amend

You will be redirect to a editor screen to amend the last commit message. Just
save it and check the history tree.

    $ git log --pretty=oneline --graph --pretty=format:"%h - %an, %ar : %s" 

Please mind that the last commit didn't add a new commit instance on the
history tree. Thus, it served as a correction of the last commit.

### Revert back to a commit state
And if you take some time modifying your files and your rather prefer to return
to your previous state? You can return your files to any of the commit states.
Let's change the file once more.

    $ echo "And the cockroaches" >> local_file1.txt
    $ git status -s

We have a unstaged modified file and we will use the `checkout` command to
restore files on the working repository tree. After all, the cockroaches is a
bug on the repository and it must be eliminated. For this, we will apply the
`chekout` command directly on `local_file1.txt`.

    $ git checkout -- local_file1.txt

Now `local_file1.txt` is back to the last commit stage and we could get rid of
the evil cockroaches.

But if you need to go back even further and restore the state of a specific 
commit on the history tree? First, you need to identify the commit ID. It
appears as the first information on commit logs.

    $ git log --pretty=oneline --graph --pretty=format:"%h - %an, %ar : %s" 
    * 8601886 (HEAD -> master) I like the moon better
    * df49dae Add and delete files
    * dbb8ab4 This is the first commit. Including file on git VCS

The ID of the second commit is `df49dae` and we can change to its state with
checkout.

    $ git checkout df49dae

The command output warns you that the `HEAD` pointer has detached from the 
master state. Now you can check the files, modify them, commit new changes and
more. You can check the list of commits.

    $ git log --oneline --graph

The problem is that any commit on a detached `HEAD` is orphaned and will 
eventually be deleted by git. To solve this, we could create a branch of the
current state. But it is a topic for the future. Thus, let's return to the 
master commit.

    $ git checkout master

## Stashing your work
Consider a modification on `local_file1.txt` including a new line as

    $ echo Hello, ocean >> local_file1.txt
    $ cat local_file1.txt
    $ git status -s

You can see that the file is modified without indexing or commiting it. The
`stash` command saves your current work locally for future use. To be more 
specific, stashes are temporary stages that you may find interesting to save 
for future use or as reference. You can have one or several stashes and you 
can revert or apply it to your current work. Let's stash this work.

    $ git stash
    $ git status

Ok, now we have stashed the current work and git understands that everything
is fine. After all, the work is saved and any modification on the repository is
put away. Considering this, please mind that `local_file1.txt` have returned 
to the state **before** the stash action. You may check the file using the
command below.

    $ cat local_file1.txt

From now, you can change your files and perform any git actions necessary.
Instead, I have decided to work a bit more on `local_file1.txt`.

    $ echo Hello, lake >> local_file1.txt
    $ git status -s

This is an important change on the current project as lakes are very different
from oceans. Still, I am not sure if I want to mantain lakes or if any
improvement is worth. Well, let's stash this including a descriptive message
with the `save` option.

    $ git stash save "Including lakes, but I prefer rivers"
    $ git status -s

### Multiple stashes
The work is saved and, thus, everything is fine on git world. We can now list
the stahes we have until now.

    $ git stash list

You can see that we have 2 stashes saved and the file returned to a state 
**before** the stash action. You can see the stash identifiers from the 
command output, namely `stash@{0}` and `stash@{1}`. The identifiers are
organised sequentially using `{0}` for the newest stash. In addition, there
is a WIP identifier which means "Work In Progress" status.

In order to understand the mechanics of stashes better, let's change our 
file one more time.

    $ echo Hello, river >> local_file1.txt
    $ git status -s

We now have a modified file, the git repository identifies the modified file
and we have some options for next steps. But first we can view a summary of
the differences between the commited file and the latest stash (by default).

    $ git stash show

For a more detailed output, you may include `-p` option. We can also check the 
differences from the commit and any stash using its identifiers.

    $ git stash show -p stash@{1}

Let's stash the current state.

    $ git stash

### Manipulating stashes
We have 3 stashes and the last commit state was restored. As far as I don't 
like lakes, I want to delete `stash@{1}` using `drop` option.

    $ git stash drop stash@{1}
    $ git stash list

Oops... It seems that `stash@{1}` is still there. Let's check both stashes to
see what is going on.

    $ git stash show -p stash@{0}
    $ git stash show -p stash@{1}

Ow, git enumerate the stashes sequentially and, as we removed the lake stash, 
the other stashes were altered accordingly. So, there is a good reason to
include a descriptive message when stashing!

Now I want to continue working with the river stash (`stash@{0}`). We can
reapply the changes using `pop` command. Please mind that using `pop`, the
respective stash will be removed from your saved stashes. In addition, you
could use the stash identifier (`stash@{*}`) after the command in order to
use a specific stash. If it is ommited, git will use the newest stash.

    $ git stash pop
    $ git stash list
    $ git status

We are working now with rivers and there is only one saved stash left. Please
mind that the ocean file is now on `stash@{0}`. If you like the ocean better 
(I do!), we can apply `stash@{0}` changes to our file. But this time, we want
to keep the saved stash available for future use. On this case, we need to use
`apply` command. Again, if you omit the stash identifier after the command, it
will use the newest one.

    $ git stash apply stash@{0}

Ooops, git does not allow this by default as an overwriting precaution. After
all, your work is not currently saved! Thanks git, for the warning! For the
record, the command will fail if there are conflicts on the  project. In order
to resolve the conflicts, let's commit the current changes.

    $ git commit -a -m "Using the river approach"
    $ git status
    $ git stash list

The stash list is still there and now we can apply it! But we see that our 
file now have a description of modifications.

    $ git stash apply
    $ git status
    $ git stash list

A conflict message regarding merging the file contents is shown. Well, we
messed up our project. But we can fix it using `stash push`!

    $ git stash push

This operation is the opposite of `apply`, 


### Reverting modifications
In fact, git included merging identifiers on your file! And how the merging
conflicts are presented? You may find `<<<<<`, `======` and `>>>>>` 
identifiers in your file.

    $ cat local_file1.txt
    Hi, world
    <<<<<<< Updated upstream
    Hello, river
    =======
    Hello, ocean
    >>>>>>> Stashed changes

We have some options here.

* We may decide not to merge the files. We can go back to the `HEAD` pointer on
  the commit history.
* We can also deal with the conflicts, correct the files to what you really
  want and clean everything to commit (or add) the modifications.

From now, I will go for the first option and revert the changes using `git
apply`. The later use the `git diff` output to check the differences and revert
it. But first, we will check the commit identifier 

