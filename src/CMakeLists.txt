cmake_minimum_required (VERSION 3.8)

if (NOT VCPKG_TOOLCHAIN)
  find_package(PkgConfig)
  pkg_check_modules(GTKMM_LIB gtkmm-4.0)
  include_directories(${GTKMM_INCLUDE_DIRS})
  link_directories(${GTKMM_LIBRARY_DIRS})
  link_libraries(${GTKMM_LIBRARIES})
else()
  find_package(PkgConfig REQUIRED)
  message(${PKG_CONFIG_FOUND})
  pkg_check_modules(GTKMM_LIB REQUIRED IMPORTED_TARGET gtkmm-4.0)
endif()

if (MSVC)
  set(GUI_OPTION WIN32)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /wd4250 /wd4275 /wd4819 /wd26812")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /ENTRY:mainCRTStartup")
endif()

add_executable (GTKMM-4.0VcpkgCmakeExample ${GUI_OPTION} "app.cpp" "app.h")

target_link_libraries(GTKMM-4.0VcpkgCmakeExample PRIVATE PkgConfig::GTKMM_LIB)
target_compile_features(GTKMM-4.0VcpkgCmakeExample PRIVATE cxx_std_17)