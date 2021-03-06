# Copyright (C) 2005-2009 MaNGOS project <http://getmangos.com/>
# Copyright (C) 2008-2014 TrinityCore <http://www.trinitycore.org/>
#
# This file is free software; as a special exception the author gives
# unlimited permission to copy and/or distribute it, with or without
# modifications, as long as this notice is preserved.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY, to the extent permitted by law; without even the
# implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

include_directories(
  ${CMAKE_HW2CPP_DEP}/g3dlite/include
  ${CMAKE_HW2CPP_LIBRARIES}
  ${CMAKE_HW2CPP_LIBRARIES}/Common
  ${CMAKE_WOWENGINE_335a_DIR}/collision
  ${CMAKE_WOWENGINE_335a_DIR}/collision/Maps
  ${CMAKE_WOWENGINE_335a_DIR}/collision/Models
  ${ACE_INCLUDE_DIR}
  ${ZLIB_INCLUDE_DIR}
)

add_executable(vmap4assembler VMapAssembler.cpp)

if(CMAKE_SYSTEM_NAME MATCHES "Darwin")
  set_target_properties(vmap4assembler PROPERTIES LINK_FLAGS "-framework Carbon")
endif()

target_link_libraries(vmap4assembler
  collision
  g3dlib
  ${ZLIB_LIBRARIES}
)

if( UNIX )
  install(TARGETS vmap4assembler DESTINATION bin)
elseif( WIN32 )
  install(TARGETS vmap4assembler DESTINATION "${CMAKE_INSTALL_PREFIX}")
endif()
