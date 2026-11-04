# Autonomous Maze Drawing and Navigation Using ROS 1

## 1. Project Overview

This project demonstrates a two-turtle maze drawing and autonomous navigation system using **ROS 1 Noetic and TurtleSim**. The simulation uses a 7x7 grid. One turtle draws the maze, while a second turtle navigates from the start cell to the goal cell.

The navigation is performed using the **Breadth-First Search (BFS)** algorithm. BFS explores neighbouring cells level by level and finds a shortest route in an unweighted grid. The calculated route is published through the ROS topic `/path_topic`, and the second turtle follows the route using velocity commands.

This project demonstrates basic path planning, ROS publishers and subscribers, ROS services, and simulated robot movement.

> **Important implementation note:** This is a reproducible implementation based on the project description. The obstacle arrangement is an example layout designed to produce the route documented below. It may not be identical to the maze used in an earlier demonstration or presentation.

## 2. Project Objectives

- Draw a 7x7 grid-based maze in TurtleSim.
- Use two turtles for maze drawing and navigation.
- Implement BFS for grid-based path planning.
- Find a route from start `(0, 0)` to goal `(6, 6)`.
- Publish the calculated route on `/path_topic`.
- Control the navigating turtle through `/turtle2/cmd_vel`.
- Understand ROS topics, services, publishers, and subscribers.

## 3. System Workflow

1. The TurtleSim node opens the simulation window.
2. The program spawns `turtle2` at the start cell.
3. `turtle1` draws the grid and marks blocked cells.
4. BFS searches for a route from `(0, 0)` to `(6, 6)`.
5. The route is published on `/path_topic`.
6. `turtle2` follows the route and reaches the goal.

## 4. Maze Configuration

| Parameter | Value |
|---|---|
| Grid size | 7 x 7 |
| Start cell | `(0, 0)` |
| Goal cell | `(6, 6)` |
| Path-planning algorithm | Breadth-First Search |
| Allowed movement | Up, down, left, right |
| Number of cells in example route | 13 |

### Demonstrated BFS Route

```text
(0,0) -> (0,1) -> (0,2) -> (1,2) -> (2,2)
      -> (2,3) -> (2,4) -> (3,4) -> (4,4)
      -> (4,5) -> (4,6) -> (5,6) -> (6,6)
```

Coordinates are written as `(row, column)`. The example maze leaves the listed corridor open and marks the other cells as blocked.

## 5. How the BFS Algorithm Works

BFS is an uninformed search algorithm that explores all reachable neighbouring cells in layers.

It uses:
- **Queue:** Holds cells that still need to be explored.
- **Visited set:** Prevents repeated exploration of a cell.
- **Parent dictionary:** Records the previous cell so the final route can be reconstructed.

Algorithm:
1. Add the start cell to the queue and mark it visited.
2. Remove the first cell from the queue.
3. Check its four neighbouring cells.
4. Add each valid, unblocked, unvisited neighbour to the queue.
5. Store the current cell as the neighbour's parent.
6. Continue until the goal is reached or no cells remain.
7. Reconstruct the route by following parent links from the goal to the start.

Since every grid movement has equal cost, BFS finds a shortest route in terms of the number of steps.

## 6. ROS Topics and Services

| ROS interface | Purpose |
|---|---|
| `/turtle1/cmd_vel` | Velocity commands for the drawing turtle |
| `/turtle1/pose` | Position and orientation of turtle1 |
| `/turtle2/cmd_vel` | Velocity commands for the navigating turtle |
| `/turtle2/pose` | Position and orientation of turtle2 |
| `/path_topic` | Publishes the BFS route as a string |
| `/spawn` | Spawns the second turtle |
| `/turtle1/set_pen` | Changes turtle1 pen settings |
| `/turtle2/set_pen` | Changes turtle2 pen settings |

## 7. Technologies Used

- Ubuntu 20.04
- ROS 1 Noetic
- Python 3
- rospy
- TurtleSim
- geometry_msgs
- std_msgs
- Breadth-First Search

## 8. Package Structure

```text
autonomous_maze_turtle/
├── CMakeLists.txt
├── package.xml
├── README.md
├── launch/
│   └── maze.launch
└── scripts/
    └── maze_navigation.py
```

## 9. Installation and Execution

### Prerequisites

Install ROS Noetic and TurtleSim on Ubuntu 20.04. Install TurtleSim if it is not already available:

```bash
sudo apt update
sudo apt install ros-noetic-turtlesim
```

Source ROS:

```bash
source /opt/ros/noetic/setup.bash
```

### Create a catkin workspace

```bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws
catkin_make
```

### Add and build the package

Copy the `autonomous_maze_turtle` folder into `~/catkin_ws/src/`, then run:

```bash
cd ~/catkin_ws
catkin_make
source devel/setup.bash
```

Make the Python script executable:

```bash
chmod +x ~/catkin_ws/src/autonomous_maze_turtle/scripts/maze_navigation.py
```

### Launch the project

```bash
roslaunch autonomous_maze_turtle maze.launch
```

The TurtleSim window should open. Turtle1 draws the maze, and turtle2 follows the BFS route.

## 10. Checking the Published Route

Open another terminal and source the workspace:

```bash
source ~/catkin_ws/devel/setup.bash
```

View the calculated route:

```bash
rostopic echo /path_topic
```

List active ROS topics:

```bash
rostopic list
```

## 11. Expected Output

- A 7x7 grid is drawn in the TurtleSim window.
- Blocked cells are marked with red crosses.
- BFS calculates a route from `(0, 0)` to `(6, 6)`.
- The route is published on `/path_topic`.
- Turtle2 follows the route and reaches the goal cell.

## 12. Limitations

- The project is simulation-only and does not use physical robot hardware.
- The maze layout, start cell, and goal cell are predefined in the Python script.
- The example uses a fixed obstacle arrangement.
- TurtleSim navigation does not represent real-world localization or obstacle avoidance.
- The project demonstrates basic grid path planning rather than a complete autonomous mobile-robot navigation stack.

## 13. Future Improvements

- Allow users to enter different maze layouts.
- Add dynamic obstacles and path replanning.
- Visualize the BFS search process.
- Compare BFS with A* and Dijkstra's algorithm.
- Extend the project to Gazebo or a physical mobile robot.

## 14. Conclusion

The Autonomous Maze Drawing and Navigation project demonstrates how a grid-based path-planning algorithm can be integrated with ROS. Two TurtleSim turtles are used to separate maze drawing from autonomous navigation. BFS calculates a shortest route, which is published through a ROS topic and followed by the navigating turtle.

The project provides practical experience with ROS communication, Python-based robot control, simulation, and fundamental path-planning concepts.
