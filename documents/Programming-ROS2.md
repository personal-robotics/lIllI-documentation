# ROS Programming Tips & Tricks

## Enable Debug Logs

see: https://docs.ros.org/en/humble/Tutorials/Demos/Logging-and-logger-configuration.html#logger-level-configuration-externally


## VS Code & ROS2

### Debug ROS2 Launch Files 

As of Nov. 2023, debugging of launch files is supported, see https://github.com/ms-iot/vscode-ros/pull/900

This requires the ROS extension. Debugging of launched nodes is planned.

Add launch file to VS Code `launch.json` configuration, e.g.

```
{
    "name": "ROS: Launch",
    "type": "ros",
    "request": "debug_launch",
    "target": "${workspaceFolder}/src/lilli_bot/launch/rsp.launch.py"
}
```        

Start "Run and Debug" to debug the launch file.

With `"request": "launch"`, the launch file is run without debugging.

Note that the target is the launch file in the src directory, not the installed launch file. This simplifies the edit / debug cycle for non-symlink installs.

To debug launch files, one can also select "Run and Debug", then select in the combo box "ROS... / Select Package / Select Launch File".
However, for some reason VS Code may not list all launch files here.  

### Troubleshoot attaching to processes using GDB

For reference, see https://github.com/Microsoft/MIEngine/wiki/Troubleshoot-attaching-to-processes-using-GDB

### Warning: "gdb failed to set controlling terminal operation not permitted"

When using "Run and Debug" A warning may appear in the terminal

"warning gdb failed to set controlling terminal operation not permitted" 

and the debug console will show an error "Stopped due to shared library event (no libraries added or removed)"

This warning itself does not stop the debugger from working (see https://developercommunity.visualstudio.com/t/warning-gdb-failed-to-set-controlling-terminal-ope/206839).

Multiple solutions are suggested for this issue https://github.com/microsoft/vscode-cpptools/issues/121 but have not been confirmed.