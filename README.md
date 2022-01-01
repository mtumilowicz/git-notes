# git-notes

* references
    * https://www.manning.com/books/git-in-practice
    * https://www.manning.com/books/learn-git-in-a-month-of-lunches
    * https://git-scm.com/book/en/v2

## introduction
* repository
    * is the local collection of all the files related to a particular Git version
            control system and contains a .git subdirectory in its root
    * Git keeps track of the state of the files in the repository’s directory on disk
* `.git` directory
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
* commit
    * contains
        * message entered by the author
        * details of the commit author
        * unique commit reference
            * SHA-1 hashes such as `86bb0d659a39c98808439fadb8dbd594bec0004d`
            * everything in Git is checksummed before it is stored and is then referred to by that checksum
            * it’s impossible to change the contents of any file or directory without Git knowing about it
            * Git stores everything in its database not by file name but by the hash value of its contents
        * pointer to the preceding commit (parent commit)
        * date the commit was created
        * a pointer to the contents of files when the commit was made (object store context)
* object store
    * git stores all the history, branches, and commits locally
        * example: querying history doesn’t require a network connection
    * Git creates and stores a collection of objects when you commit
    * object store is stored inside the Git repository
    * the main Git objects: commits, blobs, tags, and trees
    * commit object contains a reference to the root tree object for this version
    * tree object contains:
        * references to blob objects for each file in the directory for this version
        * references to tree objects for each subdirectory of the directory for this version
    * blob object contains: contents of the file for this version
* index
    * Git doesn’t add anything to the index without your instruction
    * the first thing you have to do with a file you want to include in a Git repository
    is request that Git add it to the index
## basics
* Git has three main states that your files can reside in: modified,
  staged, and committed
  * The staging area is a file, generally contained in your Git directory, that stores information about
    what will go into your next commit.
    * Its technical name in Git parlance is the “index”, but the phrase
       “staging area” works just as well.
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
    * Adding the -a option to the git commit command makes
      Git automatically stage every file that is already tracked before doing the commit
* git amend
    * It’s important to understand that when you’re amending your last commit, you’re
      not so much fixing it as replacing it entirely with a new, improved commit that
      pushes the old commit out of the way and puts the new commit in its place.
* git add, git commit
    * It
      turns out that Git stages a file exactly as it is when you run the git add command. If you commit
      now, the version of CONTRIBUTING.md as it was when you last ran the git add command is how it will
      go into the commit, not the version of the file as it looks in your working directory when you run
      git commit. If you modify a file after you run git add, you have to run git add again to stage the
      latest version of the file
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
    * While the git fetch command will fetch all the changes on the server that you don’t have yet, it will
      not modify your working directory at all.
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
      * It’s important to note that the git fetch command only downloads
        the data to your local repository - it doesn’t automatically merge it with any of your work or
        modify what you’re currently working on
* merge
    * A merge results in a commit that has two (or even more) parent
      commits.
    * merge commit has two parents: the latest commit from the
      master branch and the latest commit from the bugfix branch
    * One special case of merging in Git is the fast-forward merge. This special case takes
      effect when the target branch is a descendant of the branch that it will merge with.
    * fast-forward
        * Because the commit C4 pointed to by the
          branch hotfix you merged in was directly ahead of the commit C2 you’re on, Git simply moves the
          pointer forward.
          * To phrase that another way, when you try to merge one commit with a commit
            that can be reached by following the first commit’s history, Git simplifies things by moving the
            pointer forward because there is no divergent work to merge together — this is called a “fast-
            forward.”
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
    * Because the commit on the branch you’re on isn’t a
      direct ancestor of the branch you’re merging in, Git has to do some work.
      * In this case, Git does a
        simple three-way merge, using the two snapshots pointed to by the branch tips and the common
        ancestor of the two.
      * Instead of just moving the branch pointer forward, Git creates a new snapshot that results from this
        three-way merge and automatically creates a new commit that points to it.
        * This is referred to as a
          merge commit, and is special in that it has more than one parent.
