add_subdirectory(googletest)

add_executable(libadder_test
        AdderTs.cpp
        )

target_link_libraries(libadder_test
        PRIVATE
            libadder_include_libadder
            libadder_static
            gtest_main
        )

add_test(NAME libadder_test
        COMMAND libadder_test
        )