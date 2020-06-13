cmake_minimum_required(VERSION 3.9)
project(ps2test VERSION 0.1
	    DESCRIPTION "Playstation 2 Test Project"
        LANGUAGES C)

set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})
set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})

if(NOT CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the build type" FORCE)
	set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
		         "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()


find_package(Git QUIET)
if(GIT_FOUND AND EXISTS "${PROJECT_SOURCE_DIR}/.git")
	    message(STATUS "update submodules")
		execute_process(COMMAND ${GIT_EXECUTABLE} submodule update --init --recursive
			            WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
						RESULT_VARIABLE GIT_SUBMOD_RESULT)
		if(NOT GIT_SUBMOD_RESULT EQUAL "0")
			message(FATAL_ERROR "could not fetch git submodules")
		endif()
endif()

find_package(Doxygen OPTIONAL_COMPONENTS dot mscgen dia)

if(DOXYGEN_FOUND)
	set(DOXYGEN_OUTPUT_DIRECTORY doc)
	set(DOXYGEN_USE_MDFILE_AS_MAINPAGE "README.md")
	doxygen_add_docs(doxygen
		"${PROJECT_SOURCE_DIR}/src/"
		"${PROJECT_SOURCE_DIR}/README.md"
		COMMENT "Generate documentation")
endif()

add_subdirectory(src)
