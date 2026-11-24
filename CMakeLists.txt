#   /$$$$$$   /$$$$$$  /$$   /$$ /$$$$$$$$ /$$$$$$  /$$$$$$
#  /$$__  $$ /$$__  $$| $$$ | $$| $$_____/|_  $$_/ /$$__  $$
# | $$  \__/| $$  \ $$| $$$$| $$| $$        | $$  | $$  \__/
# | $$      | $$  | $$| $$ $$ $$| $$$$$     | $$  | $$ /$$$$
# | $$      | $$  | $$| $$  $$$$| $$__/     | $$  | $$|_  $$
# | $$    $$| $$  | $$| $$\  $$$| $$        | $$  | $$  \ $$
# |  $$$$$$/|  $$$$$$/| $$ \  $$| $$       /$$$$$$|  $$$$$$/
#  \______/  \______/ |__/  \__/|__/      |______/ \______/
set(PROJECT_NAME "project")

cmake_minimum_required(VERSION 3.22)
project(${PROJECT_NAME} VERSION 0.0)

set(CMAKE_CXX_STANDARD 26)
set(CMAKE_CXX_STANDARD_REQUIRED True)

#   /$$$$$$   /$$$$$$  /$$   /$$ /$$$$$$$   /$$$$$$  /$$$$$$$$  /$$$$$$
#  /$$__  $$ /$$__  $$| $$  | $$| $$__  $$ /$$__  $$| $$_____/ /$$__  $$
# | $$  \__/| $$  \ $$| $$  | $$| $$  \ $$| $$  \__/| $$      | $$  \__/
# |  $$$$$$ | $$  | $$| $$  | $$| $$$$$$$/| $$      | $$$$$   |  $$$$$$
#  \____  $$| $$  | $$| $$  | $$| $$__  $$| $$      | $$__/    \____  $$
#  /$$  \ $$| $$  | $$| $$  | $$| $$  \ $$| $$    $$| $$       /$$  \ $$
# |  $$$$$$/|  $$$$$$/|  $$$$$$/| $$  | $$|  $$$$$$/| $$$$$$$$|  $$$$$$/
#  \______/  \______/  \______/ |__/  |__/ \______/ |________/ \______/
file(GLOB_RECURSE SOURCES "src/*.cpp" "src/*.hpp")

#  /$$$$$$$$ /$$$$$$$$  /$$$$$$  /$$$$$$$$ /$$$$$$ /$$   /$$  /$$$$$$
# |__  $$__/| $$_____/ /$$__  $$|__  $$__/|_  $$_/| $$$ | $$ /$$__  $$
#    | $$   | $$      | $$  \__/   | $$     | $$  | $$$$| $$| $$  \__/
#    | $$   | $$$$$   |  $$$$$$    | $$     | $$  | $$ $$ $$| $$ /$$$$
#    | $$   | $$__/    \____  $$   | $$     | $$  | $$  $$$$| $$|_  $$
#    | $$   | $$       /$$  \ $$   | $$     | $$  | $$\  $$$| $$  \ $$
#    | $$   | $$$$$$$$|  $$$$$$/   | $$    /$$$$$$| $$ \  $$|  $$$$$$/
#    |__/   |________/ \______/    |__/   |______/|__/  \__/ \______/

option(BUILD_TESTS "Build tests" ${PROJECT_IS_TOP_LEVEL})

if(BUILD_TESTS)
    set(TEST_NAME "run_tests")
    include(FetchContent)
    FetchContent_Declare(
        googletest
        GIT_REPOSITORY https://github.com/google/googletest.git
        GIT_TAG v1.17.0
        FIND_PACKAGE_ARGS NAMES GTest
    )
    set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
    FetchContent_MakeAvailable(googletest)

    enable_testing()

    file(GLOB_RECURSE TEST_SOURCES "test/*.cpp" "test/*.hpp")

    add_executable(${TEST_NAME} ${TEST_SOURCES})

    target_link_libraries(${TEST_NAME} PRIVATE
        jde::jde
        GTest::gtest_main
    )


    target_include_directories(${TEST_NAME} PRIVATE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    )
    target_include_directories(${TEST_NAME} PRIVATE "src")

    include(GoogleTest)
    gtest_discover_tests(${TEST_NAME})
    add_custom_target(build_tests DEPENDS ${TEST_NAME})
endif()



#  /$$       /$$$$$$ /$$$$$$$   /$$$$$$
# | $$      |_  $$_/| $$__  $$ /$$__  $$
# | $$        | $$  | $$  \ $$| $$  \__/
# | $$        | $$  | $$$$$$$ |  $$$$$$
# | $$        | $$  | $$__  $$ \____  $$
# | $$        | $$  | $$  \ $$ /$$  \ $$
# | $$$$$$$$ /$$$$$$| $$$$$$$/|  $$$$$$/
# |________/|______/|_______/  \______/

include(FetchContent)
FetchContent_Declare(
  jde
  GIT_REPOSITORY https://github.com/jevndev/jde.git
  GIT_TAG        v1.0.2
  FIND_PACKAGE_ARGS NAMES jde
)
FetchContent_MakeAvailable(jde)



#  /$$$$$$$$ /$$$$$$  /$$$$$$$   /$$$$$$  /$$$$$$$$ /$$$$$$$$
# |__  $$__//$$__  $$| $$__  $$ /$$__  $$| $$_____/|__  $$__/
#    | $$  | $$  \ $$| $$  \ $$| $$  \__/| $$         | $$
#    | $$  | $$$$$$$$| $$$$$$$/| $$ /$$$$| $$$$$      | $$
#    | $$  | $$__  $$| $$__  $$| $$|_  $$| $$__/      | $$
#    | $$  | $$  | $$| $$  \ $$| $$  \ $$| $$         | $$
#    | $$  | $$  | $$| $$  | $$|  $$$$$$/| $$$$$$$$   | $$
#    |__/  |__/  |__/|__/  |__/ \______/ |________/   |__/
add_executable(${PROJECT_NAME} ${SOURCES})
target_include_directories(${PROJECT_NAME} PRIVATE "src")
target_link_libraries(${PROJECT_NAME} PRIVATE jde::jde)
target_compile_options(${PROJECT_NAME} PRIVATE -Wall -Wextra -Werror -freflection)

add_custom_target(build_project DEPENDS ${PROJECT_NAME})
