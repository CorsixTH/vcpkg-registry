cmake_minimum_required(VERSION 3.11)
project(lpeg)

find_path(LUA_INCLUDE_DIR lua.h PATH_SUFFIXES lua)
find_library(LUA_LIBRARY lua)
set(LPEG_INCLUDES ${LUA_INCLUDE_DIR})
set(LPEG_LIBRARIES ${LUA_LIBRARY})

add_library(lpeg
    lpvm.c
    lptree.c
    lpprint.c
    lpcap.c
    lpcode.c
    lpcset.c
    lpeg.def)

target_include_directories(lpeg PRIVATE ${LPEG_INCLUDES})
target_link_libraries(lpeg PRIVATE ${LPEG_LIBRARIES})

install(TARGETS lpeg
    EXPORT lpegTargets
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib)
install(FILES re.lua DESTINATION share/lua)

include(CMakePackageConfigHelpers)

configure_package_config_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_NAME}Config.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
  INSTALL_DESTINATION "share/${PROJECT_NAME}"
)

set(VERSION_FILE_PATH "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake")
write_basic_package_version_file(
  "${VERSION_FILE_PATH}"
  VERSION       "${LPEG_VERSION}"
  COMPATIBILITY SameMajorVersion
)

install(
  FILES "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
  DESTINATION "share/${PROJECT_NAME}"
)

install(
  EXPORT      "${PROJECT_NAME}Targets"
  NAMESPACE   "${PROJECT_NAME}::"
  DESTINATION "share/${PROJECT_NAME}")
