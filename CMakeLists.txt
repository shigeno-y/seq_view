cmake_minimum_required(VERSION 3.23)
project(seq_view)

add_subdirectory(seq_view)

option(SEQVIEW_BUILD_TEST "build test" OFF)
if(SEQVIEW_BUILD_TEST)
    add_subdirectory(test_project)
endif()

if(MSVC AND TARGET test_project)
    set_property(DIRECTORY PROPERTY VS_STARTUP_PROJECT test_project)
endif()

include(CMakePackageConfigHelpers)
install(
    EXPORT "${PROJECT_NAME}Targets"
    NAMESPACE ${PROJECT_NAME}::
    FILE "${PROJECT_NAME}Targets.cmake"
    DESTINATION share/cmake/${PROJECT_NAME})

# generate the config file that includes the exports
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/Config.in.cmake
    "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
    INSTALL_DESTINATION "share/cmake/${PROJECT_NAME}"
    NO_SET_AND_CHECK_MACRO NO_CHECK_REQUIRED_COMPONENTS_MACRO)
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
    VERSION
        "${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.${PROJECT_VERSION_PATCH}"
    COMPATIBILITY AnyNewerVersion)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
              ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
        DESTINATION share/cmake/${PROJECT_NAME})
