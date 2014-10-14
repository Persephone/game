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

# Set wowengine directories
get_filename_component(CMAKE_MODULES_ROOT_DIR "../../../../" REALPATH)
add_subdirectory("${CMAKE_MODULES_ROOT_DIR}/trinitycore/src/project" "${CMAKE_BINARY_DIR}/tc_project")

set(CMAKE_MODULE_PATH "${CMAKE_TC_PROJECT}/cmake/macros")

TC_SET_GLOBAL("CMAKE_WOWENGINE_ROOT_DIR" "${CMAKE_MODULES_ROOT_DIR}/wowengine")
TC_SET_GLOBAL("CMAKE_WOWENGINE_335a_DIR" "${CMAKE_WOWENGINE_ROOT_DIR}/src/game/3.3.5a")

if( SERVERS )
  # Find revision ID and hash of the sourcetree
  include("${CMAKE_TC_PROJECT}/cmake/genrev.cmake")

  add_subdirectory(world)
  add_subdirectory(collision)
  add_subdirectory(scripts)

  set(sources_windows_Debugging
    ${CMAKE_TC_LIBRARIES}/core/Debugging/WheatyExceptionReport.cpp
    ${CMAKE_TC_LIBRARIES}/core/Debugging/WheatyExceptionReport.h
  )

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

  add_subdirectory(server)
else()
  if( TOOLS )
    add_subdirectory(collision)
    add_subdirectory(server)
  endif()
endif()

