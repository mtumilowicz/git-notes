# git-notes

## introduction
* version control concepts
* gits key features
    * It stores all the history, branches, and commits
      locally. This means adding new versions or querying history doesn’t require a network
      connection.
    * A Git repository is the local collection of all the files related to a particular Git version
      control system and contains a .git subdirectory in its root. Git keeps track of the state
      of the files in the repository’s directory on disk.
    * Git repositories store all their data on your local machine. Making commits, view-
      ing history, and requesting differences between commits are all local operations that
      don’t require a network connection.
    * in .git directory
          /.git/config // contains the configuration of the local repository
          /.git/description // is a file that describes the repository
          /.git/HEAD // HEAD pointer, respectively, that point to commits
          /.git/hooks/applypatch-msg.sample // Event hooks
          /.git/info/exclude //  contains files that should be excluded from the repository
          /.git/objects/info // Object information, used for object storage
          /.git/objects/pack // Pack files, used for reference
          /.git/refs/heads // Branch pointers, respectively, that point to commits
          /.git/refs/tags // Tag pointers, respectively, that point to commits
          /.git/index // Git’s index is a staging area used to build up new commits.
    * Git doesn’t add anything to the index without your instruction
      * As a result, the first
        thing you have to do with a file you want to include in a Git repository is request that
        Git add it to the index.
    * Each commit contains a
      message entered by the author, details of the commit author, a unique commit refer-
      ence (in Git, SHA-1 hashes such as 86bb0d659a39c98808439fadb8dbd594bec0004d ) a
      pointer to the preceding commit (known as the parent commit), the date the commit
      was created, and a pointer to the contents of files when the commit was made
      * A unique SHA-1 hash of this
        commit and all the metadata
      * git commit can be called with a path (like git add ) to do the equivalent of an add
        followed immediately by a commit.
      * It can also take the --all (or -a ) flag to add all
        changes to files tracked in the repository into a new commit
    * Git is a version control system built on top of an object store
      * Git creates and stores a col-
         lection of objects when you commit
      * The object store is stored inside the Git repository.
      * the main Git objects we’re concerned with: commits, blobs,
         and trees
        * There’s also a tag object, but don’t worry about tags until they’re introduced
      * Commit object contains:
        - commit metadata for this version
        - a reference to the root tree object
          for this version
          * Tree object contains:
            - references to blob objects for each file in the
              directory for this version
              - Blob object contains: contents of the file for this version
            - references to tree objects for each subdirectory
              of the directory for this version

## basics
* commit
    * The first thing to point out about the git log output is that each commit has an ID
      * This ID is unique to this commit even if you share your repository with a different
        server
   * Each commit contains, at a minimum, the committer’s name and the date and time at
     which the user made the commit.
   * One thing that’s not so obvious in the git log output is that every commit has a
     parent, except for the first commit.
     * The parent commit can be revealed by using the
       git log --parents switch.
* git status
  * command will tell you the state of your working directory
* history
    * The history in Git is the complete list of all commits made since the repository was cre-
      ated. The history also contains references to any branches, merges, and tags made within
      the repository.
    * The git log output lists all the commits that have been made on the current branch
      in reverse chronological order
* git add, git commit
    * Whenever you want to introduce a new file to a Git repository, you must use git add
      on that file first
      * Git can only keep track of files that it has been told about
      * To create a timeline event, you have to commit the file to the repository, using git
        commit
      * It also reports the mode for the
        newly created file in your repository ( 100644 ). The mode is a number representing a
        file’s permission. For the purposes of this book, don’t worry about a file’s permissions,
        but do note that Git tracks this.
    * git commit -a -m "This is the second commit."
      * By using the -a and -m switches together, you've avoided the git add step and
        entered a message at the same time.
      * Performing the git add at the same time as git commit is a common shortcut.
      * You do
        have to add the file first (with an initial git add ) before this shortcut can work.
    * One thing that might begin to bother you about Git is that you have to use git add at
      least once for every file that you want to commit into the repository. Then, after every
      change you make to that file, you still have to use git add on the changed file before
      you can commit the file to the repository.
      * After the first git add , doing it again for every change seems
        redundant.
  * The Git staging area contains the version of the Git working directory that you want to
    commit
    * Git acknowledges with the shortcut git commit -a that most of us will typi-
      cally put the changes in our working directory into the repository.
