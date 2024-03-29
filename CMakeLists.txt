cmake_minimum_required(VERSION 2.8.3)
project(test_andor)
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
        message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  roscpp
  rospy
  #baxter_core_msgs
  message_generation
  std_msgs
  andor

)
catkin_python_setup()

include_directories(
	INCLUDE_DIRS
	include/
)

#Is for add the .o files to devel/lib

catkin_package(  CATKIN_DEPENDS message_generation roscpp rospy std_msgs geometry_msgs  andor_msgs)#baxter_core_msgs 

## Specify additional locations of header files
## Your package locations should be listed before other locations
# include_directories(include)
include_directories(
  ${catkin_INCLUDE_DIRS}
)

#ADD_LIBRARY(endor /home/nasa/Documents/Endor/endor-master/aograph.cpp)
#add_library(andor_aonode include/andor_aonode.cpp)
#add_library(andor_aograph include/andor_aograph.cpp)
#add_library(CallBackClass src/CallBackClass.cpp)
#add_library(endorAction src/endorAction.cpp)


## Declare a C++ executable
# name  name.cpp
add_executable(test_andor src/test_andor.cpp )
add_executable(random_test_andor src/random_test_andor.cpp )


#add_dependencies(andor_aograph andor_msgs_generate_messages_cpp)

add_dependencies(test_andor geometry_msgs_generate_messages_cpp andor_msgs_generate_messages_cpp) #baxter_core_msgs_generate_messages_cpp  
add_dependencies(random_test_andor geometry_msgs_generate_messages_cpp andor_msgs_generate_messages_cpp) #baxter_core_msgs_generate_messages_cpp
## Specify libraries to link a library or executable target against
target_link_libraries(test_andor
   ${catkin_LIBRARIES}
#andor_aonode
#andor_aograph
 )

target_link_libraries(random_test_andor
   ${catkin_LIBRARIES}
#andor_aonode
#andor_aograph
 )

#target_link_libraries(andor_aograph andor_aonode)
catkin_install_python(PROGRAMS scripts/randomTest_runner.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
