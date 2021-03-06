# Copyright (C) 2008-2014 TrinityCore <http://www.trinitycore.org/>
#
# This file is free software; as a special exception the author gives
# unlimited permission to copy and/or distribute it, with or without
# modifications, as long as this notice is preserved.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY, to the extent permitted by law; without even the
# implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

########### authserver ###############

file(GLOB_RECURSE sources_authentication "${CMAKE_CURRENT_LIST_DIR}/Authentication/*.cpp" "${CMAKE_CURRENT_LIST_DIR}/Authentication/*.h")
file(GLOB_RECURSE sources_realms "${CMAKE_CURRENT_LIST_DIR}/Realms/*.cpp" "${CMAKE_CURRENT_LIST_DIR}/Realms/*.h")
file(GLOB_RECURSE sources_server "${CMAKE_CURRENT_LIST_DIR}/Server/*.cpp" "${CMAKE_CURRENT_LIST_DIR}/Server/*.h")
file(GLOB sources_localdir "${CMAKE_CURRENT_LIST_DIR}/*.cpp" "${CMAKE_CURRENT_LIST_DIR}/*.h")

if (USE_COREPCH)
  set(authserver_PCH_HDR "${CMAKE_CURRENT_LIST_DIR}/PrecompiledHeaders/authPCH.h")
  set(authserver_PCH_SRC "${CMAKE_CURRENT_LIST_DIR}/PrecompiledHeaders/authPCH.cpp")
endif()

set(authserver_SRCS
  ${authserver_SRCS}
  ${sources_authentication}
  ${sources_realms}
  ${sources_server}
  ${sources_localdir}
)

if( WIN32 )
  set(authserver_SRCS
    ${authserver_SRCS}
    ${sources_windows_Debugging}
  )
  if ( MSVC )
    set(authserver_SRCS
      ${authserver_SRCS}
      authserver.rc
    )
  endif ()
endif()

include_directories(
  ${CMAKE_HW2CPP_LIBRARIES}/Common
  ${CMAKE_HW2CPP_LIBRARIES}/Configuration
  ${CMAKE_HW2CPP_LIBRARIES}/Database
  ${CMAKE_HW2CPP_LIBRARIES}/Debugging
  ${CMAKE_HW2CPP_LIBRARIES}/Packets
  ${CMAKE_HW2CPP_LIBRARIES}/Cryptography
  ${CMAKE_HW2CPP_LIBRARIES}/Cryptography/Authentication
  ${CMAKE_HW2CPP_LIBRARIES}/Logging
  ${CMAKE_HW2CPP_LIBRARIES}/Networking
  ${CMAKE_HW2CPP_LIBRARIES}/Threading
  ${CMAKE_HW2CPP_LIBRARIES}/Utilities
  ${CMAKE_CURRENT_LIST_DIR}/Authentication
  ${CMAKE_CURRENT_LIST_DIR}/Realms
  ${CMAKE_CURRENT_LIST_DIR}/Server
  ${MYSQL_INCLUDE_DIR}
  ${OPENSSL_INCLUDE_DIR}
  ${VALGRIND_INCLUDE_DIR}
)

add_executable(authserver
  ${authserver_SRCS}
  ${authserver_PCH_SRC}
)

add_dependencies(authserver revision.h)

if( NOT WIN32 )
  HW2_GET_GLOBAL("CONF_DIR")
  set_target_properties(authserver PROPERTIES
    COMPILE_DEFINITIONS _TRINITY_REALM_CONFIG="${CONF_DIR}/authserver.conf"
  )
endif()

target_link_libraries(authserver
  core
  ${MYSQL_LIBRARY}
  ${OPENSSL_LIBRARIES}
  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
)

if( WIN32 )
  if ( MSVC )
    add_custom_command(TARGET authserver
      POST_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_LIST_DIR}/authserver.conf.dist ${CMAKE_BINARY_DIR}/bin/$(ConfigurationName)/
    )
  elseif ( MINGW )
    add_custom_command(TARGET authserver
      POST_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_LIST_DIR}/authserver.conf.dist ${CMAKE_BINARY_DIR}/bin/
    )
  endif()
endif()

if( UNIX )
  install(TARGETS authserver DESTINATION bin)
  install(FILES  ${CMAKE_CURRENT_LIST_DIR}/authserver.conf.dist DESTINATION ${CONF_DIR})
elseif( WIN32 )
  install(TARGETS authserver DESTINATION "${CMAKE_INSTALL_PREFIX}")
  install(FILES ${CMAKE_CURRENT_LIST_DIR}/authserver.conf.dist DESTINATION "${CMAKE_INSTALL_PREFIX}")
endif()

# Generate precompiled header
if (USE_COREPCH)
  add_cxx_pch(authserver ${authserver_PCH_HDR} ${authserver_PCH_SRC})
endif()
