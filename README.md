# Test package to evaluate behavior of `ros_command` with output

## roslaunch

### wrapper swallows launch output

```
$ roslaunch test_minimal_output minimal_output.launch
Hello World
^CTraceback (most recent call last):
  File "/home/v4hn/ros/one/guzheng/venv/bin/roslaunch", line 8, in <module>
    sys.exit(main_launch())
             ^^^^^^^^^^^^^
  File "/home/v4hn/repos/ros_command/src/ros_command/commands/roslaunch.py", line 41, in main_launch
    loop.run_until_complete(main())
  File "/usr/lib/python3.11/asyncio/base_events.py", line 641, in run_until_complete
    self.run_forever()
  File "/usr/lib/python3.11/asyncio/base_events.py", line 608, in run_forever
    self._run_once()
  File "/usr/lib/python3.11/asyncio/base_events.py", line 1898, in _run_once
    event_list = self._selector.select(timeout)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/selectors.py", line 468, in select
    fd_event_list = self._selector.poll(timeout, max_ev)
                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
KeyboardInterrupt

```

### if launch is shutdown due to required process death, launch output is *appended*

```
$ roslaunch test_minimal_output minimal_output_required.launch
Hello World
================================================================================REQUIRED process [minimal_output-2] has died!
process has finished cleanly
log file: /home/v4hn/.ros/log/04aa6424-1db5-11ef-85ed-cc96e59be9af/minimal_output-2*.log
Initiating shutdown!
================================================================================
... logging to /home/v4hn/.ros/log/04aa6424-1db5-11ef-85ed-cc96e59be9af/roslaunch-samsa-451464.log
Checking log directory for disk usage. This may take a while.
Press Ctrl-C to interrupt
Done checking log file disk usage. Usage is <1GB.

started roslaunch server http://samsa:45369/

SUMMARY
========

PARAMETERS
 * /rosdistro: one
 * /rosversion: 1.16.0

NODES
  /
    minimal_output (test_minimal_output/minimal_output.py)

auto-starting new master
process[master]: started with pid [451489]
ROS_MASTER_URI=http://localhost:11311

setting /run_id to 04aa6424-1db5-11ef-85ed-cc96e59be9af
process[rosout-1]: started with pid [451498]
started core service [/rosout]
process[minimal_output-2]: started with pid [451501]
[minimal_output-2] killing on exit
[rosout-1] killing on exit
[master] killing on exit
shutting down processing monitor...
... shutting down processing monitor complete
done

```

### regular expected behavior through roslaunch

```
$ ros-o/install/bin/roslaunch test_minimal_output minimal_output_required.launch
... logging to /home/v4hn/.ros/log/aa435882-1db5-11ef-85ed-cc96e59be9af/roslaunch-samsa-460843.log
Checking log directory for disk usage. This may take a while.
Press Ctrl-C to interrupt
Done checking log file disk usage. Usage is <1GB.

started roslaunch server http://samsa:35847/

SUMMARY
========

PARAMETERS
 * /rosdistro: one
 * /rosversion: 1.16.0

NODES
  /
    minimal_output (test_minimal_output/minimal_output.py)

auto-starting new master
process[master]: started with pid [460868]
ROS_MASTER_URI=http://localhost:11311

setting /run_id to aa435882-1db5-11ef-85ed-cc96e59be9af
process[rosout-1]: started with pid [460894]
started core service [/rosout]
process[minimal_output-2]: started with pid [460897]
Hello World
================================================================================REQUIRED process [minimal_output-2] has died!
process has finished cleanly
log file: /home/v4hn/.ros/log/aa435882-1db5-11ef-85ed-cc96e59be9af/minimal_output-2*.log
Initiating shutdown!
================================================================================
[minimal_output-2] killing on exit
[rosout-1] killing on exit
[master] killing on exit
shutting down processing monitor...
... shutting down processing monitor complete
done
```
