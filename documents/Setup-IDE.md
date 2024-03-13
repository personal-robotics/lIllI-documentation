# IDE Setup

The recommended Integrated Development Environment is Visual Studio Code.

For Arduino, the Arduino IDE is recommended.
Aternatively, you can use the PlatformIO plugin for VSCode.

Recommendations or suggestions for other development tools are listed here as well.

## Visual Studio Code 

https://code.visualstudio.com/docs

### Installation

#### Installation from Download 
available for Windows, Linux, MacOS: https://code.visualstudio.com/Download

User installer does not require admin privileges

For Linux, download .deb package from website matching your architecture

Installation (from: https://code.visualstudio.com/docs/setup/linux)

open with Software Center, Install (not tested), or
on command line in download folder, run `sudo apt install ./code_*.deb`

#### Installation with Snap (on Ubuntu)

`sudo snap install --classic code

### Update

Update with `sudo apt install code`

### Recommended Extensions

Extensions recommended by VS Code for

Python:             @id:ms-python.python
CMakeLists.txt:     @id:ms-vscode.cmake-tools
CMakeLists.txt:     @id:twxs.cmake (for auto-completion)
XML Formatting:     @id:redhat.vscode-xml

C/C++:              @id:ms-vscode.cpptools-extension-pack
Other languages:    search extensions market for @recommended:languages 

ROS 1 and 2:        @id:ms-iot.vscode-ros (see https://github.com/ms-iot/vscode-ros#readme for an introduction and tutorials)

Optional extensions

CLang Format automatically formatting C++ ROS nodes once they are saved (requires `sudo apt-get install clang-format` on Linux; set editor.formatOnSave’ to true)

vscode-icons for visually distinct file icons; press ‘Ctrl + Shift + P’ and type ‘activate icons’ to activate

TODO others?

###  Setup Remote Development

This VS Code feature allows to edit files on a remote machine (e.g. a Raspberry Pi) from a development PC.

https://code.visualstudio.com/docs/remote/remote-overview

(from https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)

Launch VS Code Quick Open (Ctrl+P), paste the following command, and press enter.

`ext install ms-vscode-remote.vscode-remote-extensionpack`

#### SSH Connection

SSH connections allow development on a different machine.

Prerequisites: 
setup network connection to remote machine
remote machine must have an internet connection for initial setup 
In VS Code:

Icon in lower left corner -> Connect to Host... -> Add Host -> Enter username@IP -> Store in user ssh config
Connect to Host... -> select added host -> new VS window opens -> password prompt appears in search bar -> VS Code installs on remote machine

If connection with SSH in VS Code fails with "XHR failed", it might be because VS Code cannot download some files on the remote machine. Check the internet connection

Open Folder on remote machine -> VS Code might ask for SSH password again (to fix this, see SSH Connection Without Password)

##### Remote Tab

Once set up, remote connections are also shown in the Remote Tab. Select 'Remote' in dropdown box to see a list of configured connections.

#### GUIs over SSH (X11 Forwarding)

Connect to remote machine with `ssh -X user@ip` or set ForwardX11 option in ssh config:

```
  Host 10.0.0.199
  HostName 10.0.0.199
  ForwardX11 yes
  User user
```

When connecting from Windows, install an X Server (e.g. XMing) to run remote X11 apps.

#### SSH Connection Without Password

Use a public/private key pair to connect via SSH, eliminating the need for entering user/password. From https://stackoverflow.com/questions/66113731/how-to-save-ssh-password-to-vscode

On the client, the ssh config file needs to be edited, and two ssh key files created. 
On Linux, these files are located at `/home/<localuser>/.ssh/config` and `/home/<localuser>/.ssh/keys/<myhost>_rsa`, respectively.
On Windows, replace the file names with `/Users/<localuser>/.ssh/config` and `/Users/<localuser>/.ssh/keys/<myhost>_rsa`, respectively.

##### Client Configuration

Edit `/home/<localuser>/.ssh/config`
Add:
```
Host <myhost>
  HostName <myhost>
  User <remoteuser>
  Port 22
  PreferredAuthentications publickey
  IdentityFile "/home/<localuser>/.ssh/keys/<myhost>_rsa"
```

Generate public/private keys, e.g.: 
`mkdir /home/<localuser>/.ssh/keys/`
`ssh-keygen -q -b 2048 -P "" -f /home/<localuser>/.ssh/keys/<myhost>_rsa -t rsa`

##### Add Key to Remote Host

Add content of `<myhost>_rsa.pub` to `home/<remoteuser>/.ssh/authorized_keys`. Create this file with permissions 600 if it does not exist yet.


## Git GUI

Optional graphical user interface for git commits.

Install with`sudo apt install git-gui`

Start with `git gui`

If there is an error message about missing word lists for spell checking when starting `git gui`, install the aspell package for your language, e.g. `sudo apt install aspell-de`


## C/C++ Include Paths

For the recommended way to manage include paths, see C/C++ IntelliSense Code Completion.

Alternatively, you can edit include paths manually, as follows:

For each VS Code workspace, edit `.vscode/c_cpp_properties.json:
add `"/opt/ros/humble/include/**"` to `includePath` (** indicates recursive search below the given path)

This file can also be edited via C/C++ Configurations (IntelliSense Konfigurationen) page in VS Code. 


## Create Launch Items

Untested: https://answers.ros.org/question/256565/how-to-add-ros-to-path-in-vs-code/#366675
How to set up VS Code without ROS extension
Set IntelliSense Engine to "Tag Parser"?


## ROS development with Visual Studio Code

from https://erdalpekel.de/?p=157

Note: when working with VS Code, open your ROS workspace, not specific packages. Otherwise these settings will not work.

### Create Build Tasks

Set custom key bindings for tasks: File -> Preferences -> Keyboard Shortcuts

```
'Run Build Task': 'Ctrl + Shift + B'
'Run Task': 'Ctrl + Shift + 3'
```

Set .vscode/tasks.json (TODO include file for details)

### C/C++ IntelliSense Code Completion

The C/C++ VS Code extension requires information about build dependencies for code completion.

Add `"compileCommands": "${workspaceFolder}/build/compile_commands.json"`to `.vscode/c_cpp_properties.json`

Make sure `CMAKE_EXPORT_COMPILE_COMMANDS=1` is passed to the build process. E.g. for colcon, add 
`"--cmake-args", "-DCMAKE_EXPORT_COMPILE_COMMANDS=1",
"--ament-cmake-args", "-DCMAKE_EXPORT_COMPILE_COMMANDS=1"`
"--catkin-cmake-args", "-DCMAKE_EXPORT_COMPILE_COMMANDS=1"`
to cover all types of package build systems. 

NOTE: the workspace has to be built at least once after checkout to recreate the compile_commands.json file.

## Setting Up VSCode with ROS

Untested, may be outdated: https://samarth-robo.github.io/blog/2020/12/03/vscode_ros.html

## Troubleshooting

### On Ubuntu: VS Code Context Popup Menu Disappears Instantly on Right Click

Solution from https://stackoverflow.com/a/71935006/22824140

1. Install "Easystroke Gesture Recognition" from repository
`sudo apt-get install easystroke`

2. Open Easystroke and go to the preferences tab, click on "Gesture Button", and right click on the rectangular area to select the right mouse button.

3. In the "Timeout Profile" dropdown list pick up "Timeout Off"

