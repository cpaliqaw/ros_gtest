cmake_minimum_required(VERSION 3.0.2)
project(ros_gtest)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  rospy_tutorials
)

## supported in ROS Melodic and newer
set(CMAKE_CXX_STANDARD 14)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS roscpp std_msgs rospy_tutorials
  DEPENDS system_lib
)

include_directories(
  ${PROJECT_NAME}
  ${catkin_INCLUDE_DIRS}
)

## Declare C++ library
add_library(${PROJECT_NAME}
	src/talker.cpp
)

target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})

# add_dependencies(talker_node ${PROJECT_NAME})

add_executable(talker_node nodes/talker_node.cpp)
target_link_libraries(talker_node ${catkin_LIBRARIES} ${PROJECT_NAME})

install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

## Mark cpp header files for installation
install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
)

if(CATKIN_ENABLE_TESTING)
  find_package(rostest REQUIRED)
  add_rostest_gtest(testing-test launch/gtest.test tests/tests_1.cpp)
  target_link_libraries(testing-test ${catkin_LIBRARIES} ${PROJECT_NAME})
endif()