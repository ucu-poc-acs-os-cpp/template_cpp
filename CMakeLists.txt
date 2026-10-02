cmake_minimum_required(VERSION 3.15)

#! Check every comment after the "#!"

# If you've already read all of the comments and want to declutter this file,
# you can remove all of them by replacing this regex pattern via Ctrl+H
# (don't forget to enable regular expressions): #.*$\r?\n?

#! CHANGE YOUR PROJECT NAME
set(PROJECT_NAME template)
project(${PROJECT_NAME} C CXX) # project(${PROJECT_NAME} C CXX ASM)

set(CMAKE_CXX_STANDARD 23)

##########################################################
# User configurable options of the template
##########################################################
# Note: symbols like WARNINGS_AS_ERRORS in configuration are intentionally variables
# and not CMake options --using options creates too many problems for students.

#! It is a good practice to set "WARNINGS_AS_ERRORS" ON,
#  but sometimes it creates too much trouble, so default is OFF.
set(WARNINGS_AS_ERRORS OFF)

#! Always use PVS Studio while developing (if you have a student license).
set(ENABLE_PVS_STUDIO OFF)

#! Select appropriate sanitizers.
#  Definitely enable sanitizers while developing.
#  Disable it for the production builds and before submitting for grading.
#  Only one of the Memory (MSAN), Address (ASAN), or Thread (TSan)
#  sanitizers is applicable at the time -- the first defined.

#! UndefinedBehaviorSanitizer (UBSan).
#  Info: https://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html
set(ENABLE_UBSan OFF)
#! AddressSanitizer -- detects use after free or after scope exit,
#  memory overflows and leaks.
#  Info: https://github.com/google/sanitizers/wiki/AddressSanitizer
set(ENABLE_ASAN OFF)
#! ThreadSanitizer -- detects data races.
#  Info: https://clang.llvm.org/docs/ThreadSanitizer.html
set(ENABLE_TSan OFF)
#! MemorySanitizer -- detects uninitialized memory reads
#  Info: https://github.com/google/sanitizers/wiki/MemorySanitizer
set(ENABLE_MSAN OFF)

#! By default -- build release version if not specified otherwise.
if (NOT CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE Release)
endif ()

# Warnings as errors should be imported here -- do not move this line
include(cmake/CompilerWarnings.cmake)

##########################################################
# Project files, packages, libraries and so on
##########################################################

#! Define ALL_TARGETS variable to use in PVS and Sanitizers
#  Change your target names (it doesn't have to be PROJECT_NAME) 
#  and add new ones as needed.
set(ALL_TARGETS ${PROJECT_NAME})

#! Project main executable source compilation
#! Change your executable name to match a target, and add sources!
#  You can add other executables as needed.
add_executable(${PROJECT_NAME} src/main.cpp)

#! Add external packages
# Uncomment if you need Boost or other packages
# find_package(Boost REQUIRED CONFIG COMPONENTS system)

#! Then link them to your target
# target_link_libraries(${PROJECT_NAME} PRIVATE Boost::system)

# If there are any commands that should be executed for all targets, 
# you can put them in the loop below.
foreach(target ${ALL_TARGETS})
	#! Put path to your project headers
	target_include_directories(${target} PRIVATE 
		${CMAKE_CURRENT_SOURCE_DIR}/include)

	# Installs your executables in the bin folder
	install(TARGETS ${target} RUNTIME DESTINATION bin)
endforeach()

##########################################################
# Fixed CMakeLists.txt part
##########################################################

# Include CMake setup
include(cmake/main-config.cmake)
