# Copyright (C) 2008-2014 TrinityCore <http://www.trinitycore.org/>
#
# This file is free software; as a special exception the author gives
# unlimited permission to copy and/or distribute it, with or without
# modifications, as long as this notice is preserved.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY, to the extent permitted by law; without even the
# implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

# Enforce compileparameters for corebuilds under GCC
# This to stop a few silly crashes that could have been avoided IF people
# weren't doing some -O3 psychooptimizations etc.

# CMake policies (can not be handled elsewhere)
cmake_minimum_required(VERSION 2.8.9)
cmake_policy(SET CMP0005 OLD)
if(POLICY CMP0043)
  cmake_policy(SET CMP0043 OLD) # Disable 'Ignore COMPILE_DEFINITIONS_<Config> properties'
endif(POLICY CMP0043)

if(CMAKE_COMPILER_IS_GNUCXX AND NOT MINGW)
  add_definitions(-fno-delete-null-pointer-checks)
endif()

# Informations and definitions
if( SERVERS )
  message("* Build world/auth       : Yes (default)")
else()
  message("* Build world/authserver : No")
endif()

if( SCRIPTS )
  message("* Build with scripts     : Yes (default)")
  add_definitions(-DSCRIPTS)
else()
  message("* Build with scripts     : No")
endif()

if( TOOLS )
  message("* Build map/vmap tools   : Yes")
  add_definitions(-DNO_CORE_FUNCS)
else()
  message("* Build map/vmap tools   : No  (default)")
endif()

if( USE_SCRIPTPCH )
  message("* Build scripts w/PCH    : Yes (default)")
else()
  message("* Build scripts w/PCH    : No")
endif()

if( WITH_COREDEBUG )
  add_definitions(-DTRINITY_DEBUG)
endif()

get_filename_component(WOWENGINE_SRC "../../" REALPATH)
add_subdirectory("${WOWENGINE_SRC}/project" "${CMAKE_BINARY_DIR}/wowengine_project")

HW2_SET_PATH("CMAKE_WOWENGINE_335a_DIR" "${CMAKE_WOWENGINE_ROOT_DIR}/src/game/3.3.5a")
HW2_SET_PATH("CMAKE_WOWENGINE_335a_WORLD_DIR" "${CMAKE_WOWENGINE_335a_DIR}/world")
HW2_SET_PATH("CMAKE_WOWENGINE_335a_SCRIPTS_DIR" "${CMAKE_WOWENGINE_335a_DIR}/scripts")
HW2_SET_PATH("CMAKE_WOWENGINE_335a_SERVER_DIR" "${CMAKE_WOWENGINE_335a_DIR}/server")
HW2_SET_PATH("CMAKE_WOWENGINE_335a_TOOL_DIR" "${CMAKE_WOWENGINE_335a_DIR}/tool")
HW2_SET_CACHE("CMAKE_WOWENGINE_VERSION" "3.3.5a") # can be usefull for conditions

find_package(PCHSupport)
find_package(OpenSSL REQUIRED)
find_package(Threads REQUIRED)

include(ConfigureBoost)
find_package(MySQL REQUIRED)

if( UNIX )
  find_package(Readline)
  find_package(ZLIB)
  find_package(BZip2)
endif()

if(NOT WITHOUT_GIT)
  find_package(Git)
endif()

HW2_GET_GLOBAL("CMAKE_HW2_ALLPATHS")

include_directories(
  ${CMAKE_HW2_ALLPATHS}
  ${CMAKE_BINARY_DIR}
  ${CMAKE_BINARY_DIR}/hw2_project
  ${CMAKE_BINARY_DIR}/wowengine_project
)

if( SERVERS )
  add_subdirectory(world)
  add_subdirectory(collision)
  add_subdirectory(scripts)

  set(sources_windows_Debugging
    ${CMAKE_HW2CPP_LIBRARIES}/Debugging/WheatyExceptionReport.cpp
    ${CMAKE_HW2CPP_LIBRARIES}/Debugging/WheatyExceptionReport.h
  )

  add_subdirectory(server)

  # INCLUDE MODS
  include("${CMAKE_WOWENGINE_MODS_DIR}/CMakeLists.txt")
else()
  if( TOOLS )
    add_subdirectory(collision)
    add_subdirectory(server)
  endif()
endif()

if ( TOOLS )
  add_subdirectory(tool)
endif(TOOLS)


