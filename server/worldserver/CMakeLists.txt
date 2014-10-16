# Copyright (C) 2008-2014 TrinityCore <http://www.trinitycore.org/>
#
# This file is free software; as a special exception the author gives
# unlimited permission to copy and/or distribute it, with or without
# modifications, as long as this notice is preserved.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY, to the extent permitted by law; without even the
# implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

file(GLOB_RECURSE sources_CommandLine CommandLine/*.cpp CommandLine/*.h)
file(GLOB_RECURSE sources_RemoteAccess RemoteAccess/*.cpp RemoteAccess/*.h)
file(GLOB_RECURSE sources_TCSoap TCSoap/*.cpp TCSoap/*.h)
file(GLOB sources_localdir *.cpp *.h)

if (USE_COREPCH)
  set(worldserver_PCH_HDR PrecompiledHeaders/worldserverPCH.h)
  set(worldserver_PCH_SRC PrecompiledHeaders/worldserverPCH.cpp)
endif()

set(worldserver_SRCS
  ${worldserver_SRCS}
  ${sources_CommandLine}
  ${sources_RemoteAccess}
  ${sources_TCSoap}
  ${sources_localdir}
)

if( WIN32 )
  set(worldserver_SRCS
    ${worldserver_SRCS}
    ${sources_windows_Debugging}
  )
  if ( MSVC )
    set(worldserver_SRCS
      ${worldserver_SRCS}
      worldserver.rc
    )
  endif()
endif()

include_directories(
  ${CMAKE_BINARY_DIR}
  ${CMAKE_HW2CPP_DEP}/g3dlite/include
  ${CMAKE_HW2CPP_DEP}/recastnavigation/Detour
  ${CMAKE_HW2CPP_DEP}/recastnavigation/Detour/Include
  ${CMAKE_HW2CPP_DEP}/gsoap
  ${CMAKE_HW2CPP_DEP}/sockets/include
  ${CMAKE_HW2CPP_DEP}/SFMT
  ${CMAKE_WOWENGINE_335a_DIR}/collision
  ${CMAKE_WOWENGINE_335a_DIR}/collision/Management
  ${CMAKE_WOWENGINE_335a_DIR}/collision/Models
  ${CMAKE_HW2CPP_LIBRARIES}
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
  ${CMAKE_WOWENGINE_335a_DIR}/world
  ${CMAKE_WOWENGINE_335a_DIR}/world/Accounts
  ${CMAKE_WOWENGINE_335a_DIR}/world/Achievements
  ${CMAKE_WOWENGINE_335a_DIR}/world/Addons
  ${CMAKE_WOWENGINE_335a_DIR}/world/AI
  ${CMAKE_WOWENGINE_335a_DIR}/world/AI/CoreAI
  ${CMAKE_WOWENGINE_335a_DIR}/world/AI/ScriptedAI
  ${CMAKE_WOWENGINE_335a_DIR}/world/AI/SmartScripts
  ${CMAKE_WOWENGINE_335a_DIR}/world/AuctionHouse
  ${CMAKE_WOWENGINE_335a_DIR}/world/AuctionHouse/AuctionHouseBot
  ${CMAKE_WOWENGINE_335a_DIR}/world/Battlegrounds
  ${CMAKE_WOWENGINE_335a_DIR}/world/Battlegrounds/Zones
  ${CMAKE_WOWENGINE_335a_DIR}/world/Calendar
  ${CMAKE_WOWENGINE_335a_DIR}/world/Chat
  ${CMAKE_WOWENGINE_335a_DIR}/world/Chat/Channels
  ${CMAKE_WOWENGINE_335a_DIR}/world/Combat
  ${CMAKE_WOWENGINE_335a_DIR}/world/Conditions
  ${CMAKE_WOWENGINE_335a_DIR}/world/DataStores
  ${CMAKE_WOWENGINE_335a_DIR}/world/DungeonFinding
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Creature
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Corpse
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/DynamicObject
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/GameObject
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Item
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Item/Container
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Object
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Object/Updates
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Pet
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Player
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Totem
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Unit
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Vehicle
  ${CMAKE_WOWENGINE_335a_DIR}/world/Entities/Transport
  ${CMAKE_WOWENGINE_335a_DIR}/world/Events
  ${CMAKE_WOWENGINE_335a_DIR}/world/Globals
  ${CMAKE_WOWENGINE_335a_DIR}/world/Grids/Cells
  ${CMAKE_WOWENGINE_335a_DIR}/world/Grids/Notifiers
  ${CMAKE_WOWENGINE_335a_DIR}/world/Grids
  ${CMAKE_WOWENGINE_335a_DIR}/world/Groups
  ${CMAKE_WOWENGINE_335a_DIR}/world/Guilds
  ${CMAKE_WOWENGINE_335a_DIR}/world/Handlers
  ${CMAKE_WOWENGINE_335a_DIR}/world/Instances
  ${CMAKE_WOWENGINE_335a_DIR}/world/Loot
  ${CMAKE_WOWENGINE_335a_DIR}/world/Mails
  ${CMAKE_WOWENGINE_335a_DIR}/world/Maps
  ${CMAKE_WOWENGINE_335a_DIR}/world/Miscellaneous
  ${CMAKE_WOWENGINE_335a_DIR}/world/Movement
  ${CMAKE_WOWENGINE_335a_DIR}/world/Movement/MovementGenerators
  ${CMAKE_WOWENGINE_335a_DIR}/world/Movement/Waypoints
  ${CMAKE_WOWENGINE_335a_DIR}/world/OutdoorPvP
  ${CMAKE_WOWENGINE_335a_DIR}/world/Pools
  ${CMAKE_WOWENGINE_335a_DIR}/world/PrecompiledHeaders
  ${CMAKE_WOWENGINE_335a_DIR}/world/Quests
  ${CMAKE_WOWENGINE_335a_DIR}/world/Reputation
  ${CMAKE_WOWENGINE_335a_DIR}/world/Scripting
  ${CMAKE_WOWENGINE_335a_DIR}/world/Server/Protocol
  ${CMAKE_WOWENGINE_335a_DIR}/world/Server
  ${CMAKE_WOWENGINE_335a_DIR}/world/Skills
  ${CMAKE_WOWENGINE_335a_DIR}/world/Spells
  ${CMAKE_WOWENGINE_335a_DIR}/world/Spells/Auras
  ${CMAKE_WOWENGINE_335a_DIR}/world/Tools
  ${CMAKE_WOWENGINE_335a_DIR}/world/Warden
  ${CMAKE_WOWENGINE_335a_DIR}/world/Warden/Modules
  ${CMAKE_WOWENGINE_335a_DIR}/world/Weather
  ${CMAKE_WOWENGINE_335a_DIR}/world/World
  ${CMAKE_WOWENGINE_335a_DIR}/server/authserver/Server
  ${CMAKE_WOWENGINE_335a_DIR}/server/authserver/Realms
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${CMAKE_CURRENT_SOURCE_DIR}/CommandLine
  ${CMAKE_CURRENT_SOURCE_DIR}/RemoteAccess
  ${CMAKE_CURRENT_SOURCE_DIR}/TCSoap
  ${MYSQL_INCLUDE_DIR}
  ${OPENSSL_INCLUDE_DIR}
  ${VALGRIND_INCLUDE_DIR}
)

add_executable(worldserver
  ${worldserver_SRCS}
  ${worldserver_PCH_SRC}
)

if( NOT WIN32 )
  HW2_GET_GLOBAL("CONF_DIR")
  set_target_properties(worldserver PROPERTIES
    COMPILE_DEFINITIONS _TRINITY_CORE_CONFIG="${CONF_DIR}/worldserver.conf"
  )
endif()

add_dependencies(worldserver revision.h)

if( UNIX AND NOT NOJEM AND NOT APPLE )
  set(worldserver_LINK_FLAGS "-pthread -lncurses ${worldserver_LINK_FLAGS}")
endif()

set_target_properties(worldserver PROPERTIES LINK_FLAGS "${worldserver_LINK_FLAGS}")

target_link_libraries(worldserver
  wowengine
  world
  core
  scripts
  collision
  g3dlib
  gsoap
  Detour
  ${JEMALLOC_LIBRARY}
  ${READLINE_LIBRARY}
  ${TERMCAP_LIBRARY}
  ${MYSQL_LIBRARY}
  ${OPENSSL_LIBRARIES}
  ${ZLIB_LIBRARIES}
  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
)

if( WIN32 )
  if ( MSVC )
    add_custom_command(TARGET worldserver
      POST_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/worldserver.conf.dist ${CMAKE_BINARY_DIR}/bin/$(ConfigurationName)/
    )
  elseif ( MINGW )
    add_custom_command(TARGET worldserver
      POST_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/worldserver.conf.dist ${CMAKE_BINARY_DIR}/bin/
    )
  endif()
endif()

if( UNIX )
  install(TARGETS worldserver DESTINATION bin)
  install(FILES worldserver.conf.dist DESTINATION ${CONF_DIR})
elseif( WIN32 )
  install(TARGETS worldserver DESTINATION "${CMAKE_INSTALL_PREFIX}")
  install(FILES worldserver.conf.dist DESTINATION "${CMAKE_INSTALL_PREFIX}")
endif()

# Generate precompiled header
if( USE_COREPCH )
  add_cxx_pch(worldserver ${worldserver_PCH_HDR} ${worldserver_PCH_SRC})
endif()
