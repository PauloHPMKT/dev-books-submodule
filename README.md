# dev-books-submodule

Initialize submodules:
* run git submodule init to initialize your local configuration file
* run git submodule update to fetch all data from the project and check out the appropriate commit listed in your superproject

Another equivalent commands to initialize submodules:
<!-- initialize submodules for all project in this context -->
git submodule update --init --recursive --remote
<!-- set a default branch (master ou main) for all projects into submodule -->
git submodule foreach git checkout master

Set initialize automaticaly submodules:
git clone --recurse-submodules https://github.com/chaconinc/MainProject

If you already cloned the project and forgot --recurse-submodules, you can combine the commands git submodule init and git submodule update steps by running: 
git submodule update --init => To also initialize, fetch and checkout any nested submodules, you can use the foolproof git submodule update --init --recursive.

##
<strong>Pulling Upstream Changes from the Project Remote</strong>
Let’s now step into the shoes of your collaborator, who has their own local clone of the MainProject repository. 
Simply executing git pull to get your newly committed changes is not enough:

Ex:
$ git pull
From https://github.com/chaconinc/MainProject
   fb9093c..0a24cfc  master     -> origin/master
Fetching submodule DbConnector
From https://github.com/chaconinc/DbConnector
   c3f01dc..c87d55d  stable     -> origin/stable
Updating fb9093c..0a24cfc
Fast-forward
 .gitmodules         | 2 +-
 DbConnector         | 2 +-
 2 files changed, 2 insertions(+), 2 deletions(-)

$ git status
 On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   DbConnector (new commits)

Submodules changed but not updated:

* DbConnector c87d55d...c3f01dc (4):
  < catch non-null terminated lines
  < more robust error handling
  < more efficient db routine
  < better connection routine

no changes added to commit (use "git add" and/or "git commit -a")
By default, the git pull command recursively fetches submodules changes, as we can see in the output of the first command above. However, it does not update the submodules. This is shown by the output of the git status command, which shows the submodule is “modified”, and has “new commits”. What’s more, the brackets showing the new commits point left (<), indicating that these commits are recorded in MainProject but are not present in the local DbConnector checkout. To finalize the update, you need to run git submodule update:

$ git submodule update --init --recursive
Submodule path 'vendor/plugins/demo': checked out '48679c6302815f6c76f1fe30625d795d9e55fc56'

$ git status
 On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working tree clean

</hr>
<strong>Submodule Foreach</strong>
There is a foreach submodule command to run some arbitrary command in each submodule. This can be really helpful if you have a number of submodules in the same project.

For example, let’s say we want to start a new feature or do a bugfix and we have work going on in several submodules. We can easily stash all the work in all our submodules.

Ex:
$ git submodule foreach 'git stash'
Entering 'CryptoLibrary'
No local changes to save
Entering 'DbConnector'
Saved working directory and index state WIP on stable: 82d2ad3 Merge from origin/stable
HEAD is now at 82d2ad3 Merge from origin/stable
Then we can create a new branch and switch to it in all our submodules.

$ git submodule foreach 'git checkout -b featureA'
Entering 'CryptoLibrary'
Switched to a new branch 'featureA'
Entering 'DbConnector'
Switched to a new branch 'featureA'

<a href="https://git-scm.com/book/en/v2/Git-Tools-Submodules">Link to Git documentation</a>


Obs: 
* when updating local project with submodule is necessary update the main project into submodule too!
* every project keep your own commit tree