* tracking branch
* pull
    * git pull downloads the new commits from another repository and merges the
      remote branch into the current branch
    * Remember that git pull performs two actions: fetching the changes from a remote
      repository and merging them into the current branch.
      * Sometimes you may wish to
        download the new commits from the remote repository without merging them into
        your current branch (or without merging them yet)
      * git fetch performs the fetching action of downloading the new
        commits but skips the merge step (which you can manually perform later)
    * git pull is a two-part operation. In the first part, your local repository retrieves
      (fetches) the contents of the remote repository. In the second part, git pull attempts
      to make your repository look like the remote repository (which is now on your local
      machine) by performing a merge of the local branch and the remote-tracking branch.
    * The second step of git pull is to run git merge FETCH_HEAD
      * What is
        FETCH_HEAD ? It’s a reference to the
        remote branch that you just fetched
        in the previous section.
      * git rev-parse origin/master
        This last command should give you the same SHA1 ID as FETCH_HEAD
      * because you have two commit ID s, you can ask Git to indicate what is
         different between these two branches
        * git diff HEAD..FETCH_HEAD
      * If
        two branches have modified the same file in the same place but in different ways, Git
        has to ask how to resolve the conflict
* push
    * git push sends your new commits to it,
      and git fetch retrieves from it any new commits made by others
* fetch
      * retrieves files from one repository and incorporates those files into your
        repository. Specifically, git fetch retrieves references, which for you means branches
        or tags
      * When they arrive at your local repository, they are laid down on top of your
        repository, along with any files that they point to
      * you can see that your current HEAD (master) is still at 195f2a1 on
        the second line, but the remote-tracking branch for master (origin/master) is
        7746e35 on the first line
        * Hopefully it’s clear by now that
          git fetch has brought in a new
          object (a new commit to master)
          and laid it right on top of your
          local repository.
* merge
    * A merge results in a commit that has two (or even more) parent
      commits.
    * merge commit has two parents: the latest commit from the
      master branch and the latest commit from the bugfix branch
    * One special case of merging in Git is the fast-forward merge. This special case takes
      effect when the target branch is a descendant of the branch that it will merge with.
    * fast-forward
    * Let’s start by setting up how to perform a merge that could be made without creating
      a merge commit: a fast-forward merge.
      * Recall that a fast-forward merge means the
        incoming branch has the current branch as an ancestor
      * A merge commit has two parents: the previous commit on the current branch ( master
        in this case) and the previous commit on the incoming branch ( chapter-spacing in
        this case)
      * https://www.biteinteractive.com/understanding-git-merge/
        * Git first figures out that the merge base is commit C. (Do you see why?)

      Git then calculates the diff from C to G (because G is master) and the diff from C to Z (because Z is otherbranch).

      Git then applies both of those diffs to C simultaneously — and commits the result on master. That is the merge commit. (And, as I’ve already said, master now points to the merge commit, which has two parents, G and Z).
    * A merge strategy is an algorithm that Git uses to decide how to perform a merge.
      * git merge --strategy=recursive
      * Git has a command named git rerere (which stands for “Reuse Recorded Resolu-
        tion”) that integrates with the normal git merge workflow to record the resolution of
        merge conflicts for later replay
    * git pull --rebase
    * Merging joins the history of two branches together with a merge commit (a commit
      with two parent commits);
    * and rebasing creates new, reparented commits on top of
      the existing commits.
