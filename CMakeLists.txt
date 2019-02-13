cmake_minimum_required (VERSION 3.13)
project (basics C)

set (CMAKE_C_STANDARD 11)

include (GNUInstallDirs)

include_directories ()

## Documentation.
# Require dot, treat the other components as optional
find_package (Doxygen
              REQUIRED dot
              OPTIONAL_COMPONENTS mscgen dia)

if (DOXYGEN_FOUND)
  set (DOXYGEN_GENERATE_HTML YES)
  set (DOXYGEN_GENERATE_MAN YES)
  set (DOXYGEN_OPTIMIZE_OUTPUT_FOR_C YES)
  set (DOXYGEN_QT_AUTOBRIEF YES)
  set (DOXYGEN_EXTRACT_ALL YES)
  set (DOXYGEN_EXCLUDE_PATTERNS */cmake-* */cmake-*/*)
  set (DOXYGEN_QUIET YES)

  doxygen_add_docs (
      docs
      ${PROJECT_SOURCE_DIR}
      COMMENT "Building documentation using doxygen."
  )

else (DOXYGEN_FOUND)
    message ("Doxygen need to be installed to generate the doxygen documentation")
endif (DOXYGEN_FOUND)

## Unit testing.
set (ENABLE_TESTS 1 CACHE BOOL "Enable unit tests.")
if (ENABLE_TESTS)
  find_package (CMOCKA)

  if (CMOCKA_FOUND)
    message (STATUS "CMocka found, tests are enabled.")
    enable_testing ()
    add_subdirectory (tests)

  else (CMOCKA_FOUND)
    message (WARNING "CMocka not found, tests are disabled.")
    set (ENABLE_TESTS false)

  endif (CMOCKA_FOUND)
endif (ENABLE_TESTS)

add_executable (basics main.c)