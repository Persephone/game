# Copyright (C) 2008-2014 TrinityCore <http://www.trinitycore.org/>
#
# This file is free software; as a special exception the author gives
# unlimited permission to copy and/or distribute it, with or without
# modifications, as long as this notice is preserved.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY, to the extent permitted by law; without even the
# implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

if( USE_COREPCH )
  include_directories(${CMAKE_CURRENT_BINARY_DIR})
endif()

file(GLOB_RECURSE sources_Management Management/*.cpp Management/*.h)
file(GLOB_RECURSE sources_Maps Maps/*.cpp Maps/*.h)
file(GLOB_RECURSE sources_Models Models/*.cpp Models/*.h)
file(GLOB sources_localdir *.cpp *.h)

if (USE_COREPCH)
  set(collision_STAT_PCH_HDR PrecompiledHeaders/collisionPCH.h)
  set(collision_STAT_PCH_SRC PrecompiledHeaders/collisionPCH.cpp)
endif ()

set(collision_STAT_SRCS
  ${collision_STAT_SRCS}
  ${sources_Management}
  ${sources_Maps}
  ${sources_Models}
  ${sources_localdir}
)

include_directories(
  ${CMAKE_HW2CPP_DEP}/g3dlite/include
  ${CMAKE_HW2CPP_DEP}/recastnavigation/Detour
  ${CMAKE_HW2CPP_DEP}/recastnavigation/Detour/Include
  ${CMAKE_HW2CPP_LIBRARIES}/Common
  ${CMAKE_HW2CPP_LIBRARIES}/Configuration
  ${CMAKE_HW2CPP_LIBRARIES}/Debugging
  ${CMAKE_HW2CPP_LIBRARIES}/Database
  ${CMAKE_HW2CPP_LIBRARIES}/Debugging
  ${CMAKE_HW2CPP_LIBRARIES}/DataStructures
  ${CMAKE_HW2CPP_LIBRARIES}/DataStructures/LinkedReference
  ${CMAKE_HW2CPP_LIBRARIES}/Logging
  ${CMAKE_HW2CPP_LIBRARIES}/Threading
  ${CMAKE_HW2CPP_LIBRARIES}/Packets
  ${CMAKE_HW2CPP_LIBRARIES}/Utilities
  ${CMAKE_WOWENGINE_LIB_DIR}/DataStores
  ${CMAKE_WOWENGINE_335a_DIR}/world/Addons
  ${CMAKE_WOWENGINE_335a_DIR}/world/Conditions
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Item
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/GameObject
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Creature
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Object
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Object/Updates
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Unit
  ${CMAKE_WOWENGINE_335a_DIR}/world/Combat
  ${CMAKE_WOWENGINE_335a_DIR}/world/Loot
  ${CMAKE_WOWENGINE_335a_DIR}/world/Miscellaneous
  ${CMAKE_WOWENGINE_335a_DIR}/world/Grids
  ${CMAKE_WOWENGINE_335a_DIR}/world/Grids/Cells
  ${CMAKE_WOWENGINE_335a_DIR}/world/Grids/Notifiers
  ${CMAKE_WOWENGINE_335a_DIR}/world/Maps
  ${CMAKE_WOWENGINE_335a_DIR}/world/DataStores
  ${CMAKE_WOWENGINE_335a_DIR}/world/Movement/Waypoints
  ${CMAKE_WOWENGINE_335a_DIR}/world/Movement/Spline
  ${CMAKE_WOWENGINE_335a_DIR}/world/Movement
  ${CMAKE_WOWENGINE_335a_DIR}/world/Server
  ${CMAKE_WOWENGINE_335a_DIR}/world/Server/Protocol
  ${CMAKE_WOWENGINE_335a_DIR}/world/World
  ${CMAKE_WOWENGINE_335a_DIR}/world/Spells
  ${CMAKE_WOWENGINE_335a_DIR}/world/Spells/Auras
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${CMAKE_CURRENT_SOURCE_DIR}/Management
  ${CMAKE_CURRENT_SOURCE_DIR}/Maps
  ${CMAKE_CURRENT_SOURCE_DIR}/Models
  ${MYSQL_INCLUDE_DIR}
  ${VALGRIND_INCLUDE_DIR}
)

add_library(collision STATIC
  ${collision_STAT_SRCS}
  ${collision_STAT_PCH_SRC}
)

target_link_libraries(collision
  core
)

# Generate precompiled header
if (USE_COREPCH)
  add_cxx_pch(collision ${collision_STAT_PCH_HDR} ${collision_STAT_PCH_SRC})
endif ()
