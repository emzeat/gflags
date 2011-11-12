cmake_minimum_required(VERSION 2.8)
INCLUDE (CheckIncludeFiles)
INCLUDE (CheckFunctionExists)
INCLUDE (CheckSymbolExists)
project(gflags)

CHECK_INCLUDE_FILES (dlfcn.h HAVE_DLFCN_H)
CHECK_INCLUDE_FILES (fnmatch.h HAVE_FNMATCH_H)
CHECK_INCLUDE_FILES (inttypes.h HAVE_INTTYPES_H)
CHECK_INCLUDE_FILES (memory.h HAVE_MEMORY_H)
CHECK_INCLUDE_FILES (pthread.h HAVE_PTHREAD)
CHECK_INCLUDE_FILES (stdint.h HAVE_STDINT_H)
CHECK_INCLUDE_FILES (stdlib.h HAVE_STDLIB_H)
CHECK_INCLUDE_FILES (strings.h HAVE_STRINGS_H)
CHECK_INCLUDE_FILES (string.h HAVE_STRING_H)
CHECK_INCLUDE_FILES (sys/stat.h HAVE_SYS_STAT_H)
CHECK_INCLUDE_FILES (sys/types.h HAVE_SYS_TYPES_H)
CHECK_INCLUDE_FILES (unistd.h HAVE_UNISTD_H)

CHECK_FUNCTION_EXISTS(strtoll HAVE_STRTOLL)
CHECK_FUNCTION_EXISTS(strtoq HAVE_STRTOQ)

CONFIGURE_FILE(src/config.h.in ${CMAKE_CURRENT_BINARY_DIR}/src/config.h)

if(WIN32)
	set(SRCS
		src/gflags.cc
		src/gflags_completions.cc
		src/gflags_reporting.cc
		src/windows/port.cc
	)
	
	set(HDRS
		src/mutex.h
		src/windows/config.h
		src/windows/port.h
		src/windows/gflags/gflags.h
		src/windows/gflags/gflags_completions.h
	)

	include_directories(
		${PROJECT_SOURCE_DIR}/src/windows/gflags
		${PROJECT_SOURCE_DIR}/src/windows/
	)
else()
	set(SRCS
		src/gflags.cc
		src/gflags_completions.cc
		src/gflags_reporting.cc
	)
	
	set(HDRS
		src/mutex.h
	)

	include_directories(
		${PROJECT_SOURCE_DIR}/src/
		${CMAKE_CURRENT_BINARY_DIR}/src/		
	)
endif()

mz_use_default_compiler_settings()
mz_add_definition(NO_THREADS)
add_library(gflags STATIC ${SRCS} ${HDRS})
mz_target_props(gflags)


## gflags_unittest stuff

option(GLFAGS_UNITTEST "Enable to build unittests for gflags" OFF)

if(GLFAGS_UNITTEST)
	set(gflags_unittest_src
    	src/windows/config.h
		src/config_for_unittests.h
		src/windows/gflags/gflags.h
		src/windows/gflags/gflags_completions.h
		src/gflags_unittest.cc
	)

	add_executable(gflags_unittest ${gflags_unittest_src})
	target_link_libraries(gflags_unittest gflags)
endif()