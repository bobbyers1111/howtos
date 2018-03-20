***
# git cheatsheet
***
### General Workflow..

1. Fetch and merge changes from the remote
2. Create a branch to work on a new project feature
3. Develop the feature on your branch and commit your work
4. Fetch and merge from the remote again (in case new commits were made while you were working)
5. Push your branch up to the remote for review
***

### Git Objects

>Git objects are stored in the *Object Store* and consists of the following objects..

1. The **Commit Object**
    1. Is a small text file
    1. Is all git needs to rebuild the full contents of a commit
    1. Stores the following information..
        1. Commit user information
        1. Commit message
        1. Reference to the commit's parent(s)
        1. Reference to the root tree of the project

2. **Annotated tag**
> Is a reference to a specific commit

3. **Tree**
> List of directories and files inside a project

4. **Blob**
> Contents of a any file being managed by git

***
### Git IDs

* A git ID is the name of a git object

* Stored as a 40-char hex string

* Often displayed as only the first 7 characters

* You may use the first few ID chars (min 4) in git so long as it is unique within the repository

* Uses SHA1 algorithm

* Can manually compute a SHA1 for a file with..

>git hash-object *file*

***
### References, Labels, Tags..

- **Master:** Default name for the *main branch*
- **Branch Label:** points to the most recent commit in a branch
- **HEAD:** A reference to the *current* commit.
  - Usually points to branch label of current branch.
  - only *one* HEAD per repository

        % git log --decorate=full
        commit d99bc0c533a61f97fcb85ff44d18d20e4f86053a (HEAD -> refs/heads/master, refs/remotes/origin/master)
        Author: Bob Byers <robertbyers@post.harvard.edu>
        Date:   Mon Mar 19 13:16:02 2018 -0400
- **References:** are user-friendly names that point to..
  - A commit SHA1 hash, or
  - Another reference
- **Tilde shortcuts**
   - Tilde ('~') refers to the *previous* (i.e., *parent*) commit
   - Tilde-two ('~2') refers to the parent's parent
   - Tilde-n ('~n') refers to the nth parent
   - Multiple tildes same as ~n

            % git log --oneline --graph HEAD
            627aefc More mods to refs section (6)
            443abaf More mods to refs section (5)
            58acf16 More mods to refs section (4)
            cd6a632 More mods to refs section (3)
            84a09ad More mods to refs section (2)
            f3cfbc6 More mods to refs section
            d99bc0c More info added to references section
            c2d0138 Add references section
            8dfad10 New file gitnotes.md
            1a9f006 Initial commit

            % git log --oneline --graph 58acf16
            58acf16 More mods to refs section (4)
            cd6a632 More mods to refs section (3)
            84a09ad More mods to refs section (2)
            f3cfbc6 More mods to refs section
            d99bc0c More info added to references section
            c2d0138 Add references section
            8dfad10 New file gitnotes.md
            1a9f006 Initial commit
 
            % git log --oneline --graph 58acf16~~~
            f3cfbc6 More mods to refs section
            d99bc0c More info added to references section
            c2d0138 Add references section
            8dfad10 New file gitnotes.md
            1a9f006 Initial commit
- **Carrat ('^') character**
   - Refers to a parent in a *merge commit*
   - ^ or ^1: first parent of the merge commit
   - ^^: first parent's first parent
   - ^2 second parent of the **MERGE** commit (not grandparent)
   - If the current branch is master, then the commit has only one parent and there is no second parent - hence an error would be returned if you specify HEAD^2
   - Can be combined with '~'. "HEAD~^2" is the *parent's second parent*
- **Tags**
    - Use tags in place of branch labels or git IDs in git commands
    - *Lightweight Tag* simple reference to a commit
    - *Annotated Tag* is a full git object containing..
        - Reference to the commit
        - Tag author
        - Tag date/time
        - Tag message
        - Commit ID
        - (optional) signed with GNU Privacy Guard (GPG)
    - Use 'git tag' to display existing tags
    - Use 'git show *&<tag&>*' to display details for a specific tag
    - NOTE: 'git push' does not automatically push tags. Use '--tags' to make sure they do get pushed.


