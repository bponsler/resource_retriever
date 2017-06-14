cmake_minimum_required(VERSION 2.8.3)
project(resource_retriever)

# Add support for C++11
if(NOT WIN32)
  add_definitions(-std=c++11)
endif()

find_package(ament_cmake REQUIRED)
find_package(ament_cmake_python REQUIRED)
find_package(ros2_console REQUIRED)

ament_python_install_package(${PROJECT_NAME})

set(INCLUDE_DIRS include ${ament_cmake_INCLUDE_DIRS}
  ${ros2_console_INCLUDE_DIRS})
include_directories(${INCLUDE_DIRS})

set(LIBRARY_DIRS ${ament_cmake_LIBRARY_DIRS} ${ros2_console_LIBRARY_DIRS})

link_directories(${LIBRARY_DIRS})

set(LIBS ${ament_cmake_LIBRARIES} ${ros2_console_LIBRARIES})

find_package(Boost REQUIRED)

include(FindCURL)
if(NOT CURL_FOUND)
  message("CURL not found!  Aborting...")
  fail()
endif(NOT CURL_FOUND)

add_library(${PROJECT_NAME} src/retriever.cpp)
target_link_libraries(${PROJECT_NAME} ${CURL_LIBRARIES} ${LIBS})

if(BUILD_TESTING)
  add_subdirectory(test EXCLUDE_FROM_ALL)
endif()

install(TARGETS ${PROJECT_NAME} DESTINATION lib)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION include/${PROJECT_NAME})

ament_export_dependencies(ament_cmake)
ament_export_dependencies(ros2_console)
ament_export_include_directories(${INCLUDE_DIRS})
ament_export_libraries(${PROJECT_NAME} ${LIBS})

ament_package()
