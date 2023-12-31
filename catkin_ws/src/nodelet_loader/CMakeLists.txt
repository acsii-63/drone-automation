cmake_minimum_required(VERSION 2.8.3)
project(nodelet_loader)

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
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR}/cmake)



find_package(catkin REQUIRED 
   COMPONENTS 
   roscpp 
   nodelet)

include_directories(${catkin_INCLUDE_DIRS})

catkin_package(
   CATKIN_DEPENDS 
   roscpp 
   nodelet 
   LIBRARIES ${PROJECT_NAME}
)
add_executable(${PROJECT_NAME} src/nodelet_loader.cpp)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})


