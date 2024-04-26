cmake_minimum_required(VERSION 3.0)
project(luafilesystem)

find_path(LUA_INCLUDE_DIR lua.h PATH_SUFFIXES lua)
find_library(LUA_LIBRARY lua)
set(LFS_INCLUDES ${LUA_INCLUDE_DIR})
set(LFS_LIBRARIES ${LUA_LIBRARY})

add_library(lfs src/lfs.h src/lfs.c src/lfs.def)

target_include_directories(lfs PRIVATE ${LFS_INCLUDES})
target_link_libraries(lfs PRIVATE ${LFS_LIBRARIES})

install(TARGETS lfs
    EXPORT lfs-targets
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib)

  include(CMakePackageConfigHelpers)
configure_package_config_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/luafilesystem-config.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/luafileystem-config.cmake"
  INSTALL_DESTINATION "share/luafilesystem"
)

install(
  FILES "${CMAKE_CURRENT_BINARY_DIR}/luafilesystem-config.cmake"
  DESTINATION "share/luafilesystem"
)

install(
    EXPORT lfs-targets
    DESTINATION share/luafilesystem
    NAMESPACE luafilesystem::
)
