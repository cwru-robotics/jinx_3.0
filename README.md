# Jinx 3.0
jinx 3.0 repositories in ubuntu 20.04 and ros Noetic

# installation 
Refer to https://github.com/cwru-robotics/jinx_3.0/blob/master/DOC/Installation_instructions.md

# Starting Jinx up

## Hardwere start

- Push the lever up for the main system switch on the back.
- Turn the board on by holding the digital button for 5 seconds, until the digital voltage indicator is powered on. 
- turn gigabyte computer on
- turn Baxter on from its left lower side.

## Sshing into Jinx and Merry

- 'ping jinx' and 'ping baxter01'. When they start pinging back, this means they are ready for ssh.

Currently there is an issue with the serial liberary. we have to make the serial package every time we log in. To do that:

Open three terminals:
- In first terminal run the following commands: 'ssh admin@jinx', then enter password, then 'serial_make', 'baxter_master', then 'kinect_start'
- In the second terminal run: 'ssh admin@jinx', then enter password, then 'serial_make', 'baxter_master', then 'jinx_start'
- In the third terminal, run: 'ssh ruser@baxter01', then enter password, then calibrate the gripper by: 'rosrun baxter_examples gripper keyboard.py' then press capital 'C' to calibrate right gripper, then 'esc' to end. Then you can enable the robot by running 'rosrun baxter_tools enable_robot.py -e'

The robot should be ready to go after that. 

To switch the system off, press the small red oval button on the back, which is the main system breaker.

