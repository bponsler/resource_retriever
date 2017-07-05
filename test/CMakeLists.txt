set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR})

find_package(ament_lint_auto REQUIRED)
ament_lint_auto_find_test_dependencies()

ament_add_gtest(${PROJECT_NAME}_utest test.cpp)
target_link_libraries(${PROJECT_NAME}_utest ${PROJECT_NAME})

ament_add_nose_test(${PROJECT_NAME}_nose_tests test.py)
