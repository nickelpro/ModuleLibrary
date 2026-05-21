cmake_minimum_required(VERSION 4.3)
project(ModuleLibrary VERSION 1.0.0)

add_library(ModuleLibrary)
target_compile_features(ModuleLibrary PRIVATE cxx_std_23)

target_sources(ModuleLibrary
  PRIVATE
    FILE_SET SOURCES
    FILES
      src/HelloWorld.cpp

  PUBLIC
    FILE_SET CXX_MODULES
    BASE_DIRS
      src
    FILES
      src/HelloWorld.cppm
      src/ModuleLibrary.cppm
)

add_executable(TestModuleLibrary)
target_compile_features(TestModuleLibrary PRIVATE cxx_std_23)

target_sources(TestModuleLibrary
  PRIVATE
    FILE_SET SOURCES
    FILES
      src/Test.cpp
)
target_link_libraries(TestModuleLibrary PRIVATE ModuleLibrary)

enable_testing()
add_test(TestModuleLibrary TestModuleLibrary)


include(GNUInstallDirs)

install(
  TARGETS ModuleLibrary
  EXPORT ModuleLibrary-targets
  FILE_SET CXX_MODULES
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/ModuleLibrary
)

install(
  EXPORT ModuleLibrary-targets
  NAMESPACE ModuleLibrary::
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ModuleLibrary
  FILE ModuleLibraryConfig.cmake
  CXX_MODULES_DIRECTORY cxxmodules
)

install(
  PACKAGE_INFO ModuleLibrary
  EXPORT ModuleLibrary-targets
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cps/ModuleLibrary
  CXX_MODULES_DIRECTORY cxxmodules
)
