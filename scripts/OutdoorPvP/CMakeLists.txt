# * Copyright (C) 2007 - 2014 Hyperweb2 All rights reserved.
# * GNU General Public License version 3; see www.hyperweb2.com/terms/

file(GLOB_RECURSE current_dir_SRCS "${CMAKE_CURRENT_LIST_DIR}/*.cpp" "${CMAKE_CURRENT_LIST_DIR}/*.h")

set(scripts_STAT_SRCS
  ${scripts_STAT_SRCS}
  ${current_dir_SRCS}
)

HW2_SUBDIRLIST(sub_DIRS  "${CMAKE_CURRENT_LIST_DIR}" FALSE)
FOREACH(subdir ${sub_DIRS})
    file(GLOB sources "${subdir}/*.cpp" "${subdir}/*.h")

    set(scripts_STAT_SRCS
      ${scripts_STAT_SRCS}
      ${sources}
    )
ENDFOREACH()

message("  -> Prepared: Outdoor PVP Zones")
