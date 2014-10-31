cmake_minimum_required(VERSION 2.8.12 FATAL_ERROR)
project(CMakeToolbox NONE)

set(CTB_MAJOR_VERSION 1)
set(CTB_MINOR_VERSION 0)
set(CTB_PATCH_VERSION 0)
set(CTB_VERSION ${CTB_MAJOR_VERSION}.${CTB_MINOR_VERSION}.${CTB_PATCH_VERSION})

# - Core install paths
set(CMAKE_INSTALL_SHAREDIR share)
set(CMAKECONFIG_INSTALL_DIR ${CMAKE_INSTALL_SHAREDIR}/CMakeToolbox-${CBT_VERSION}/cmake)
set(MODULES_INSTALL_DIR     ${CMAKE_INSTALL_SHAREDIR}/CMakeToolbox-${CBT_VERSION}/modules)
set(CET_MODULES_INSTALL_DIR ${CMAKE_INSTALL_SHAREDIR}/CMakeToolbox-${CBT_VERSION}/cet-modules)
set(FIND_MODULES_INSTALL_DIR ${CMAKE_INSTALL_SHAREDIR}/CMakeToolbox-${CBT_VERSION}/find-modules)

# - Select and install Modules
# FNAL CET Modules
set(CET_MODULES
  cet-modules/cet_test_functions.sh
  cet-modules/CetTest.cmake
  cet-modules/CetRegexEscape.cmake
  )
install(FILES ${CET_MODULES}
  DESTINATION ${CET_MODULES_INSTALL_DIR}
  )

# FindXXX modules
file(GLOB FIND_MODULES ${PROJECT_SOURCE_DIR}/find-modules/*[^~])
install(FILES ${FIND_MODULES}
  DESTINATION ${FIND_MODULES_INSTALL_DIR}
  )

# - Package helpers
include(CMakePackageConfigHelpers)

configure_package_config_file(
  CMakeToolboxConfig.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/CMakeToolboxConfig.cmake
  INSTALL_DESTINATION ${CMAKECONFIG_INSTALL_DIR}
  PATH_VARS
   MODULES_INSTALL_DIR
   CET_MODULES_INSTALL_DIR
   FIND_MODULES_INSTALL_DIR
  )

write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/CMakeToolboxConfigVersion.cmake
  VERSION ${CTB_VERSION}
  COMPATIBILITY AnyNewerVersion
 )

install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/CMakeToolboxConfig.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/CMakeToolboxConfigVersion.cmake
  DESTINATION ${CMAKECONFIG_INSTALL_DIR}
  )

# Configure packaging
set(CPACK_PACKAGE_NAME CMakeToolbox)
set(CPACK_PACKAGE_VERSION ${CTB_VERSION})
set(CPACK_SYSTEM_NAME "generic")
set(CPACK_GENERATOR "STGZ;TGZ")
set(CPACK_SET_DESTDIR FALSE)
include(CPack)