***
### Git remote, local, terminology, misc.
- Two fundamental starting scenarios..
- - Already have a local repository => add the remote
>(the add command also sets up an alias for the remote; e.g., 'origin')  
>>git remote add origin https://github.com/bobbyers1111/reposC.git  
>>git push -u origin master  
>>
- - Already have a remote repository => clone the remote
>>git clone https://bitbucket.org/atlassian_tutorial/helloworld.git  
>>

- A *clone* is a local copy of a remote repository

- *origin* is an alias for the remote repository's URL

- Everything needed to access the remote is in the .git/config file. Example..

>    [core]  
>>	    bare = false  
>>	    repositoryformatversion = 0  
>>	    filemode = true  
>>	    logallrefupdates = true  

>    [remote "origin"]  
>>	    url = https://github.com/bobbyers1111/bobcheat.git  
>>	    fetch = +refs/heads/*:refs/remotes/origin/*  

>    [branch "master"]  
>>	    remote = origin  
>>	    merge = refs/heads/master  

***
#### Branches

To create a branch..
    git branch branchname

To switch to the branch..
    git checkout branchname

To see a list of branches and which one you are working on..
    get branch

***
#### Merging
- Fast Forward Merge
- Merge Commit
- Squash Merge
- Rebase

#### Merge conflicts
1. Checkout master
1. Merge the feature branch into master
    1. git detects the conflict
    1. git modifies the file, adding conflict markers
1. Manually resolve the conflict (in the master branch)
1. Stage the file
1. Commit the merge commit
1. (optional) Delte the feature branch

***
### How to..

#### Create a *new* repository from command line..

    FIRST STEP: Create an empty repository on the server (I don't know if this can be done from cmd line)
    
    echo "# reposC" >> README.md
    git init
    git add README.md
    git commit -m "first commit"
    git remote add origin https://github.com/bobbyers1111/reposC.git
    git push -u origin master

#### Push to an *existing* repository from the command line

    git remote add origin https://github.com/bobbyers1111/reposC.git
    git push -u origin master

***
### Commands Overview..

#### git help *gitcmd*
>Displays in-depth help

#### git *gitcmd* -h
>Displays command line syntax only

#### git add
>Stages files from the working directory to the staging area

#### git branch
>Displays the branch upon which you're currently working

#### git branch branchname
>Creates a new branch

#### git branch -d branchname
>Deletes an old branch

#### git checkout branchname
>Changes current working branch to branchname

#### git checkout HEAD
>Will OVERWRITE any changes to the local file and retrieve the most recent committed version from the repository.

#### git clone remote local
>Makes a local copy of a remote branch

#### git commit
>Permanently stores file changes from the staging area in the repository

#### git config
>Get and set repository or global options.

>>--system: apply command to every repository for every user on current system.

>>--global: applies to every repository for the current user on current system.

>>--local: applies only to the current repository (highest precedence).

#### git config core.editor <editor>
>Set editor for git operations requiring an editor

#### git diff
>Shows the difference between the working directory and the staging area

#### git fetch
>Retrieve any changes that may have been made to the clone's origin. Changed files are placed in a REMOTE BRANCH.

#### git init
> Creates a new Git repository

#### git log
>Shows a list of all previous commits

#### git merge branchname
>Merges changes from branchname into the current working branch.

#### git push [-u] remote localbr
>Pushes your local changes to the branch specified by *remote*  
>*remote* can be either an alias (e.g., *origin*) or a full URL  
>Use '-u' to enable tracking so git will warn you of out-of-sync files  

#### get remote -v
>Lists the known remotes of the files in the current working repository. Each file listed at least twice - once as a fetch, the other as a push.

#### git reset HEAD filename
>This command resets the file in the staging area to be the same as the HEAD commit. It does not discard file changes from the working directory, it just removes them from the staging area.

#### git reset SHA
>Resets to a previous commit identified by the first 7 digits of its SHA from 'git log'. Conceptually..

              Before reset..
              [A] ------ [B] ------ [C] ------ [D] ------ [HEAD]
              
              After reset..
              [A] ------ [B] ------ [HEAD]

#### git show HEAD
>Display everything the git log command displays for the HEAD commit, plus all the file changes that were committed.

#### git status
>Inspects the contents of the working directory and staging area
