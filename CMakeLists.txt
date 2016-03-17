#Set minimum version requered
cmake_minimum_required(VERSION 2.8.3)
#set project name
project(tld)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  geometry_msgs
  cmake_modules
)

include_directories(include ${catkin_INCLUDE_DIRS})

#Append path to the module path
#list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})
#OpenCV
find_package(OpenCV REQUIRED)
#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/./bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/./lib)
#set the include directories
include_directories (${PROJECT_SOURCE_DIR}/include	${OpenCV_INCLUDE_DIRS})
#libraries
add_library(tld_utils src/tld_utils.cpp)
add_library(LKTracker src/LKTracker.cpp)
add_library(ferNN src/FerNNClassifier.cpp)
add_library(tld src/TLD.cpp)
#executables
add_executable(run_tld src/run_tld.cpp)
add_dependencies(run_tld run_tld_generate_messages_cpp)
#link the libraries
target_link_libraries(run_tld tld LKTracker ferNN tld_utils ${OpenCV_LIBS} ${catkin_LIBRARIES})
#set optimization level 
set(CMAKE_BUILD_TYPE Release)

