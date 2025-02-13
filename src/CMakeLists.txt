# Find required Qt5 components
find_package(Qt5 REQUIRED COMPONENTS
  Core
  Gui
  Widgets
  PrintSupport
)

# Enable AUTOMOC
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

# Add source files
set(SOURCES
  ${CMAKE_CURRENT_SOURCE_DIR}/qcustomplot.cpp
)

# Add header files
set(HEADERS
  ${CMAKE_CURRENT_SOURCE_DIR}/qcustomplot.h
)

# Set MSVC compiler optimization for speed in release mode
if (MSVC)
  set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /O2")
endif()

# Create static library for debug mode
add_library(QCustomPlotStaticLib STATIC ${SOURCES} ${HEADERS})
set_target_properties(QCustomPlotStaticLib PROPERTIES 
  OUTPUT_NAME "QCustomPlot"
  DEBUG_POSTFIX "d"
)

target_compile_definitions(QCustomPlotStaticLib PRIVATE QCUSTOMPLOT_COMPILE_LIBRARY)

# Create shared library for release mode
add_library(QCustomPlotSharedLib SHARED ${SOURCES} ${HEADERS})
set_target_properties(QCustomPlotSharedLib PROPERTIES 
  OUTPUT_NAME "QCustomPlot"
  DEBUG_POSTFIX "d"
 # WINDOWS_EXPORT_ALL_SYMBOLS ON
)

target_compile_definitions(QCustomPlotSharedLib PRIVATE QCUSTOMPLOT_COMPILE_LIBRARY)

# Set debug and release output directories for binaries
set_target_properties(QCustomPlotStaticLib PROPERTIES
  ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${CMAKE_SOURCE_DIR}/build/debug
  LIBRARY_OUTPUT_DIRECTORY_DEBUG ${CMAKE_SOURCE_DIR}/build/debug
  RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_SOURCE_DIR}/build/debug
  ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${CMAKE_SOURCE_DIR}/build/release
  LIBRARY_OUTPUT_DIRECTORY_RELEASE ${CMAKE_SOURCE_DIR}/build/release
  RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_SOURCE_DIR}/build/release
)

set_target_properties(QCustomPlotSharedLib PROPERTIES
  ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${CMAKE_SOURCE_DIR}/build/debug
  LIBRARY_OUTPUT_DIRECTORY_DEBUG ${CMAKE_SOURCE_DIR}/build/debug
  RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_SOURCE_DIR}/build/debug
  ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${CMAKE_SOURCE_DIR}/build/release
  LIBRARY_OUTPUT_DIRECTORY_RELEASE ${CMAKE_SOURCE_DIR}/build/release
  RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_SOURCE_DIR}/build/release
)

# Include directories
target_include_directories(QCustomPlotStaticLib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_include_directories(QCustomPlotSharedLib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

# Link Qt libraries
target_link_libraries(QCustomPlotStaticLib PRIVATE
  Qt5::Core
  Qt5::Gui
  Qt5::Widgets
  Qt5::PrintSupport
)

target_link_libraries(QCustomPlotSharedLib PRIVATE
  Qt5::Core
  Qt5::Gui
  Qt5::Widgets
  Qt5::PrintSupport
)

### installing the library ####

# Set installation directory
if (NOT DEFINED INSTALLATION_DIR OR INSTALLATION_DIR STREQUAL "")
  if (CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(INSTALLATION_DIR ${CMAKE_SOURCE_DIR}/installation/debug)
  else()
    set(INSTALLATION_DIR ${CMAKE_SOURCE_DIR}/installation/release)
  endif()
endif()

# Install targets
install(TARGETS QCustomPlotStaticLib QCustomPlotSharedLib
  ARCHIVE DESTINATION ${INSTALLATION_DIR}
  LIBRARY DESTINATION ${INSTALLATION_DIR}
  RUNTIME DESTINATION ${INSTALLATION_DIR}
)
