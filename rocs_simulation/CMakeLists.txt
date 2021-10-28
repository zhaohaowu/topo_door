cmake_minimum_required(VERSION 3.0.2)
project(rocs_simulation)

set(CMAKE_BUILD_TYPE "Debug")

# Check C++11 or C++0x support
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
   add_definitions(-DCOMPILEDWITHC11)
   message(STATUS "Using flag -std=c++11.")
elseif(COMPILER_SUPPORTS_CXX0X)
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
   add_definitions(-DCOMPILEDWITHC0X)
   message(STATUS "Using flag -std=c++0x.")
else()
   message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

find_package(catkin REQUIRED COMPONENTS
  roscpp
  message_generation
  tf
  nav_msgs
  sensor_msgs
  
cv_bridge
std_msgs
image_transport

)
find_package( OpenCV REQUIRED )
find_package( aruco REQUIRED )
find_package( realsense2 REQUIRED )


generate_messages(
  DEPENDENCIES
)

include_directories(
# include
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS} 
  ${aruco_INCLUDE_DIRS}
  ${realsense2_INCLUDE_DIRS}
)



add_executable(publisher src/publisher.cpp)
target_link_libraries(publisher ${catkin_LIBRARIES} )
add_dependencies(publisher ${PROJECT_NAME}_gencpp)



