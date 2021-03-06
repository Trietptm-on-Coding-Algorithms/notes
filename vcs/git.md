- Bookmark: 4.4. https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server
- `git clone git://git.kernel.org/pub/scm/git/git.git`
- http://git-htmldocs.googlecode.com/git/user-manual.html
- https://github.com/Homebrew/homebrew/blob/master/Library/Formula/git.rb
- http://git.kernel.org/cgit/git/git.git/
- https://git-scm.com
- https://help.github.com/
- https://www.atlassian.com/git/tutorials/
- https://github.com/git/git
- http://stackoverflow.com/questions/7321360/is-the-git-storage-model-wasteful
- http://stackoverflow.com/questions/338436/is-there-a-quick-git-command-to-see-an-old-version-of-a-file
- http://stackoverflow.com/questions/449541/how-do-you-merge-selective-files-with-git-merge
- http://stackoverflow.com/questions/5817579/how-can-i-preview-a-merge-in-git
- [How git branches and tags are stored on disk SO](http://stackoverflow.com/questions/20666331/how-git-branches-and-tags-are-stored-in-disks)
- `man git`
- `gitcli`
- `gitworkflows`
- `giteveryday`
- `gitcvs-migration`
- `gittutorial-2`
- `gitcore-tutorial`
- `gitglossary`
- `git-help`

# Bookmark
- Done: clone, brach, remote
- `man git-merge` : Bookmark: options `--no-commit`

# Compare the fetched commits with local branch
- `git log master..origin/master --format="%Cred%><(12)%ar %Cblue%h%Creset %s" --color`

# `.gitignore`
- https://github.com/github/gitignore
- https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository#Ignoring-Files

# Concepts
- distributed (Git, Mercurial, Bazaar, Darcs) and centralized (CVS, Subversion, Perforce) VCS differ in where all the deltas and associated metadata are stored.
- difference-based (CVS, Subversion, Perforce, Bazaar) vs snapshot-based (Git)
- branch
    + long-running vs topic
- tag
- conflict resolution mechanism
- textual conflict (can be handled by VCS) vs distributed conflict (can't be handled by VCS)
- detached HEAD

In git commits, trees and blobs are stored under the name equal to their hash values.

GitHub uses https and ssh protocols for remote communication.

# How to commit
Begin the commit message with a single short (less than 50 character) line summarizing the change, followed by a blank line and then a more thorough description.

- `git help gitremote-helpers`
- `git format-patch` - format commit into an email
- `git send-email`
- `git request-pull`

Commit
- type and size
- hash key of tree representing root directory
- author
- committer
- parent
- comment
- description

A tree is a blob which represents directory state
- type and size
- list of hash keys of trees and blobs that this directory contains

A blob
- type and size
- file contents

- commit ammending
- patch - file format representing difference between files
- checkout - decompressed snapshot in the working directory
- tracked files - files that are in the last snapshot
- remote (`https://`, `git://`, `user@server:path/to/repo.git` (ssh))
- three-way merging

- Staging Area (index) - a special file in `.git` - staged files (place where a new snapshot is being formed).
- `.git` repository - committed files (compressed metadata and object database). Git generally only adds data here. Everything in Git is check-summed before it is stored and is then referred to by that checksum for the purposed of **integrity**. You can’t lose information in transit or get file corruption without Git being able to detect it. The mechanism that Git uses for this checksumming is called a SHA-1 hash. This is a 40-character string composed of hexadecimal characters (0–9 and a–f) and calculated based on the contents of a file or directory structure in Git. Git stores everything in its database not by file name but by the hash value of its contents.
In addition to the commit SHA-1 checksum, Git also calculates a checksum that is based on the patch introduced with the commit. This is called a “patch-id”.
  + `hooks/`
  + `info/`
    - `alternates/`
  + `logs/`
  + `objects/` - the object database (files, dirs, commits, annotated tags)
    - `00/`, `01/`, ..., `ff/`
    - `info/`
    - `pack/`
        + `pack-hash.idx`
        + `pack-hash.pack`
  + `refs/`
    - `remotes/`
        + `origin`
            - `HEAD` -> `refs/remotes/origin/master`
            - dev -> commit hash
            - master -> commit hash
        + `john_doe`
            - head-name-1 -> commit hash
    - `heads/`
        + `dev` -> commit hash
        + `master` -> commit hash
        + `feature1` -> commit hash
    - `stash`
    - `tags/`
        + `0.0.1` -> tag object hash for annotated tags (Tag object is a binary blob)
        + `0.0.2` -> commit object hash for simple tags (Commit object is a binary blob)
  + `config` - repository configuration file
  + `COMMIT_EDITMSG`
  + `TAG_EDITMSG`
  + `FETCH_HEAD` - hashes of all branches heads for fetching (txt file)
  + `HEAD` -> `refs/heads/dev` (currently checkout branch)
  + `ORIG_HEAD`
  + `description`
  + `packed-refs`
  + `index` - cache of the state of a dir tree, used to create commits, check out working dirs, and hold the various trees involved in a merge.

# Commands
- `git help <verb>`
- `git <verb> --help`
- `man git-<verb>`

- `git-format-patch`
- `git-am`
- `git-bisect` - When there is a regression in your project, one way to track down the bug is by searching through the history to find the exact commit that's to blame. Git
bisect can help you perform a binary search for that commit.

- `git gc`

- `git repack`
- `git pack-objects`
- `git submodule`
- `git init` - create new `.git` folder with skeleton contents

- `git push` - upload `.git` folder.
- `git push remote refs/heads/localhead:refs/heads/remotehead
- `git push [remote-name] [local-head]` - send new snapshots upstream
- `git push [remote-name] [local-head]:[remote-head]
- `git push [remote-name] :[remote-head] - delete remote branch
- `git push [remote-name] --delete [remote-head] - delete remote branch
- `git push --force`

- `git pull` - download `.git` folder
- `git pull` - fetch and merge a tracked remote branch into a local branch
- `git pull --rebase`

- `git rm --cached -r` - ???
- `git rm --cached <pattern>` - remove files from git but keep it on disk
- `git rm <pattern>` - remove files from git and disk

- `git add <pattern>` - copy any new/changed file, directory, matches or everything into `.git/index` as they are now

- `git reset HEAD *` - unstage all
- `git reset HEAD *.swp` - unstage all `.swp` files
- `git reset HEAD <file>` - unstage from index
- `git reset --hard HEAD^` - reset current branch and working dir to its state at HEAD^
In addition to losing any changes in the working directory, it will also remove all later commits from this branch. If this branch is the
only branch containing those commits, they will be lost. Also, don't use git reset on a publicly-visible branch that other developers pull from, as it will force needless
merges on other developers to clean up the history. If you need to undo changes that you have pushed, use git revert instead.
- `git revert`
- `git gc` - garbage collect leftover objects.

- `git grep "hello" v2.5` - search for string "hello" in v2.5 history.
- `git grep "hello"` - search for "hello" in current working dir among tracked files.

- `git commit -m 'my message'` - create a new snapshot from staged changes.
- `git commit -v` - open an editor with status and diff attached as comments.
- `git commit -a -m 'added new benchmarks'` - skip `git add` and add and commit automatically what's tracked and changed.
- `git commit --amend` - fix the last commit (both contents and message)

- `git status -s` - compare files in working directory and index against the latest snapshot

- `git checkout` - updates files in the working tree and HEAD, resets index.
- `git checkout -b <new-head> --track <remote-shorthand>/<head>`
- `git checkout -b [new-head] [remote/head]` - checkout a remote head into a local tracking branch.
- `git checkout -- <file>` - replace file in the working directory with one from the latest snapshot (discard changes)
- `git checkout -- .` - discard all unstaged changes.
- `git checkout [commit-hash]` - 'HEAD = commit-hash and revert files in working directory'
- `git checkout --track remote/head` - checkout a remote head into a local tracking head.

- `git-check-ref-format`

Now git uses reflog.
HEAD@{1} is the previous version of HEAD, i.e. @{1} is $(git symbolic-ref HEAD)@{1}

`git ls-remote --heads http://git.kernel.org/.../jgarzik/libata-dev.git` - check the branch names in a remote repo.

## Rebasing
http://stackoverflow.com/questions/7744049/git-how-to-rebase-to-a-specific-commit

# Art of collaborating
- `git fetch [remote-url] [remote-head]`
- `git log -p HEAD..FETCH_HEAD` - what changed in FETCH_HEAD since history diverged.
- `git log -p FETCH_HEAD..HEAD` - what changed in HEAD since history diverged.
- `git log -p HEAD...FETCH_HEAD` - what changed in both HEAD and FETCH_HEAD since history diverged.
- `git log -p master..bob/master` - remote shorthand used.
- `git diff hash1 hash2 [--]` - difference between tips of 2 heades
- `git diff hash1..hash2`
- `git diff hash1...hash2` - what's in head1 or head2 but not in their common ancestor (head1 XOR head2)
- `git diff v2.5:Makefile HEAD:Makefile.in` - compare 2 files in different snapshots
- http://stackoverflow.com/questions/3368590/show-diff-between-commits/29374476#29374476
If you run `git difftool` instead of `git diff`, you can view any of these diffs in software like `emerge`, `vimdiff` and many more. Run `git difftool --tool-help` to see what is available on your system.
- http://stackoverflow.com/questions/822811/showing-which-files-have-changed-between-two-revisions


# See file at a specific revision
- `git show REVISION:Makefile` - time machine for a file.

# Art of merging and conflict resolution
In git if anyone move an directory and someone else create a file in that directory in another branch merging logic won't detect this and the created file will land in the unexpected place after the merge. That's why if someone is going to rename or move directories they should notify other collaborators about that.

## Refs
- Bookmark: https://en.wikipedia.org/wiki/Merge_(version_control)#Fuzzy_patch_application
- Bookmark: https://git-scm.com/book/en/v2/Git-Tools-Advanced-Merging # Ignoring whitespace
- http://stackoverflow.com/questions/572237/whats-the-best-three-way-merge-tool
- http://stackoverflow.com/questions/366860/when-would-you-use-the-different-git-merge-strategies
- http://stackoverflow.com/questions/17656448/how-does-git-decide-on-conflicts
- http://stackoverflow.com/questions/4920885/what-constitutes-a-merge-conflict-in-git

## Tips
- Merge long lived branches often.
- Always have working tree and index in a clean state before merging.
- `git merge --no-commit <commit>; git diff --staged` - inspect what merge will introduce.

## Merge algorithms
- three-way merge
- recursive three-way merge
- fuzzy patch application
- weave merge
- patch commutation

## Merge tools
- `diff3`

## Theory
With git, every merge is a conflict, which leaves you with an index that contains three versions of each file, the versions from each branch and the base. On this index, various resolvers are run, which can decide for each individual file how to resolve the matter.

The first stage is a trivial resolver, which takes care of things like unchanged files, cases where one branch has modified a file while the other didn't, or where both branches contain the same new version of the file.

Afterwards, it's plugins that look at the remaining cases. There is a plugin that handles text files by identifying individual changes (like diff) in one branch and trying to apply those to the other branch, falling back on placing conflict markers if that doesn't work. You can easily hook in your own merge tool at this point, for example, you could write a tool that knows how to merge XML files without violating well-formedness, or that gives a graphical user interface that allows interactive editing and a side-by-side view (for example, kdiff3 does that).

So the presentation of conflicts is really a matter of the plugin used; the default plugin for text files will use the same style as CVS did, because people and tools are used to it, and the conflict markers are a known syntax error in almost any programming language.

`merge-file` is the last-resort merge driver for text files. You can specify that a different merge driver should be used instead.

## Commands
- `git merge bob/master`
- `git diff` will show the conflicts when merge failed.
- `git commit -a` will commit a merge once all conflicts are resolved.
- http://stackoverflow.com/questions/226976/how-can-i-know-in-git-if-a-branch-has-been-already-merged-into-master

## Undo a commit
- http://stackoverflow.com/questions/927358/how-do-you-undo-the-last-commit

## Edit commit message
- http://stackoverflow.com/questions/179123/edit-an-incorrect-commit-message-in-git

## git log
By default the commits are shown in reverse chronological order. The default revision range is HEAD.
- `git log [<options>] [<revision range>] [[\--] <path>…​]`
The command takes options applicable to the `git rev-list` to control what is shown and how, and options applicable to the `git diff-*` to control how the changes each commit introduces are shown.
- `git log -p` - commits with diffs (patches) (helpful for code review)
- `git log --stat` - commits with stats
- `git log --stat --summary`
- `git log --shortstat`, `--name-only`, `--name-status`, `--abbrev-commit`, `--relative-date`
- `git log --pretty=oneline` - format
- `git log --oneline`
- `git log --decorate` - display which branches point to commits
- `git log --pretty=format:"h - %an, %ar : %s" - format
- `git log --graph` - ASCII graph showing your branch and merge history
- `git log --all` - log all refs
- `git log --graph --all` - nice staff
- `git shortlog` groups commits by author
- `git log --stat src/main/resources/assets/client` - see only commits to the files inside the folder (recursively) with stats
- `% git log --format="%s" v0.1.0..v0.1.1` - see commits between 2 tags
- `git rev-list --max-parents=0 HEAD` - hash of first commit
- http://stackoverflow.com/questions/14247713/retrieve-the-list-of-child-commits-of-an-specific-commit-in-git`:`
- `% git log -2` - last 2 commits
- `% git log --after=2.weeks`
- `% git log --before=2.weeks`
- `% git log --since="2008-01-15"`
- `% git log --since="2 years 1 day 3 minutes ago"`
- `% git log --author`
- `% git log --committer`
- `% git log --grep` - grep through commit messages
- `% git log -Sfunction_name` - grep through modifications
- `% git log -- <filepath>` - files and directories of interest
- `% git log --no-merges`
- `% git log --author --grep` - show either (OR) match
- `% git log --author --grep --all-match` - show both (AND) match
- `git log v2.5..v2.6`
- `git log v2.5..`
- `git log v2.5.. Makefile` - commits since v2.5 which modify Makefile.
- `git log v0.1.8.. --graph --oneline`

- `git config --global user.name 'Alex Yursha'`
- `git config --global user.email 'alexyursha@example.com'`
- `git config --global core.editor vim`
- `git config --get-regexp '^(remote|branch)\.'

- `git cherry-pick`

# See the file history
- `git log -p <filename>`

# Rename function calls
- https://github.com/thlorenz/rename-function-calls
- Tracing the git history of a Ruby Method  http://gofreerange.com/tracing-the-git-history-of-a-ruby-method

# Git Branching 
A head is a movable pointer to one of commits.
`HEAD` is a movable pointer to the currently active head.
`HEAD^` is a `HEAD`'s parent.
`HEAD~2` - is a `HEAD`'s grandparent.
`HEAD^1` - first parent of HEAD
`HEAD^2` - second parent of HEAD
- A successful Git branching model http://nvie.com/posts/a-successful-git-branching-model/ 

# Stashing
Stashing takes modified tracked files and staged changes and saves it on a stack of unfinished changes that you can reapply at any time.
- Bookmark: https://git-scm.com/book/en/v1/Git-Tools-Stashing#Un-applying-a-Stash 
- http://stackoverflow.com/questions/3040833/stash-only-one-file-out-of-multiple-files-that-have-changed-with-git/
- `git stash list` - show stash stack
- `git stash apply` - apply the stack top stash, by default changes that were staged are not staged again, you continue to have stash in a stack
- `git stash apply stash@{2}` - apply a particular stash
- `git stash apply --index` - try also to stage changes that were staged during saving a stash
- `git stash drop [stash@{2}]` - drop a (named) stash from the stack
- `git stash pop` - apply the topmost stash and drop it.

# Remotes
- `% git ls-remote` - list references in a remote repository
- `% git fetch [remote-name]`
- `% git fetch --all`

## Rebasing
- http://gitforteams.com/resources/rebasing.html
- https://help.github.com/articles/resolving-a-merge-conflict-from-the-command-line/
- https://help.github.com/articles/resolving-merge-conflicts-after-a-git-rebase/
Allows for a cleaner history which is important for a code review and easy pull requests integration.
Do not rebase commits that exist outside your repository.
If you treat rebasing as a way to clean up and work with commits before you push them, and if you only rebase commits that have never been available publicly, then you’ll be fine. If you rebase commits that have already been pushed publicly, and people may have based work on those commits, then you may be in for some frustrating trouble, and the scorn of your teammates.
- `git checkout topic-branch`
- `git rebase master`
- `git checkout master`
- `git merge topic-branch`
Rebasing replays changes from one line of work onto another in the order they were introduced, whereas merging takes the endpoints and merges them together.
- `git rebase --onto master server client`
- `git rebase [basebranch] [topicbranch]`
- `git rebase [remote/branch]

- `git filter-branch`

## How rebase works
- Determine what work is unique to branch which is to be rebased
- Determine which are not merge commits
- Determine which have not been rewritten into the base branch
- Apply those commits on top of base branch

# How to do a feature
1. Pick a Jira issue from the backlog
1. Discuss it with actual users, how are they going to use it
1. Fork `% git checkout -b feature`
1. Create an automated test case
1. Run a test case against existing code
1. Understand existing code behaviour
1. Change existing code
1. Make sure a test case succeeds against changed code
1. Commit with message prefixed as `JIRA-001: My message`
1. Merge with `master` and delete feature branch
    - `git checkout master`
    - `git merge feature` (may be fast-forward or 3-way merge with common ancestor (merge-base))
`git mergetool --tool-help`
- `git merge origin/serverfix`
- `git merge @{u}` - merge tracked remote branch.
        + if conflict:
        + git status
        + vim conflict-file
        + git add conflict-file
        + git commit
    - `git branch -d feature`
1. Push
1. Close Jira issue
1. Tag `% git tag -a v1.0 -m "version 1.0"` and push
1. Release, i.e. deploy to production

# Tags
- http://stackoverflow.com/questions/1028649/how-do-you-rename-a-git-tag
- https://confluence.atlassian.com/bitbucket/how-do-i-remove-or-delete-a-tag-from-a-git-repo-282175551.html


# Deployment
- **hot deployment** - deployment of code when an application continues to run.

# Installing from source
If you do want to install Git from source, you need to have the following libraries that Git depends on: `curl`, `zlib`, `openssl`, `expat`, and `libiconv`.
- https://www.kernel.org/pub/software/scm/git/
- https://github.com/git/git/releases

```
$ tar -zxf git-2.0.0.tar.gz
$ cd git-2.0.0
$ make configure
$ ./configure --prefix=/usr
$ make all doc info
$ sudo make install install-doc install-html install-info
```

After this is done, you can also get Git via Git itself for updates:

Git can use four major protocols to transfer data: Local, HTTP, Secure Shell (SSH) and Git.

# Git Flows
- https://github.com/pivotal/git_scripts (For Pair Programming)
- https://www.atlassian.com/git/tutorials/comparing-workflows
- http://nvie.com/posts/a-successful-git-branching-model/

# Complex commands
- `(set -e && git rev-list --reverse master~3..master | while read rev; do git checkout $rev; python runtests.py; done)`
- `git ls-files --deleted | xargs -L1 git checkout` - restore all deleted files

# Add remote repository
```
git remote add <name> <url>
```
