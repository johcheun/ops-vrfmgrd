# Copyright (C) 2016 Hewlett-Packard Development Company, L.P.
# All Rights Reserved.
#
#    Licensed under the Apache License, Version 2.0 (the "License"); you may
#    not use this file except in compliance with the License. You may obtain
#    a copy of the License at
#
#         http://www.apache.org/licenses/LICENSE-2.0
#
#    Unless required by applicable law or agreed to in writing, software
#    distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
#    WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
#    License for the specific language governing permissions and limitations
#    under the License.

cmake_minimum_required (VERSION 2.8)

set (VRFMGRD ops-vrfmgrd)
project (${VRFMGRD})
set (SRC_DIR src)
set (INCL_DIR include)

# Rules to locate needed libraries
include(FindPkgConfig)
pkg_check_modules(OVSCOMMON REQUIRED libovscommon)
pkg_check_modules(OVSDB REQUIRED libovsdb)
pkg_check_modules(OPSUTILS REQUIRED opsutils)

include_directories (${PROJECT_BINARY_DIR} ${PROJECT_SOURCE_DIR}/${INCL_DIR}
                     ${OVSCOMMON_INCLUDE_DIRS}
)

set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Werror")

# Source files to build ops-vrfmgrd
set (SOURCES ${SRC_DIR}/vrfmgrd.c ${SRC_DIR}/vrfmgrd_ovsdb_if.c ${SRC_DIR}/vrfmgrd_tableid_if.c)

# Rules to build ops-vrfmgrd
add_executable (${VRFMGRD} ${SOURCES})
target_link_libraries (${VRFMGRD} ${OVSCOMMON_LIBRARIES}
                       ${OVSDB_LIBRARIES} ${OPSUTILS_LIBRARIES}
                       -lpthread -lrt)

# Rules to install ops-vrfmgrd binary in rootfs
install(TARGETS ${VRFMGRD}
    RUNTIME DESTINATION bin)
