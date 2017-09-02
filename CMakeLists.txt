#######################################################################################
## Nuklear CMake Project File
##
##
## TODO short tutorial
##

# Macro to list all subdirectories
macro(subdirlist result curdir)
  file(GLOB children RELATIVE ${curdir} ${curdir}/*)
  set(dirlist "")
  foreach(child ${children})
    if(IS_DIRECTORY ${curdir}/${child})
      list(APPEND dirlist ${child})
    endif()
  endforeach()
  set(${result} ${dirlist})
endmacro()

# Version requirement and project info
cmake_minimum_required(VERSION 3.1)
project(nuklear)

# Define pegtl as header only interface
add_library(nuklear INTERFACE)

# Add header files als interface dependencies
target_include_directories(nuklear INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/nuklear)

set(NUKLEAR_LIBRARIES nuklear CACHE INTERNAL "")

# Generate targets for the various bindings
subdirlist(BINDING_DIRS ${CMAKE_CURRENT_LIST_DIR}/nuklear/demo)

# Walk through all available bindings
foreach(BINDING ${BINDING_DIRS})
	# Create unique target name for the binding
	set(BINDING_TARGET_NAME "nuklear_${BINDING}")
	# Add binding as interface library
	add_library(${BINDING_TARGET_NAME} INTERFACE)
	# Build include path
	set(BINDING_INCLUDE_PATH "${CMAKE_CURRENT_LIST_DIR}/nuklear/demo/${BINDING}")
	# Set include path as public dependency
	target_include_directories(${BINDING_TARGET_NAME} INTERFACE ${BINDING_INCLUDE_PATH})
	# Create global variable containing libraries for this binding
	set(BINDING_GLOBAL_REF "NUKLEAR_${BINDING}_LIBRARIES")
	string(TOUPPER ${BINDING_GLOBAL_REF} BINDING_GLOBAL_REF)
	set(${BINDING_GLOBAL_REF} ${BINDING_TARGET_NAME} CACHE INTERNAL "")
endforeach()
