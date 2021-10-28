cmake_minimum_required(VERSION 3.0.2)
project(rocs_topological_mapping)

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
  geometry_msgs
  
cv_bridge
std_msgs
image_transport

)
find_package( OpenCV REQUIRED )
find_package( aruco REQUIRED )
find_package( realsense2 REQUIRED )

add_message_files(
  FILES
  PlaceHolders.msg
)

generate_messages(
  DEPENDENCIES
  geometry_msgs
  std_msgs
)

include_directories(
# include
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS} 
  ${aruco_INCLUDE_DIRS}
  ${realsense2_INCLUDE_DIRS}
)



add_executable(rocs_topological_map_builder src/topological_graph_builder.cpp src/place_holder.cpp
src/show_topo_map.cpp src/door_detection/InDoorOpeningDetector.cpp)
target_link_libraries(rocs_topological_map_builder ${catkin_LIBRARIES} )
add_dependencies(rocs_topological_map_builder ${PROJECT_NAME}_gencpp)



