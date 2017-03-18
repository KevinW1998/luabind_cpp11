cmake_minimum_required (VERSION 3.1)
project (luabind)

option(BUILD_SHARED "Build as shared" ON)
option(BUILD_TEST "Build tests" OFF)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if(NOT Boost_FOUND)
	find_package(Boost REQUIRED)
endif()

if(NOT LUA_FOUND AND NOT LUA51_FOUND)
	find_package(Lua51 REQUIRED)
	set(LUA_INCLUDE_DIRS "${LUA_INCLUDE_DIR}")
endif()

if(MSVC)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /MP")
endif()

include_directories("${PROJECT_SOURCE_DIR}/" ${Boost_INCLUDE_DIRS} ${LUA_INCLUDE_DIRS})


file(GLOB SRC "src/*.cpp" "luabind/*.hpp")
file(GLOB TEST_SRC "test/*.cpp" "test/*.hpp")

if(BUILD_SHARED)
	add_definitions(-DLUABIND_DYNAMIC_LINK)
	add_library(luabind SHARED ${SRC})
else()
	add_library(luabind STATIC ${SRC})
endif()

target_link_libraries(luabind ${LUA_LIBRARIES}/lua51.lib)

if(BUILD_TEST)
	add_executable(Test ${TEST_SRC})
	target_link_libraries(Test luabind)
endif()

install(TARGETS luabind RUNTIME DESTINATION bin LIBRARY DESTINATION lib ARCHIVE DESTINATION lib)
install(DIRECTORY luabind/ DESTINATION include/luabind FILES_MATCHING PATTERN "*.hpp")
