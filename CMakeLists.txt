cmake_minimum_required(VERSION 3.24)
project("skl-game-template")

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)

FILE(MAKE_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/bin)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)

add_compile_definitions(NOMINMAX)

if(NOT DEFINED SKL_ENGINE_PATH)
        option(SKL_ENGINE_PATH "The path to the skyline-engine project directory")
endif()

if (${SKL_ENGINE_PATH} EQUAL OFF)
	message(FATAL_ERROR "SKL_ENGINE_PATH must be set to the path to the skyline-engine project directory")
endif()

set(SKL_EXTERNAL_GAME ON)
add_subdirectory(${SKL_ENGINE_PATH} ${CMAKE_CURRENT_BINARY_DIR}/engine)

#==============================================================================
# GAME MODULE
#==============================================================================
file(GLOB_RECURSE GAME_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)

add_library(game-module MODULE ${GAME_SRC})
# Version the PDB file to avoid locking issues
string(TIMESTAMP BUILD_TIME "%Y%m%d_%H%M%S")
set_target_properties(game-module PROPERTIES
        PDB_NAME "game_module_${BUILD_TIME}")
target_compile_definitions(game-module PRIVATE
        _CRT_SECURE_NO_WARNINGS
        debugRecordArray=debugRecordsGame
        SKL_GAME_MODULE)
target_link_libraries(game-module PRIVATE
        SHARED_DEPENDENCIES
        Jolt
        "$<LINK_LIBRARY:WHOLE_ARCHIVE,engine>")
target_include_directories(game-module PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/include)
