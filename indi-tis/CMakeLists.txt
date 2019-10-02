cmake_minimum_required(VERSION 2.4.7)
PROJECT(tis_ccd CXX C)

LIST(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake_modules/")
LIST(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../cmake_modules/")

include(GNUInstallDirs)

set (TIS_VERSION_MAJOR 1)
set (TIS_VERSION_MINOR 0)

find_package(GStreamer REQUIRED QUIET)
find_package(CFITSIO REQUIRED)
find_package(INDI REQUIRED)
find_package(ZLIB REQUIRED)
find_package(GLIB2 REQUIRED QUIET)
find_package(GObject REQUIRED QUIET)
find_package(GObjectIntrospection REQUIRED QUIET)

find_package(PkgConfig REQUIRED)
pkg_check_modules(TCAMLIB tcam)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/config.h.cmake ${CMAKE_CURRENT_BINARY_DIR}/config.h )
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/indi_tis_ccd.xml.cmake ${CMAKE_CURRENT_BINARY_DIR}/indi_tis_ccd.xml )

include_directories( ${CMAKE_CURRENT_BINARY_DIR})
include_directories( ${CMAKE_CURRENT_SOURCE_DIR})
include_directories( ${INDI_INCLUDE_DIR})
include_directories( ${CFITSIO_INCLUDE_DIR})
include_directories( ${GSTREAMER_INCLUDE_DIRS})
include_directories( ${GSTREAMER_BASE_INCLUDE_DIRS})
include_directories( ${GSTREAMER_VIDEO_INCLUDE_DIRS})
include_directories( ${GLIB2_INCLUDE_DIR})
include_directories( ${GObject_INCLUDE_DIR})
include_directories( ${TCAM_INCLUDE_DIRS})

include(CMakeCommon)

############# TIS CCD ###############
set(tisccd_SRCS
	${CMAKE_CURRENT_SOURCE_DIR}/tis_ccd.cpp
)

add_executable(indi_tis_ccd ${tisccd_SRCS})

target_link_libraries(indi_tis_ccd ${INDI_LIBRARIES} ${CFITSIO_LIBRARIES} m ${ZLIB_LIBRARY} ${GSTREAMER_LIBRARIES} ${GSTREAMER_BASE_LIBRARIES} ${GSTREAMER_VIDEO_LIBRARIES} ${GOBJECT_LIBRARIES} ${GLIB2_LIBRARIES} ${TCAMLIB_LIBRARIES})

install(TARGETS indi_tis_ccd RUNTIME DESTINATION bin)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/indi_tis_ccd.xml DESTINATION ${INDI_DATA_DIR})
