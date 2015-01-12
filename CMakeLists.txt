# * Copyright (C) 2007 - 2014 Hyperweb2 All rights reserved.
# * GNU General Public License version 3; see www.hyperweb2.com/terms/

cmake_minimum_required(VERSION 2.8.9)

if (NOT WOWENGINE_335a)
  set(WOWENGINE_335a ON)
else()
  return()
endif()

include("${CMAKE_CURRENT_LIST_DIR}/../../project/CMakeLists.txt")

HW2_SET_PATH("CMAKE_WOWENGINE_335a_DIR" "${CMAKE_WOWENGINE_ROOT_DIR}/src/game/3.3.5a")
HW2_SET_PATH("CMAKE_WOWENGINE_335a_WORLD_DIR" "${CMAKE_WOWENGINE_335a_DIR}/world")
HW2_SET_PATH("CMAKE_WOWENGINE_335a_SCRIPTS_DIR" "${CMAKE_WOWENGINE_335a_DIR}/scripts")
HW2_SET_PATH("CMAKE_WOWENGINE_335a_SERVER_DIR" "${CMAKE_WOWENGINE_335a_DIR}/server")
HW2_SET_PATH("CMAKE_WOWENGINE_335a_TOOL_DIR" "${CMAKE_WOWENGINE_335a_DIR}/tool")
HW2_SET_PATH("CMAKE_WOWENGINE_335a_MODS_DIR" "${CMAKE_WOWENGINE_335a_DIR}/mods")

HW2_SET_CACHE("CMAKE_WOWENGINE_VERSION" "3.3.5a") # can be usefull for conditions

if(TOOLS)
  add_subdirectory("${CMAKE_WOWENGINE_MODULES_DIR}/dep" "${CMAKE_BINARY_DIR}/wowengine_dep") 
endif()

#include common libraries
include("${CMAKE_WOWENGINE_LIB_DIR}/DataStores/CMakeLists.txt")

if( SERVERS )
  include("${CMAKE_WOWENGINE_335a_DIR}/world/CMakeLists.txt")
  include("${CMAKE_WOWENGINE_335a_DIR}/collision/CMakeLists.txt")
  include("${CMAKE_WOWENGINE_335a_DIR}/scripts/CMakeLists.txt")

  set(sources_windows_Debugging
    ${CMAKE_HW2CPP_LIBRARIES}/Debugging/WheatyExceptionReport.cpp
    ${CMAKE_HW2CPP_LIBRARIES}/Debugging/WheatyExceptionReport.h
  )

  # must follow this order
  include("${CMAKE_WOWENGINE_335a_MODS_DIR}/CMakeLists.txt")
  include("${CMAKE_WOWENGINE_GAME_MODS_DIR}/CMakeLists.txt")
  include("${CMAKE_WOWENGINE_LIB_DIR}/mods/CMakeLists.txt")

  # load all mods
  LOAD_335a_MODS()
  LOAD_GAME_MODS()
  # then link mods library to other related
  target_link_libraries(world
    mods
  )

  include("${CMAKE_WOWENGINE_335a_DIR}/server/CMakeLists.txt")
else()
  if( TOOLS )
    include("${CMAKE_WOWENGINE_335a_DIR}/collision/CMakeLists.txt")
    include("${CMAKE_WOWENGINE_335a_DIR}/server/CMakeLists.txt")
  endif()
endif()

if ( TOOLS )
  add_subdirectory(tool)
endif(TOOLS)


