cmake_minimum_required(VERSION 3.11)
project(luafilesystem)

find_path(LUA_INCLUDE_DIR lua.h PATH_SUFFIXES lua)
find_library(LUA_LIBRARY lua)
set(LFS_INCLUDES ${LUA_INCLUDE_DIR})
set(LFS_LIBRARIES ${LUA_LIBRARY})

add_library(lfs src/lfs.h src/lfs.c src/lfs.def)

target_include_directories(lfs PRIVATE ${LFS_INCLUDES})
target_link_libraries(lfs PRIVATE ${LFS_LIBRARIES})

install(TARGETS lfs
    EXPORT lfsTargets
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib)

include(CMakePackageConfigHelpers)

configure_package_config_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_NAME}Config.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
  INSTALL_DESTINATION "share/${PROJECT_NAME}"
)

set(VERSION_FILE_PATH "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake")
write_basic_package_version_file(
  "${VERSION_FILE_PATH}"
  VERSION       "${LFS_VERSION}"
  COMPATIBILITY SameMajorVersion
)

install(
  FILES "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
  DESTINATION "share/${PROJECT_NAME}"
)

install(
  EXPORT      "lfsTargets"
  NAMESPACE   "${PROJECT_NAME}::"
  DESTINATION "share/${PROJECT_NAME}")
