include_directories (${CMOCKA_INCLUDE_DIR})

add_executable (first_test first_test.c)
target_link_libraries (first_test ${CMOCKA_LIBRARIES})

add_test (first_test first_test)
