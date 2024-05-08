cmake_minimum_required(VERSION 3.16)
project(exec)

set(CMAKE_CXX_STANDARD 17)  # or whatever standard you need

# Append the base path for Homebrew installations on Apple Silicon
list(APPEND CMAKE_PREFIX_PATH "/opt/homebrew")

# Find the packages, assuming config files are available and correct
find_package(mongocxx REQUIRED)
find_package(bsoncxx REQUIRED)

find_package(Boost REQUIRED COMPONENTS thread json)

if(Boost_FOUND)
    # include 패스 설정
    include_directories(${Boost_INCLUDE_DIRS})
    link_libraries(${Boost_LIBRARIES})
endif()

# If the find_package commands above don't handle the include directories,
# you manually add them
include_directories("/opt/homebrew/include/mongocxx/v_noabi")
include_directories("/opt/homebrew/include/bsoncxx/v_noabi")
include_directories("/opt/homebrew/Cellar/mongo-cxx-driver/3.10.1/include/bsoncxx/v_noabi/bsoncxx/third_party/mnmlstc")

# Assuming you're building an executable
add_executable(${PROJECT_NAME} main.cpp)

# Linking libraries, adjust library names as found in /opt/homebrew/lib/
target_link_libraries(${PROJECT_NAME} PRIVATE mongocxx bsoncxx)
