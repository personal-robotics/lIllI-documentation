
# General Information for Workspace Setups

Miscellaneous information related to setting up ROS workspaces.

## Build a Workspace

This general set of instructions builds a workspace, using the lilli_bot tutorial as an example:

```
# start from personal-robotics-testbench repository root directory
cd personal-robotics-testbench

# change to workspace
cd tutorials/lilli_ws

# import dependencies
# NOTE: cache your git credentials so you do not have to reenter them 
vcs import src < src/packages.repos

# make sure package database is up-to-date for next step
sudo apt-get update 

# install additional dependencies (if any)
rosdep install --from-paths src --ignore-src -r -y

# build workspace
colcon build --symlink-install

# source workspace 
source install/setup.bash
```

If you ever extend your robot description files or add other packages, rebuild the workspace to ensure new files are installed properly:

```
# build workspace
colcon build --symlink-install

# source workspace 
source install/setup.bash
```

Updates to existing files should be loaded automatically, hence no rebuild is necessary for updated files.

### Troubleshooting

A warning may occur from `rosdep install --from-paths src --ignore-src -r -y`

```
ERROR: your rosdep installation has not been initialized yet.  Please run:

    rosdep update
```

This can be ignored.

## Python Virtual Environment

Note: ROS2 might cause problems when used with Conda. 
If you are experiencing issues with Conda, try using Python virtual environments.

If a package contains a `requirements.txt` file, it is recommended to setup a virtual environment for Python code within this package.

```
# install virtualenv
sudo apt install python3-virtualenv python3-pip

# change to the directory containing the requirements.txt
cd tutorials/lilli_ws

# create virtual environment
python3 -m virtualenv .venv

# activate venv 
# NOTE: do this in every terminal before running python scripts from this package
source .venv/bin/activate

# install packages listed in file 
pip install -r requirements.txt
```

### Python Virtual Environment in VS Code

Run VS Code from the top-level tutorial folder, i.e.

```
cd tutorials/lilli_bot
code .
```

Launch Command Palette with Ctrl+Shift+P, enter "Select Python Interpeter",
select python executable in the .venv folder

If running VS Code for the first time, select 'File / Save Workspace as ...' from the menu, and save using the default file name.