* rebase
    * A rebase is a method of rewriting history in Git that is similar to a merge. A rebase
      involves changing the parent of a commit to point to another.
      * The rebase operation has changed the parent of the first commit in the separate-
        files branch to be the last commit in the master branch.
    * https://stackoverflow.com/questions/51608851/rebase-feature-branch-but-commit-ids-had-changed
    * https://www.algolia.com/developers-tech-blog/code-and-deep-dives/master-the-rebase-and-the-other-way-around
      * This is due to the way Git generates those commit hashes, which are not only based on the changes themselves, but also on the parent commit hash
    * The word rebase means to give a new parent
      to a branch.
    * The most important
      reason for using git rebase is to change
      the starting point of your local branches.
* stash
    * There are times when you may find yourself working on a new commit and want to
      temporarily undo your current changes but redo them at a later point.
      * Instead you can stash your
        uncommitted changes to store them temporarily and then be able to change
        branches, pull changes, and so on without needing to worry about these changes get-
        ting in the way.
      * git stash save creates a temporary commit with a prepopulated commit message and
        then returns your current branch to the state before the temporary commit was made.
      * It’s possible to access this commit directly, but you should only do so through git
        stash to avoid confusion.
      * You can see all the stashes that have been made by running git stash list . The
        output will resemble the following.
      * This is because the stashes are stored on a stack structure.
      * live in their own namespace refs/stash
      * When running git stash pop , the top stash on the stack ( stash@{0} ) is applied to the
        working directory and removed from the stack
      * If you wish to apply an item from the stack multiple times (perhaps on multiple
        branches), you can instead use git stash apply .
      * git stash clear
* git tag
    * A tag is another ref (or pointer) for a single commit.
      * Tags differ from branches in that they’re (usually) permanent
      * Rather than pointing
        to the work in progress on a feature, they’re generally used to describe a version of a
        software project.
      * For example, if you were releasing version 1.3 of your software project, you’d tag
        the commit that you release to customers as v1.3 to store that version for later use.
      * After you’ve tagged a version and verified that it’s point-
        ing to the correct commit and has the correct name, you can push it using git push-tags
      * This pushes all the tags you’ve created in the local repository to the remote
        repository.
    * git describe --tags
      * output: v0.1-1-g0a5e328
         v0.1 is the most recent tag on the current branch.
         1 indicates that one commit has been made since the most recent tag ( v0.1 ) on
        the current branch.
         g0a5e328 is the current commit SHA-1 prepended with a g (which stands for
        git ).
* git cherry-pick
    * Sometimes you may wish to include only a single commit from a branch onto the cur-
      rent branch rather than merging the entire branch.
      * git cherry-pick
      * WHY DOES THE SHA-1 CHANGE ON A CHERRY-PICK? Recall that the SHA-1 of a
        commit is based on its tree and metadata (which includes the parent commit
        SHA-1 ). Because the resulting master branch cherry-picked commit has a dif-
        ferent parent than the commit that was cherry-picked from the v0.1-release
        branch, the commit SHA-1 differs also.
* git diff
          * when you run
            git diff , you’re comparing the copy of the file in the working directory with the copy
            of the file in the staging area
* git revert
* git config
* ref
    * In Git, refs are the possible ways of addressing individual commits. They’re an easier
      way to refer to a specific commit or branch when specifying an argument to a Git
      command.
      * The first ref you’ve already seen is a branch (which is master by default if you
        haven’t created any other branches)
      * Branches are pointers to specific commits.
        * Ref-
          erencing the branch name master is the same as referencing the SHA-1 of the commit
          at the top of the master branch, such as the short SHA-1 6b437c7 in the last example.
        * Using branch names is quicker and easier to remember for referencing
          commits than always using SHA-1 s.
      * Refs can also have modifiers appended. Suffixing a ref with ~1 is the same as saying
        “one commit before that ref.”
        * For example, master~1 is the penultimate commit on the
          master branch
        * Another equivalent syntax
          is master^ , which is the same as master~1 (and master^^ is equivalent to master~2 )
      * The second ref is the string HEAD
        * HEAD always points to the top of whatever you cur-
           rently have checked out, so it’s almost always the top commit of the current branch
           you’re on
        * If you have the master branch checked out, then master and HEAD (and
          6b437c7 in the last example) are equivalent
        * The HEAD pointer points to the current branch.
        * The branch pointer points
          to a particular commit.
      * git rev-parse if you want to see what SHA-1 a given ref
        expands to
        * # git rev-parse master: 6b437c7739d24e29c8ded318e683eca8f03a5260
