cmake_minimum_required(VERSION 3.0.2)
project(autonomous_maze_turtle)

find_package(catkin REQUIRED COMPONENTS rospy geometry_msgs turtlesim std_msgs)
catkin_package()

catkin_install_python(PROGRAMS
  scripts/maze_navigation.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY launch DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
