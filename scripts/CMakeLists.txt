# Copyright (C) 2008-2014 TrinityCore <http://www.trinitycore.org/>
#
# This file is free software; as a special exception the author gives
# unlimited permission to copy and/or distribute it, with or without
# modifications, as long as this notice is preserved.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY, to the extent permitted by law; without even the
# implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

# Enable precompiled headers when using the GCC compiler.

cmake_minimum_required(VERSION 2.8.9)

if (NOT WOWENGINE_335a_SCRIPTS)
  set(WOWENGINE_335a_SCRIPTS ON)
else()
  return()
endif()

include("${CMAKE_CURRENT_LIST_DIR}/../CMakeLists.txt")

if (USE_SCRIPTPCH)
  set(scripts_STAT_PCH_HDR ${CMAKE_CURRENT_LIST_DIR}/PrecompiledHeaders/ScriptPCH.h)
  set(scripts_STAT_PCH_SRC ${CMAKE_CURRENT_LIST_DIR}/PrecompiledHeaders/ScriptPCH.cpp)
endif ()

message(STATUS "SCRIPT PREPARATIONS")
include("${CMAKE_CURRENT_LIST_DIR}/Spells/CMakeLists.txt")

include("${CMAKE_CURRENT_LIST_DIR}/Commands/CMakeLists.txt")

set(scripts_STAT_SRCS
  ${scripts_STAT_SRCS}
  "${CMAKE_CURRENT_LIST_DIR}/../world/AI/ScriptedAI/ScriptedEscortAI.cpp"
  "${CMAKE_CURRENT_LIST_DIR}/../world/AI/ScriptedAI/ScriptedCreature.cpp"
  "${CMAKE_CURRENT_LIST_DIR}/../world/AI/ScriptedAI/ScriptedFollowerAI.cpp"
)

if(SCRIPTS)
  include("${CMAKE_CURRENT_LIST_DIR}/World/CMakeLists.txt")
  include("${CMAKE_CURRENT_LIST_DIR}/OutdoorPvP/CMakeLists.txt")
  include("${CMAKE_CURRENT_LIST_DIR}/EasternKingdoms/CMakeLists.txt")
  include("${CMAKE_CURRENT_LIST_DIR}/Kalimdor/CMakeLists.txt")
  include("${CMAKE_CURRENT_LIST_DIR}/Outland/CMakeLists.txt")
  include("${CMAKE_CURRENT_LIST_DIR}/Northrend/CMakeLists.txt")
  include("${CMAKE_CURRENT_LIST_DIR}/Events/CMakeLists.txt")
  include("${CMAKE_CURRENT_LIST_DIR}/Pet/CMakeLists.txt")
endif()

message(STATUS "SCRIPT PREPARATION COMPLETE")
message("")

