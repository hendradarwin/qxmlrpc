
CMAKE_MINIMUM_REQUIRED(VERSION 2.8.2)

PROJECT(qxmlrpc)

INCLUDE(CTest)

OPTION(qxmlrpc_BUILD_EXAMPLES "Build examples" OFF)


#-----------------------------------------------------------------------------
# Output directories.
#
SET(output_dir ${qxmlrpc_BINARY_DIR}/bin)
FOREACH(type LIBRARY RUNTIME ARCHIVE)
  IF(NOT DEFINED CMAKE_${type}_OUTPUT_DIRECTORY)
    SET(CMAKE_${type}_OUTPUT_DIRECTORY ${output_dir} CACHE INTERNAL "Single output directory for building all libraries.")
  ENDIF()
ENDFOREACH()

#-----------------------------------------------------------------------------
# Qt4
#-----------------------------------------------------------------------------
#FIND_PACKAGE(Qt5 4.6 REQUIRED)
#SET(QT_USE_QTNETWORK ON)
#SET(QT_USE_QTXML ON)
#INCLUDE(${QT_USE_FILE})


#-----------------------------------------------------------------------------
# Qt5
#-----------------------------------------------------------------------------
set(QXMLRPC_QT5_COMPONENTS Core Xml XmlPatterns Network)
find_package(Qt5 COMPONENTS ${QXMLRPC_QT5_COMPONENTS} REQUIRED)



#-----------------------------------------------------------------------------
# Subdirectories
#

INCLUDE_DIRECTORIES(
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${CMAKE_CURRENT_BINARY_DIR}
  )
  
include_directories(${Qt5Widgets_INCLUDE_DIRS})
add_definitions(${Qt5Widgets_DEFINITIONS})
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${Qt5Widgets_EXECUTABLE_COMPILE_FLAGS}")  
  
  
ADD_SUBDIRECTORY(xmlrpc)



IF(BUILD_TESTING)
#  ADD_SUBDIRECTORY(test)
ENDIF()

IF(qxmlrpc_BUILD_EXAMPLES)
#  ADD_SUBDIRECTORY(examples)
ENDIF()

#-----------------------------------------------------------------------------
# Generate qxmlrpc file
#
CONFIGURE_FILE(Useqxmlrpc.cmake.in
               ${qxmlrpc_BINARY_DIR}/Useqxmlrpc.cmake COPYONLY)

INCLUDE(GenerateqxmlrpcConfig.cmake)

