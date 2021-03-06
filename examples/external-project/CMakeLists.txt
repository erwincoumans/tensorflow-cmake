cmake_minimum_required(VERSION 2.8)
project(external-project)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)
set(SOURCE_FILES main.cc)
set(EXECUTABLE external-project)

# Add modules
list(APPEND CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake/Modules")

find_package(TensorFlow REQUIRED)

# Location where external projects will be downloaded
set(DOWNLOAD_LOCATION "${PROJECT_SOURCE_DIR}/external/src" CACHE PATH "Location where external projects will be downloaded")
mark_as_advanced(DOWNLOAD_LOCATION)

include(Protobuf)
include(Eigen)

set(PROJECT_INCLUDE_DIRS ${TensorFlow_INCLUDE_DIRS})
set(PROJECT_LIBRARIES ${TensorFlow_LIBRARIES})
set(PROJECT_DEPENDENCIES Eigen Protobuf)

include_directories(${PROJECT_INCLUDE_DIRS})
add_executable(${EXECUTABLE} ${SOURCE_FILES})
target_link_libraries(${EXECUTABLE} ${PROJECT_LIBRARIES})
add_dependencies(${EXECUTABLE} ${PROJECT_DEPENDENCIES})
