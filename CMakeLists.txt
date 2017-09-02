#######################################################################################
## Nuklear CMake Project File
##

# Version requirement and project info
cmake_minimum_required(VERSION 3.1)
project(nuklear)

# Define pegtl as header only interface
add_library(nuklear INTERFACE)

# Add header files als interface dependencies
target_include_directories(nuklear INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/nuklear)

set(NUKLEAR_LIBRARIES nuklear CACHE INTERNAL "")
