The following is a guide for converting SVN to GIT. It includes steps to bringing any externals into the history as part of the mail repo. It should maintain history of the main project and the externals. It does not use submodules. If you need submodules, this is not the guide for you. 


## Get Required External URLS

- `cd [YOUR SVN PROJECT]` - Go to you checkout out SVN version of your project
- `svn propget svn:externals -R . > externals.txt` - Produce a set of required externals
- Open `externals.txt', should look something like:

    `httpdocs/library - img https://svn.mildfuzzz.com/project/branches/externals/img`

## Clone project and each of your externals to seperate folders

 - `mkdir [wrapper folder] && cd [wrapper folder]`
 - clone each of the externals in your externals txt:
   - `git svn clone [SVN URL] [desired folder]` so in my example
   - `git svn clone https://svn.mildfuzzz.com/project/branches/externals/img img`
   - repeat previous step for all externals
   - clone project, using the same syntax as above:
    - `git svn clone [YOUR SVN PROJECT REPO TRUNK URL] [PROJECT FOLDER]`
    - e.g `git svn clone https://svn.mildfuzzz.com/project/trunk baseProject`

   - you should now have a folder project like:

     - wrapper
       - project
       - external 1
       - external 2
       - etc

## Clean up external folder structure

This step is needed for each of you externals.

The folder structure of your externals needs to match there location of the project. That is to say if you have an external with a project with a structure like"

  - img
  - js
  - css

but within your project, you have a structure:

 - httpdocs
   - library
     - img
     - js
     - css

you need to move the contents of the external into `httpdocs/library`

Note: searching for like for like files in your the original SVN project will help you identify location.

Commit the changes, using --amend -no-edit to squash the the change onto the previous commit.
Doing so will add the folder structure to the original commit, meaning that the history should work as expected when exploring.

- `git commit --amend --no-edit`

## Add repo's to your project and rebase

- `cd [PROJECT FOLDER]`
- For each eternal:
	- Add as remote
		- `git remote add REMOTE_TITLE REMOTE_LOCAL_PATH`
		- e.g. `git remote add img ../img`
	- Pull and rebase
		- `git pull --rebase [REMOTE TITLE]`
		- e.g. `git pull --rebase img`
	- [optional] Remove remotes
		- `git remote remove [REMOTE TITLE]`
		- e.g. `git remote remove img`







