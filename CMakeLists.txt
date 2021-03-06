cmake_minimum_required(VERSION 3.14.7)
set(CMAKE_CXX_STANDARD 17)

project(libadder
        VERSION 2.0.8
        LANGUAGES CXX
        )

include(cmake/AddSimpleLibrary.cmake)
add_subdirectory(include)
add_subdirectory(lib)
add_subdirectory(test)

# Static library
add_library(libadder_static STATIC)

target_link_libraries(libadder_static
        PUBLIC
            libadder_include_libadder
        PRIVATE
            libadder_lib
        )

set_target_properties(libadder_static PROPERTIES
        PREFIX ""
        OUTPUT_NAME libadder_static_$<CONFIG>
        OUTPUT_NAME_RELEASE libadder_static
        SUFFIX ${CMAKE_STATIC_LIBRARY_SUFFIX}.${PROJECT_VERSION}
        )

# Shared library
add_library(libadder SHARED)

target_link_libraries(libadder
        PUBLIC
            libadder_include_libadder
        PRIVATE
            libadder_lib
        )

set_target_properties(libadder PROPERTIES
        PREFIX ""
        OUTPUT_NAME libadder_$<CONFIG>
        OUTPUT_NAME_RELEASE libadder
        SUFFIX ${CMAKE_SHARED_LIBRARY_SUFFIX}.${PROJECT_VERSION}
        )

# Installation
set(LIBADDER_CMAKE_INSTALL_DIR lib/cmake/LibAdder-${PROJECT_VERSION})

install(TARGETS
            libadder
            libadder_static
            libadder_lib
            libadder_include_libadder
        EXPORT libadder_export
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib/static
        )

install(EXPORT libadder_export
        FILE
            LibAdderTargets.cmake
        NAMESPACE
            LibAdder::
        DESTINATION
            ${LIBADDER_CMAKE_INSTALL_DIR}
        )

include(CMakePackageConfigHelpers)
write_basic_package_version_file(${CMAKE_CURRENT_BINARY_DIR}/LibAdderConfigVersion.cmake
        VERSION ${PROJECT_VERSION}
        COMPATIBILITY AnyNewerVersion
        )

configure_package_config_file(cmake/LibAdderConfig.cmake.in
        ${CMAKE_CURRENT_BINARY_DIR}/LibAdderConfig.cmake
        INSTALL_DESTINATION ${LIBADDER_CMAKE_INSTALL_DIR}
        )

install(FILES
        ${CMAKE_CURRENT_BINARY_DIR}/LibAdderConfig.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/LibAdderConfigVersion.cmake
        DESTINATION ${LIBADDER_CMAKE_INSTALL_DIR}
        )
