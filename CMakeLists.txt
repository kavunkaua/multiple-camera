cmake_policy(SET CMP0048 NEW)

project(detector VERSION 0.4.4
                LANGUAGES C CXX)

cmake_minimum_required(VERSION 3.11)

#set(CMAKE_CXX_STANDARD 17)

find_path(EGRABBER_INCLUDE_DIR EGrabber.h
	      PATH_SUFFIXES include
	      PATHS /usr/local/opt/euresys/coaxlink
	            /opt/euresys/coaxlink
	            /opt/euresys/egrabber
	            )

mark_as_advanced(EGRABBER_INCLUDE_DIR)

include(FindPackageHandleStandardArgs)
FIND_PACKAGE_HANDLE_STANDARD_ARGS(EGrabber DEFAULT_MSG EGRABBER_INCLUDE_DIR)

if(EGrabber_FOUND)
	set(EGRABBER_INCLUDE_DIRS ${EGRABBER_INCLUDE_DIR})

	include(CMakeFindDependencyMacro)
	find_dependency(Threads)
	set(EGRABBER_LIBRARIES Threads::Threads)

endif(EGrabber_FOUND)

include_directories(${EGRABBER_INCLUDE_DIRS})

#SET(MYCOMPILE_FLAGS "-Wl,--as-needed -ldl -pthread")
#SET(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} ${MYCOMPILE_FLAGS}")



add_executable(multiplecamera main.cpp)
target_link_libraries(multiplecamera ${EGRABBER_LIBRARIES} "-ldl")