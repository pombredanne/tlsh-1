# Copyright 2013 Trend Micro Incorporated
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

cmake_minimum_required(VERSION 2.6)

project(TLSH)

set(VERSION_MAJOR 3)
set(VERSION_MINOR 0)
set(VERSION_PATCH 4)

set(TLSH_SHORT 1)
if(TLSH_SHORT EQUAL 1)
  add_definitions(-DVERSION_70B)
  set(TLSH_HASH "compact hash")
else()
  set(TLSH_HASH "full hash")
endif ()

# write a file with the VERSION information
file(REMOVE VERSION)
file(WRITE VERSION 
  "TLSH version: ${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH} ${TLSH_HASH}\n")

if (CMAKE_BUILD_TYPE STREQUAL Debug)
    set(BUILD_POSTFIX "_d")
else(CMAKE_BUILD_TYPE STREQUAL Debug)
    set(BUILD_POSTFIX "")
    if(CMAKE_COMPILER_IS_GNUCXX)
        set(CMAKE_CXX_FLAGS "-O2")        ## Optimize
    endif()
    if(MSVC)
        set(CMAKE_CXX_FLAGS "/O2")        ## Optimize
    endif()
endif(CMAKE_BUILD_TYPE STREQUAL Debug)

enable_testing()

include_directories(include)
add_subdirectory(src)
add_subdirectory(test)
add_subdirectory(Testing)

set(CPACK_GENERATOR "TGZ")
set(CPACK_SOURCE_GENERATOR "TGZ")
set(CPACK_SOURCE_PACKAGE_FILE_NAME "TLSH-${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH}")
set(CPACK_PACKAGE_VERSION_MAJOR "${VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${VERSION_PATCH}")
set(CPACK_SOURCE_IGNORE_FILES
"~$"
"^${PROJECT_SOURCE_DIR}/bin/"
"^${PROJECT_SOURCE_DIR}/lib/"
"^${PROJECT_SOURCE_DIR}/build/"
"^${PROJECT_SOURCE_DIR}/py_ext/data/"
"^${PROJECT_SOURCE_DIR}/py_ext/build/"
"^${PROJECT_SOURCE_DIR}/Testing/tmp/"
"^${PROJECT_SOURCE_DIR}/CMakeFiles/"
)
include(CPack)
