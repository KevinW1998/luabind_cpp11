PROJECT(TestLuabind11)
cmake_minimum_required(VERSION 2.8)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
CONAN_BASIC_SETUP()

SET(VERBOSE 1)
SET(CMAKE_VERBOSE_MAKEFILE 1)
SET(CMAKE_SKIP_RPATH 0)

ADD_EXECUTABLE(example example.cpp)
target_link_libraries(example ${CONAN_LIBS})

# Copy .lua file into executable dir
add_custom_command(TARGET example POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
	"${CMAKE_SOURCE_DIR}/greet.lua"
	$<TARGET_FILE_DIR:example>)
