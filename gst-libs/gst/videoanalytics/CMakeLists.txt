# ==============================================================================
# Copyright (C) <2018-2019> Intel Corporation
#
# SPDX-License-Identifier: MIT
# ==============================================================================

cmake_minimum_required(VERSION 3.1)

set (TARGET_NAME "gstvideoanalyticsmeta")

find_package(PkgConfig REQUIRED)

pkg_check_modules(GSTREAMER gstreamer-1.0 REQUIRED)
pkg_check_modules(GLIB2 glib-2.0 REQUIRED)

file (GLOB MAIN_SRC
        ${CMAKE_CURRENT_SOURCE_DIR}/*.c
        ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp
        )

file (GLOB MAIN_HEADERS
        ${CMAKE_CURRENT_SOURCE_DIR}/*.h
        )

add_library(${TARGET_NAME} SHARED ${MAIN_SRC} ${MAIN_HEADERS})
set_target_lib_version(${TARGET_NAME})
set_compile_flags(${TARGET_NAME})

target_include_directories(${TARGET_NAME}
PUBLIC
        ${GSTREAMER_INCLUDE_DIRS}
        ${GLIB2_INCLUDE_DIRS}
        ${CMAKE_CURRENT_SOURCE_DIR}
)

target_link_libraries(${TARGET_NAME}
PUBLIC
        ${GSTREAMER_LIBRARIES}
        ${GSTVIDEO_LIBRARIES}
        ${GLIB2_LIBRARIES}
)


# TODO: perhaps we need to create a function
if(${ENABLE_DOCUMENTATION})
        include(${CMAKE_CURRENT_SOURCE_DIR}/CMakeDocs.txt)
endif()

install(TARGETS ${TARGET_NAME} DESTINATION lib/va-gstreamer-plugins)