* rebase
    * With the rebase command, you can take all the
      changes that were committed on one branch and replay them on a different branch.
      * git rebase --onto master client // applies commits from client to the master since diverged
      * Now you can fast-forward your master branch
        * git checkout master
        * git merge client
      * git rebase master server
        * This replays your server work on top of your master work
        * Then, you can fast-forward the base branch
          $ git checkout master
          $ git merge server
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
    * Git has the ability to tag specific points in a repository’s history as being important.
      * people use this functionality to mark release points (v1.0, v2.0 and so on)
      * Git supports two types of tags: lightweight and annotated.
        * A lightweight tag is very much like a branch that doesn’t change — it’s just a pointer to a specific
          commit.
        * Annotated tags, however, are stored as full objects in the Git database.
          * They’re checksummed;
            contain the tagger name, email, and date; have a tagging message; and can be signed and verified
            with GNU Privacy Guard (GPG)
          * It’s generally recommended that you create annotated tags so you
            can have all this information; but if you want a temporary tag or for some reason don’t want to
            keep the other information, lightweight tags are available too.
      * By default, the git push command doesn’t transfer tags to remote servers.
        * You will have to explicitly
          push tags to a shared server after you have created them.
        * git push origin <tagname>
        * If you have a lot of tags that you want to push up at once, you can also use the --tags option
      * If you want to view the versions of files a tag is pointing to, you can do a git checkout of that tag
        * git checkout v2.0.0
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
* Remember, anything that is committed in Git can almost always be recovered. Even commits that
  were on branches that were deleted or commits that were overwritten with an --amend commit can
  be recovered
  * However, anything you lose that was never
    committed is likely never to be seen again.
* reset
    * An easier way to think about reset and checkout is through the mental frame of Git being a content
      manager of three different trees.
      * By “tree” here, we really mean “collection of files”, not specifically
        the data structure
      * Tree                  Role
        HEAD                  Last commit snapshot, next parent
        Index                 Proposed next commit snapshot // staging area
        Working Directory     Sandbox
    * reset
      * The first thing reset will do is move what HEAD points to
      * This isn’t the same as changing HEAD
        itself (which is what checkout does)
      * reset moves the branch that HEAD is pointing to
        * This means if
          HEAD is set to the master branch (i.e. you’re currently on the master branch), running git reset
          9e5e6a4 will start by making master point to 9e5e6a4
      1. Move the branch HEAD points to (stop here if --soft).
      2. Make the index look like HEAD (stop here unless --hard).
      3. Make the working directory look like the index.
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
    * In “detached HEAD” state, if you make changes and then create a commit, the tag will stay the same,
      but your new commit won’t belong to any branch and will be unreachable, except by the exact
      commit hash.
      * Resetting a branch to a previous commit: git reset
        * git reset HEAD^
        * # git reflog HEAD
            3e3c417 HEAD@{0}: reset: moving to HEAD^ // Commit reset
            4455fa9 HEAD@{1}: commit: Update preface. // New commit
            git reset 4455fa9
* git reflog
    * Often, the quickest way is to use a tool called git reflog.
      * As you’re working, Git silently records
        what your HEAD is every time you change it.
      * Each time you commit or change branches, the reflog
        is updated.
      * reflog data is kept in the .git/logs/ directory
      * suppose rm -Rf .git/logs/
        * How can
          you recover that commit at this point
        * One way is to use the git fsck utility, which checks your
          database for integrity. If you run it with the --full option, it shows you all objects that aren’t
          pointed to by another object:
        * In this case, you can see your missing commit after the string “dangling commit”. You can recover it
          the same way, by adding a branch that points to that SHA-1.
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
    * Running git checkout [branch] is pretty similar to running git reset --hard [branch] in that it
      updates all three trees for you to look like [branch], but there are two important differences.
      * First, unlike reset --hard, checkout is working-directory safe; it will check to make sure it’s not
        blowing away files that have changes to them.
      * Actually, it’s a bit smarter than that — it tries to do a
        trivial merge in the working directory, so all of the files you haven’t changed will be updated
      * reset
        --hard, on the other hand, will simply replace everything across the board without checking.
    * The second important difference is how checkout updates HEAD. Whereas reset will move the
      branch that HEAD points to, checkout will move HEAD itself to point to another branch.
      * For instance, say we have master and develop branches which point at different commits, and we’re
        currently on develop (so HEAD points to it)
        * If we run git reset master, develop itself will now point
          to the same commit that master does. If we instead run git checkout master, develop does not move,
          HEAD itself does. HEAD will now point to master.
    * git checkout -- CONTRIBUTING.md // discard changes
      * git restore CONTRIBUTING.md
      * git restore is a new command that has been introduced (last summer) in Git 2.23 together with git switch. Their purposes are to simplify and separate the use cases of git checkout that does too many things.
        * https://stackoverflow.com/questions/61130412/what-is-the-difference-between-git-checkout-vs-git-restore-for-reverting-un
        * git checkout can be used to switch branches (and also to create a new branch before switching to it). This functionality has been extracted into git switch.
        * git checkout can also be used to restore files to the state they were on a specified commit. This functionality has been extracted into git restore.

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
* Tracking branches are
  local branches that have a direct relationship to a remote branch. If you’re on a tracking branch
  and type git pull, Git automatically knows which server to fetch from and which branch to merge
  in.
