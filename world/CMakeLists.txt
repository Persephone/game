# Copyright (C) 2008-2014 TrinityCore <http://www.trinitycore.org/>
#
# This file is free software; as a special exception the author gives
# unlimited permission to copy and/or distribute it, with or without
# modifications, as long as this notice is preserved.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY, to the extent permitted by law; without even the
# implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

cmake_minimum_required(VERSION 2.8.9)

if (NOT WOWENGINE_335a_WORLD)
  set(WOWENGINE_335a_WORLD ON)
else()
  return()
endif()

HW2_SUBDIRLIST(sub_DIRS  "${CMAKE_CURRENT_LIST_DIR}" TRUE)
FOREACH(subdir ${sub_DIRS})
    file(GLOB sources "${subdir}/*.cpp" "${subdir}/*.h")

    set(world_STAT_SRCS
        ${world_STAT_SRCS}
        ${sources}
    )

    set(world_DIRS
        ${subdir}
        ${world_DIRS}
    )
ENDFOREACH()

# Create world-libary

if (USE_COREPCH)
  set(world_STAT_PCH_HDR ${CMAKE_CURRENT_LIST_DIR}/PrecompiledHeaders/worldPCH.h)
  set(world_STAT_PCH_SRC ${CMAKE_CURRENT_LIST_DIR}/PrecompiledHeaders/worldPCH.cpp)
endif ()

include_directories(
  ${CMAKE_HW2CPP_DEP}/recastnavigation/Detour
  ${CMAKE_HW2CPP_DEP}/recastnavigation/Detour/Include
  ${CMAKE_HW2CPP_DEP}/recastnavigation/Recast
  ${CMAKE_HW2CPP_DEP}/recastnavigation/Recast/Include
  ${CMAKE_HW2CPP_DEP}/g3dlite/include
  ${CMAKE_HW2CPP_DEP}/SFMT
  ${CMAKE_HW2CPP_DEP}/zlib
  ${CMAKE_WOWENGINE_335a_DIR}/collision
  ${CMAKE_WOWENGINE_335a_DIR}/collision/Management
  ${CMAKE_WOWENGINE_335a_DIR}/collision/Models
  ${CMAKE_WOWENGINE_335a_DIR}/collision/Maps
  ${CMAKE_HW2CPP_LIBRARIES}/Common
  ${CMAKE_HW2CPP_LIBRARIES}/Configuration
  ${CMAKE_HW2CPP_LIBRARIES}/Cryptography
  ${CMAKE_HW2CPP_LIBRARIES}/Cryptography/Authentication
  ${CMAKE_HW2CPP_LIBRARIES}/Database
  ${CMAKE_WOWENGINE_LIB_DIR}/DataStores
  ${CMAKE_HW2CPP_LIBRARIES}/Debugging
  ${CMAKE_HW2CPP_LIBRARIES}/DataStructures/LinkedReference
  ${CMAKE_HW2CPP_LIBRARIES}/DataStructures
  ${CMAKE_HW2CPP_LIBRARIES}/Logging
  ${CMAKE_HW2CPP_LIBRARIES}/Networking
  ${CMAKE_HW2CPP_LIBRARIES}/Packets
  ${CMAKE_HW2CPP_LIBRARIES}/Threading
  ${CMAKE_HW2CPP_LIBRARIES}/Utilities
  ${world_DIRS}
  ${CMAKE_WOWENGINE_335a_DIR}/scripts/PrecompiledHeaders
  ${MYSQL_INCLUDE_DIR}
  ${OPENSSL_INCLUDE_DIR}
  ${VALGRIND_INCLUDE_DIR}
)

add_library(world STATIC
  ${world_STAT_SRCS}
  ${world_STAT_PCH_SRC}
)

target_link_libraries(world
  mods
  datastores
)

add_dependencies(world revision.h)

# Generate precompiled header
if (USE_COREPCH)
  add_cxx_pch(world ${world_STAT_PCH_HDR} ${world_STAT_PCH_SRC})
endif ()
