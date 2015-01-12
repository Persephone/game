# * Copyright (C) 2007 - 2014 Hyperweb2 All rights reserved.
# * GNU General Public License version 3; see www.hyperweb2.com/terms/

cmake_minimum_required(VERSION 2.8.9)

if (NOT WOWENGINE_335a_MODS)
  set(WOWENGINE_335a_MODS ON)
else()
  return()
endif()

include("${CMAKE_CURRENT_LIST_DIR}/../CMakeLists.txt")

# add mods
HW2_SUBDIRLIST(sub_DIRS  "${CMAKE_WOWENGINE_335a_MODS_DIR}" FALSE)
FOREACH(subdir ${sub_DIRS})
  if(EXISTS "${subdir}/CMakeLists.txt")
    include("${subdir}/CMakeLists.txt")
  endif()

    set(mods_DIRS
        ${mods_DIRS}
        ${subdir}
    )

ENDFOREACH()

# Select loader file
if(NOT EXISTS "${CMAKE_WOWENGINE_335a_MODS_DIR}/ModsLoader.h")
  HW2_SET_GLOBAL("MODS_LOADER" ON)
  include_directories("${CMAKE_WOWENGINE_335a_MODS_DIR}")
else()
  include_directories("${CMAKE_WOWENGINE_LIB_DIR}/Mods/loader")
endif()

include_directories(
  ${mods_DIRS}
)

message(STATUS "MODS PREPARATION COMPLETE")
message("")


#
# BELOW YOU CAN ADD YOUR CUSTOMIZATIONS
#