* A branch in Git is simply a lightweight movable pointer to one of these commits
  * How does Git know what branch you’re currently on? It keeps a special pointer called HEAD
  * when you switch branches, Git resets your working directory to look like it did the last time you
    committed on that branch
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
* It often happens that while working on one project, you need to use another project from within it.
* Submodules allow you to keep a Git repository as a
  subdirectory of another Git repository.
* git submodule update command to fetch changes from the submodule
  repositories
* git submodule update --remote will only update the branch registered in the .gitmodule, and by default, you will end up with a detached HEAD, unless --rebase or --merge is specified
* git submodule foreach 'git stash'
  * run some arbitrary command in each submodule
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
* A remote repository is generally a bare repository — a Git repository that has no working directory.
  * Because the repository is only used as a collaboration point, there is no reason to have a snapshot
    checked out on disk; it’s just the Git data.
  * In the simplest terms, a bare repository is the contents of
     your project’s .git directory and nothing else.
* Git doesn’t store data as a series of changesets or
  differences, but instead as a series of snapshots.
  * When you make a commit, Git stores a commit object that contains a pointer to the snapshot of the
    content you staged.
  * This object also contains the author’s name and email address, the message that
     you typed, and pointers to the commit or commits that directly came before this commit (its parent
     or parents): zero parents for the initial commit, one parent for a normal commit, and multiple
     parents for a commit that results from a merge of two or more branches.
  * Staging the files computes a checksum for each one (the SHA-1 hash we mentioned
    in What is Git?), stores that version of the file in the Git repository (Git refers to them as blobs), and
    adds that checksum to the staging area:
  * When you create the commit by running git commit, Git checksums each subdirectory (in this case,
    just the root project directory) and stores them as a tree object in the Git repository.
  * Git then creates
    a commit object that has the metadata and a pointer to the root project tree so it can re-create that
    snapshot when needed.
  * Your Git repository now contains five objects: three blobs (each representing the contents of one of
    the three files), one tree that lists the contents of the directory and specifies which file names are
    stored as which blobs, and one commit with the pointer to that root tree and all the commit
    metadata.

![alt text](img/commit1.png)
* If you make some changes and commit again, the next commit stores a pointer to the commit that
  came immediately before it
  ![alt text](img/commit2.png)

* git remote --verbose
  origin  https://github.com/mtumilowicz/book-reports.wiki.git (fetch)
  origin  https://github.com/mtumilowicz/book-reports.wiki.git (push)
  * WHAT HAPPENS WHEN THE FETCH AND PUSH URLS DIFFER?
    * The documentation wasn't clear that "remote.<nick>.pushURL" and "remote.<nick>.URL" are there to name the
    * same repository accessed via different transports, not two separate repositories
      * example: ssh, https
* Here’s what a newly-
  initialized .git directory typically looks like:
  * ls -F1
  * config
    description
    HEAD
    hooks/
    info/
    objects/
    refs/
* The description file is used only by the GitWeb
  program, so don’t worry about it.
* The config file contains your project-specific configuration
  options, and the info directory keeps a global exclude file for ignored patterns that you don’t want
  to track in a .gitignore file
* The hooks directory contains your client- or server-side hook scripts,
  which are discussed in detail in Git Hooks.
* The objects directory stores all the content for your
  database, the refs directory stores pointers into commit objects in that data (branches, tags,
  remotes and more), the HEAD file points to the branch you currently have checked out, and the index
  file is where Git stores your staging area information.
* Git Objects
  * Git is a content-addressable filesystem
  * It means that at the core of Git
    is a simple key-value data store
  * What this means is that you can insert any kind of content into a
    Git repository, for which Git will hand you back a unique key you can use later to retrieve that
    content.
  * Now, let’s use git hash-object to create a new data object and manually store it in
    your new Git database
    * echo 'test content' | git hash-object -w --stdin
      d670460b4b4aece5915caf5c68d12f560a9fe3e4
    * git hash-object would take the content you handed to it and merely return the
       unique key that would be used to store it in your Git database. The -w option then tells the command
       to not simply return the key, but to write that object to the database
    * Finally, the --stdin option tells
      git hash-object to get the content to be processed from stdin
    * find .git/objects -type f
      .git/objects/d6/70460b4b4aece5915caf5c68d12f560a9fe3e4
      * This is how Git stores the content initially — as a single file per piece of content, named
         with the SHA-1 checksum of the content and its header
  * git cat-file command
    * command is sort of a Swiss army knife for inspecting Git objects