include_directories(
  ${CMAKE_HW2CPP_DEP}/recastnavigation/Detour
  ${CMAKE_HW2CPP_DEP}/recastnavigation/Detour/Include
  ${CMAKE_HW2CPP_DEP}/recastnavigation/Recast
  ${CMAKE_HW2CPP_DEP}/recastnavigation/Recast/Include
  ${CMAKE_HW2CPP_DEP}/g3dlite/include
  ${CMAKE_HW2CPP_DEP}/SFMT
  ${CMAKE_HW2CPP_DEP}/zlib
  ${CMAKE_HW2CPP_LIBRARIES}/Common
  ${CMAKE_HW2CPP_LIBRARIES}/Configuration
  ${CMAKE_HW2CPP_LIBRARIES}/Cryptography
  ${CMAKE_HW2CPP_LIBRARIES}/Database
  ${CMAKE_WOWENGINE_LIB_DIR}/DataStores
  ${CMAKE_HW2CPP_LIBRARIES}/Debugging
  ${CMAKE_HW2CPP_LIBRARIES}/DataStructures/LinkedReference
  ${CMAKE_HW2CPP_LIBRARIES}/DataStructures
  ${CMAKE_HW2CPP_LIBRARIES}/Logging
  ${CMAKE_HW2CPP_LIBRARIES}/Packets
  ${CMAKE_HW2CPP_LIBRARIES}/Threading
  ${CMAKE_HW2CPP_LIBRARIES}/Utilities
  ${CMAKE_WOWENGINE_335a_DIR}/collision
  ${CMAKE_WOWENGINE_335a_DIR}/collision/Management
  ${CMAKE_WOWENGINE_335a_DIR}/collision/Models
  ${CMAKE_HW2CPP_LIBRARIES}/Database
  ${CMAKE_WOWENGINE_335a_DIR}/world/Accounts
  ${CMAKE_WOWENGINE_335a_DIR}/world/Achievements
  ${CMAKE_WOWENGINE_335a_DIR}/world/Addons
  ${CMAKE_WOWENGINE_335a_DIR}/world/AI
  ${CMAKE_WOWENGINE_335a_DIR}/world/AI/CoreAI
  ${CMAKE_WOWENGINE_335a_DIR}/world/AI/ScriptedAI
  ${CMAKE_WOWENGINE_335a_DIR}/world/AI/SmartScripts
  ${CMAKE_WOWENGINE_335a_DIR}/world/AuctionHouse
  ${CMAKE_WOWENGINE_335a_DIR}/world/AuctionHouseBot
  ${CMAKE_WOWENGINE_335a_DIR}/world/Battlefield
  ${CMAKE_WOWENGINE_335a_DIR}/world/Battlefield/Zones
  ${CMAKE_WOWENGINE_335a_DIR}/world/Battlegrounds
  ${CMAKE_WOWENGINE_335a_DIR}/world/Battlegrounds/Zones
  ${CMAKE_WOWENGINE_335a_DIR}/world/Calendar
  ${CMAKE_WOWENGINE_335a_DIR}/world/Chat
  ${CMAKE_WOWENGINE_335a_DIR}/world/Chat/Channels
  ${CMAKE_WOWENGINE_335a_DIR}/world/Conditions
  ${CMAKE_HW2CPP_LIBRARIES}/Configuration
  ${CMAKE_WOWENGINE_335a_DIR}/world/Combat
  ${CMAKE_WOWENGINE_335a_DIR}/world/DataStores
  ${CMAKE_WOWENGINE_335a_DIR}/world/DungeonFinding
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Corpse
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Creature
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/DynamicObject
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Item
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Item/Container
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/GameObject
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Object
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Object/Updates
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Pet
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Player
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Transport
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Unit
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Vehicle
  ${CMAKE_WOWENGINE_335a_DIR}/world/Events
  ${CMAKE_WOWENGINE_335a_DIR}/world/Globals
  ${CMAKE_WOWENGINE_335a_DIR}/world/Grids
  ${CMAKE_WOWENGINE_335a_DIR}/world/Grids/Cells
  ${CMAKE_WOWENGINE_335a_DIR}/world/Grids/Notifiers
  ${CMAKE_WOWENGINE_335a_DIR}/world/Groups
  ${CMAKE_WOWENGINE_335a_DIR}/world/Guilds
  ${CMAKE_WOWENGINE_335a_DIR}/world/Handlers
  ${CMAKE_WOWENGINE_335a_DIR}/world/Instances
  ${CMAKE_WOWENGINE_335a_DIR}/world/LookingForGroup
  ${CMAKE_WOWENGINE_335a_DIR}/world/Loot
  ${CMAKE_WOWENGINE_335a_DIR}/world/Mails
  ${CMAKE_WOWENGINE_335a_DIR}/world/Miscellaneous
  ${CMAKE_WOWENGINE_335a_DIR}/world/Maps
  ${CMAKE_WOWENGINE_335a_DIR}/world/Movement
  ${CMAKE_WOWENGINE_335a_DIR}/world/Movement/MovementGenerators
  ${CMAKE_WOWENGINE_335a_DIR}/world/Movement/Spline
  ${CMAKE_WOWENGINE_335a_DIR}/world/Movement/Waypoints
  ${CMAKE_WOWENGINE_335a_DIR}/world/Opcodes
  ${CMAKE_WOWENGINE_335a_DIR}/world/OutdoorPvP
  ${CMAKE_WOWENGINE_335a_DIR}/world/Pools
  ${CMAKE_WOWENGINE_335a_DIR}/world/PrecompiledHeaders
  ${CMAKE_WOWENGINE_335a_DIR}/world/Quests
  ${CMAKE_WOWENGINE_335a_DIR}/world/Reputation
  ${CMAKE_WOWENGINE_335a_DIR}/world/Scripting
  ${CMAKE_WOWENGINE_335a_DIR}/world/Server
  ${CMAKE_WOWENGINE_335a_DIR}/world/Server/Protocol
  ${CMAKE_WOWENGINE_335a_DIR}/world/Skills
  ${CMAKE_WOWENGINE_335a_DIR}/world/Spells
  ${CMAKE_WOWENGINE_335a_DIR}/world/Spells/Auras
  ${CMAKE_WOWENGINE_335a_DIR}/world/Texts
  ${CMAKE_WOWENGINE_335a_DIR}/world/Tickets
  ${CMAKE_WOWENGINE_335a_DIR}/world/Tools
  ${CMAKE_WOWENGINE_335a_DIR}/world/Warden
  ${CMAKE_WOWENGINE_335a_DIR}/world/Warden/Modules
  ${CMAKE_WOWENGINE_335a_DIR}/world/Weather
  ${CMAKE_WOWENGINE_335a_DIR}/world/World
  ${CMAKE_CURRENT_SOURCE_DIR}/PrecompiledHeaders
  ${MYSQL_INCLUDE_DIR}
  ${VALGRIND_INCLUDE_DIR}
)

add_library(scripts STATIC
  ${scripts_STAT_SRCS}
  ${scripts_STAT_PCH_SRC}
)

add_dependencies(scripts revision.h)

# Generate precompiled header
if (USE_SCRIPTPCH)
  add_cxx_pch(scripts ${scripts_STAT_PCH_HDR} ${scripts_STAT_PCH_SRC})
endif()