* HEAD
    * The last piece that you need for time travel is the device that does the travel. It
      turns out that Git supplies this device by default: it’s your HEAD
      * At its simplest, the HEAD is the current branch
      * This HEAD is analogous to the laser in
        a CD/DVD player, the needle of a record player, the tape player head in a cassette
        player, or that marker on a progress bar that you can move with your finger to any part
        of a song on your music-playing device
      * HEAD is also your actual head
        * What is your
          head (you) looking at right now? At the moment, you’re looking at the present, but in
          just a few paragraphs, you’ll move your head to another point in the timeline.
   * git rev-parse HEAD
     git rev-parse master
* revert
    * Reverting a previous commit: git revert


## recovery and history
* reset
    * There are times when you’ve made changes to files in the working directory but you
      don’t want to commit these changes.
      * Perhaps you added debugging statements to files and have now committed a fix, so you want to reset all the files that haven’t been com-
        mitted to their last committed state (on the current branch).
      * git reset --hard
        * git reset --hard <reglog-sha> // It will reset everything at that point of history
        * The --hard argument signals to git reset that you want it to reset both the index stag-
          ing area and the working directory to the state of the previous commit on this branch.
        * If run without an argument, it defaults to git reset --mixed , which resets the index
          staging area but not the contents of the working directory.
        * In short, git reset --mixed
          only undoes git add , but git reset --hard undoes git add and all file modifications.
    * git reset
      modifies the current branch pointer so it points to another commit. git
      checkout modifies the HEAD pointer so it points to another branch (or, rarely,
      commit)
      * If you’re on the master branch, git reset --hard v0.1-release
        sets the master branch to point to the top of the v0.1-release branch,
        whereas git checkout v0.1-release changes the current branch (the HEAD
        pointer) to point to the v0.1-release branch.
      * git commit --amend resets to the previous
        commit and then creates a new commit with the same commit message as the commit
        that was just reset
* reset, detached head
      * Resetting a branch to a previous commit: git reset
        * git reset HEAD^
        * # git reflog HEAD
            3e3c417 HEAD@{0}: reset: moving to HEAD^ // Commit reset
            4455fa9 HEAD@{1}: commit: Update preface. // New commit
            git reset 4455fa9
* git reflog
    * If everything is broken, you can use git reflog. It will show you a list of events that happened on your local repository. Copy the hash of the event before your mistake, and then run
    * Git’s reflog (or reference log) is updated whenever a commit pointer is updated (like a
      HEAD pointer or branch pointer)
      * This includes previous actions you’ve seen that don’t
        involve rewriting history, such as committing and changing branches.
      * They aren’t shared with other repositories when you git push and aren’t
        fetched when you git fetch .
      * As a result, they can only be used to see actions
        that were made in the Git repository on your local machine.
      * Basically every action you perform inside of Git where data is stored, you can find it inside of the reflog.
        * What this means is that you can use it as a safety net: you shouldn’t be worried that a merge, rebase, or some other action will destroy your work since you can find it again using this command.
        * The most common usage of this command is that you’ve just done a git reset and moved your HEAD back a few commits. But oops, you need that bit of code you left in the second commit.
          * What this shows is a list of commits that Git still has in its storage.
          * git log shows the current HEAD and its ancestry. That is, it prints the commit HEAD points to, then its parent, its parent, and so on. It traverses back through the repo's ancestry, by recursively looking up each commit's parent.
          * The reflog is an ordered list of the commits that HEAD has pointed to: it's undo history for your repo.
          * Find the 'sha' for the commit at the tip of your deleted branch using
            * git reflog
          * To restore the branch, use:
            * git checkout -b <branch> <sha>