* Tree Objects
  * is the tree, which solves the problem of storing the
    filename and also allows you to store a group of files together
  * Git stores content in a manner
    similar to a UNIX filesystem, but a bit simplified
  * All the content is stored as tree and blob objects,
    with trees corresponding to UNIX directory entries and blobs corresponding more or less to inodes
    or file contents
  * A single tree object contains one or more entries, each of which is the SHA-1 hash
    of a blob or subtree with its associated mode, type, and filename
  * git cat-file -p master^{tree}
    * master^{tree} syntax specifies the tree object that is pointed to by the last commit on your
    master branch
    ![alt text](img/git/data_model.png)
  * Git normally creates a tree by taking the state of your
    staging area or index and writing a series of tree objects from it
* Commit Objects
  * you now have three trees that represent the different snapshots of
    your project that you want to track, but the earlier problem remains: you must remember all three
    SHA-1 values in order to recall the snapshots
  * To create a commit object, you call commit-tree and specify a single tree SHA-1 and which commit
    objects, if any, directly preceded it
  * The format for a commit object is simple: it specifies the top-level tree for the snapshot of the
    project at that point; the parent commits if any (the commit object described above does not have
    any parents); the author/committer information (which uses your user.name and user.email
    configuration settings and a timestamp); a blank line, and then the commit message.
* Object Storage
  * Git concatenates the header and the original content and then calculates the SHA-1 checksum of
    that new content
* Git References
  * find .git/refs
    * .git/refs
      .git/refs/heads
      .git/refs/tags
  * echo 1a410efbd13591db07496601ebc7a059dd55cfe9 > .git/refs/heads/master
    * Now, you can use the head reference you just created instead of the SHA-1 value in your Git
      commands
    * Git provides the safer command
      git update-ref to do this if you want to update a reference
      * git update-ref refs/heads/master 1a410efbd13591db07496601ebc7a059dd55cfe9
    * That’s basically what a branch in Git is: a simple pointer or reference to the head of a line of work
    * To create a branch back at the second commit, you can do this
      * git update-ref refs/heads/test cac0ca
    * When you run commands like git branch <branch>, Git basically runs that update-ref command to
      add the SHA-1 of the last commit of the branch you’re on into whatever new reference you want to
      create.
* HEAD
  * when you run git branch <branch>, how does Git know the SHA-1 of the last
    commit? The answer is the HEAD file
  * Usually the HEAD file is a symbolic reference to the branch you’re currently on
  * By symbolic
    reference, we mean that unlike a normal reference, it contains a pointer to another reference
  * However in some rare cases the HEAD file may contain the SHA-1 value of a git object
    * This happens
      when you checkout a tag, commit, or remote branch, which puts your repository in "detached
      HEAD" state.
  * cat .git/HEAD // ref: refs/heads/master
    * If you run git checkout test
    * cat .git/HEAD // ref: refs/heads/test
  * git symbolic-ref HEAD // read the value of your HEAD
    * git symbolic-ref HEAD refs/heads/test // set the value of HEAD
* Tags
  * The tag object is very much like a commit object — it contains a tagger, a date, a message,
    and a pointer
  * The main difference is that a tag object generally points to a commit rather than a
    tree
  * It’s like a branch reference, but it never moves — it always points to the same commit but
    gives it a friendlier name
  * If you create an annotated tag, Git creates a tag object and then writes a reference to
    point to it rather than directly to the commit
  * Also notice that it
    doesn’t need to point to a commit; you can tag any Git object
* Remotes
  * If you add a remote and push to it,
    Git stores the value you last pushed to that remote for each branch in the refs/remotes directory
* Packfiles
  * Wouldn’t it be nice if Git could store one of them in full but then the second object only as the delta
    between it and the first?
* Removing Objects
  * However, if someone at any point in the history of your project added a single huge file, every clone
    for all time will be forced to download that large file, even if it was removed from the project in the
    very next commit. Because it’s reachable from the history, it will always be there.