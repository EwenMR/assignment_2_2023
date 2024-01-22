# Research Track 1 second assignment using ROS (2024)
This repository contains all the necessary code to download and run the ros simulation.

# Assignment
- (a) A node that implements an action client, allowing the user to set a target (x, y) or to cancel it. Try to use the
feedback/status of the action server to know when the target has been reached. The node also publishesthe
robot position and velocity as a custom message (x,y, vel_x, vel_z), by relying on the values published on the
topic /odom;
- (b) A service node that, when called, returnsthe coordinates of the last target sent by the user;
- (c) Anotherservice node thatsubscribes to the robot’s position and velocity (using the custom message) and
implements a server to retrieve the distance of the robot from the target and the robot’s average speed.
- Create a launch file to start the whole simulation. Use a parameterto select the size of the averaging window of node (c)

# Installing and Running the code
## Installing
You will need to clone this repository,
```bash
git clone https://github.com/EwenMR/assignment_2_2023.git
```
## Running
We want to go into the ros workspace first:
```bash
cd ros_ws
```

You must build the workspace using:
```bash
catkin_make
```

You then have to source the ros workspace:
```bash
source devel/setup.bash
```

To start the ROS master or core:
```bash
roscore
```

To launch Rviz and Gazebo:
```bash
roslaunch assignment_2_2023 assignment1.launch
```

To launch the action client and the service server nodes, with the choice of the size of the window for node (c)(replace 'X' with the desired window size):
```bash
roslaunch assignment_2_2023 assignment2.launch window_size:=X
```

If it is so that you are experiencing any issues regarding the execution of the new nodes it may be due to the permissions of the files, in which case you have to make them executable:
```bash
cd src/assignment_2_2023/scripts
chmod +x *
```

# Functionalities
## How to set or cancel a new target
Upon launching the assignment2 launch file the user should be prompted to enter two coordinates:
x,y
or to enter 's' to stop the robot and cancel the goal currently set.

## Retrieving the last target set by the user
This can be done by calling the /get_last_target service:
```bash
rosservice call /get_last_target
```

## Retrieving the distance of the target from the robot's position (dist) with the average speed of the robot (mu):
This can be done calling another service /get_dist_mu:
```bash
rosservice call /get_dist_mu
```

# Structure of the action client code
These codes are built by Python scripts.
The pseudocode of the action client node is as follows:
```bash
# Initialize ROS node and create necessary components
initialize ROS node 'action_client_node'
create SimpleActionClient for '/reaching_goal'
create publishers for '/robot_info' and '/target_info'
subscribe to '/odom' topic with odom_callback function

# Define callback function for receiving odometry information
def odom_callback(odom_msg):
    extract relevant information from odometry message
    create Custom message with extracted information
    publish Custom message to '/robot_info'

# Define method for setting the goal interactively
def set_goal_interactively():
    prompt user for target coordinates
    create PlanningGoal and Goal messages with target coordinates
    publish Goal message to '/target_info'
    send goal to action server

    allow user to cancel the goal
    wait for the result or cancellation
    print the final result

# Define callback function for receiving feedback from the action server
def feedback_callback(feedback):
    handle feedback if needed
    print received feedback

# Start the main execution
if __name__ is "__main__":
    try:
        create instance of ActionClientNode
        set a goal interactively
        keep the node alive

    except ROSInterruptException:
        handle ROSInterruptException
```

# Further improvements
Author doesn't have prerequisite knowledge of ROS, so these codes may not be so elegant. The naming of nodes and variables could have been made a little easier to understand.