* git log
* checkout, switch
    * Going back in time with git checkout
      * Normally, your HEAD is associated with a branch. When you move your HEAD
        around by using git checkout , you disassociate your HEAD from your current
        branch, which Git considers detached.
      * The master pointer (reference) doesn’t move. It always points to the last
        commit of the branch.
    * git checkout dev
      Check out isn’t like checking out a book from a library; in the context of Git,
      check out means changing the working directory to reflect the contents of
      the branch.
    * git checkout auto-
      matically associates local branches with their corresponding remote-tracking
      branches, if their names match
* Recall from technique 3 that each commit points to the previous (parent) commit
  and that this is repeated all the way to the branch’s initial commit.
  * As a result, the
    branch pointer can be used to reference not just the current commit it points to, but
    also every previous commit.
  * If a previous commit changes in any way, then its SHA-1
    hash changes too.
  * This in turn affects the SHA-1 of all its descendant commits.
  * This
    provides good protection in Git from accidentally rewriting history, because all com-
    mits effectively provide a checksum of the entire branch up until this point.
* Rewriting the entire history of a branch:
  git filter-branch
  * Perhaps you accidentally committed confidential files early
    in the project that you want to remove, or you want to split a large repository into mul-
    tiple smaller ones.
  * Git provides a tool called git filter-branch for these cases. It iterates through
    the entire history of a branch and lets you rewrite every commit as it does so.
    * This can
      be used to rewrite all the commits in an entire repository.

## branching
* is shown because the --set-upstream option was passed to git push . By passing
  this option, you tell Git that you want the local master branch you’ve just pushed to
  track the origin remote’s branch master . The master branch on the origin
  remote (which is often abbreviated origin/master ) is now known as the tracking
  branch (or upstream) for your local master branch.
* In Git, a branch is no more than a pointer to a particular
  commit.
* A tag is similar to a branch, but it points to a single commit and remains
  pointing to the same commit even when new commits are made. Typically tags are
  used for annotating commits; for example, when you release version 1.0 of your soft-
  ware, you may tag the commit used to build the 1.0 release with a 1.0 tag
  * This means
    you can come back to it in the future, rebuild that release, or check how certain things
    worked without fear that it will be somehow changed automatically.
* Instead of saying that the branch master consists of a set of com-
  mits, you can say that the branch master is the last commit.
* The
  last commit on a branch is called the tip of the branch, and it’s
  always the most recent commit made to a branch.
* This idea of pointing to a commit is known as a reference.

## submodules
* A Git repository can contain submodules. They allow you to reference other Git
  repositories at specific revisions.
* After this was done, it also created a .gitmodules file
  in the root of the repository’s working directory.
  C shows the file that contains the submodule metadata, such as the directory path
  and the URL
  * shows the new directory named submodule that was created to store the contents
     of the new submodule repository.
* Initializing all submodules can be done by running git submodule init , which
  copies all the submodule names and URL s from the .gitmodules file to the local repos-
  itory Git configuration file (in .git/config)
* git submodule status command
  * show the current states of all submodules of a repository
* git submodule update --init
  * For
    example, you may want to iterate through all the submodules in a repository (and
    their submodules) and run a Git command to ensure that they have all checked out
    the master branch and have fetched the latest remote repository commits or print sta-
    tus information.
  * git submodule foreach
  * git submodule foreach 'echo $name: $toplevel/$path [$sha1]'

## internals
* git remote --verbose
  origin  https://github.com/mtumilowicz/book-reports.wiki.git (fetch)
  origin  https://github.com/mtumilowicz/book-reports.wiki.git (push)
  * WHAT HAPPENS WHEN THE FETCH AND PUSH URLS DIFFER?
    * The documentation wasn't clear that "remote.<nick>.pushURL" and "remote.<nick>.URL" are there to name the
    * same repository accessed via different transports, not two separate repositories
      * example: ssh, https