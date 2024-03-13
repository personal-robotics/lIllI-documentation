# Repository Maintenance

## Git Configuration

### Git User Configuration

```
git config --global user.name "Your Name"
git config --global user.email  "email@example.com"
```

### Github Personal Access Token

see: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token


### Cache Git Login Credentials

Without caching credentials, command `vcs` fails when cloning multiple repositories, presenting multiple login prompts at once.

https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git

Install github client `sudo apt install gh`
Run `gh auth login`, select GitHub.com, HTTPS, Authenticate with GitHub credentials, Paste an authentication token

The minimum required scopes for caching login credentials are 'repo', 'read:org', 'workflow' (see below). You can update the token scopes on Github if it does not meet the requirements here https://github.com/settings/tokens

### User Credentials

Needed for various commands, e.g. `git subtree`

```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

### Handle Line Endings

from https://docs.github.com/en/get-started/getting-started-with-git/configuring-git-to-handle-line-endings

TODO check if we should set autocrlf to false (most IDEs should be able to handle line endings correctly)

```
git config --global core.autocrlf false
```

### Handling Merges 

TODO 
https://stackoverflow.com/questions/13846300/how-to-make-git-pull-use-rebase-by-default-for-all-my-repositories

## Repository Creation

### Add External Repositories as Subtrees

for details, see: https://www.atlassian.com/git/tutorials/git-subtree


Adding cu-lilli:

```
# add external repo as remote
git remote add -f cu-lilli https://github.com/Robotics-DAI-FMFI-UK/cu-lIllI.git

# add remote as subtree
# NOTE: master refers to the external repository branch, other repos might use main instead of master
git subtree add --prefix external/cu-lilli cu-lilli master # --squash
```

Adding ROS 2 Book:

```
git remote add -f book_ros2 https://github.com/fmrico/book_ros2.git
git subtree add --prefix external/bookros2_ws/src book_ros2 humble-devel
```

### Update External Repositories

Update the external repository e.g. with:

```
git fetch cu-lilli main
git subtree pull --prefix external/cu-lilli cu-lilli master # --squash
```

## Repository Access

### Creating a Personal Access Token

see: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token


## Merging Changes

The recommended setting for merging changes in git is rebasing local changes. This creates a cleaner timeline in the commit history. 
TBD should this be mandatory for external contributions? See https://stackoverflow.com/questions/13846300/how-to-make-git-pull-use-rebase-by-default-for-all-my-repositories for a discussion

Set with `git config --global pull.rebase true`


### Creating a Private Fork of a Public Repository on Github

This is not directly supported via the Github web site. Instead, follow this workflow: (from https://stackoverflow.com/a/30352360)

Create private repo on github, e.g. stf976/adafruit-pca9685-ros-controller
Use the same license as the cloned repo, or leave blank
Rename the main branch of your private repository to match the one you fork, if necessary 
Otherwise `git push` will issue an error `! [remote rejected] main (refusing to delete the current branch: refs/heads/main)`

```
# Clone repository you wish to fork
git clone --bare https://github.com/joshnewans/diffdrive_arduino

cd diffdrive_arduino.git/

# push to private repository
git push --mirror https://github.com/stf976/adafruit-pca9685-ros-controller

# remove temp files
cd ..
rm -rf diffdrive_arduino.git/
```

## Repository Maintenance

### Repository with Symlinks

should probably be avoided because of extra complexity, enable only if necessary

see https://stackoverflow.com/questions/56504821/sharing-git-repo-symlinks-between-windows-and-wsl
and https://github.com/git-for-windows/git/wiki/Symbolic-Links

## Repository Layout

subject to change, currently the following layout applies

- `README.md`: start and index page for documentation
- `docs/`: general documentation
- `docs/tutorials`: tutorial specific documentation, one subdirectory per tutorial (should match name in `tutorials/` directory)
- `tutorials/`: one subdirectory per tutorial, each subdirectory should be usable as a ROS workspace

### Tutorials

- `tutorials/<tutorial-name>`: should contain build, install, log directories when workspace is built (use .gitignore to exclude from commits)
- `tutorials/<tutorial-name>/src`: all source packages for the tutorial go here
- `tutorials/<tutorial-name>/src/package.repos`: list of external packages, to be used with `vcs`
`tutorials/<tutorial-name>/.vscode`: contains recommended default settings for using VS Code with this workspace

#### VS Code Settings
To create initial files in the `.vscode` directory, run VS Code in the tutorial subdirectory, e.g.

```
cd tutorials/lilli_ws
code .
```

Then, in VS Code, from the menu, select 'File / Save Workspace as ...' and use the default file name. 