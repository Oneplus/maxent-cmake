cmake_minimum_required (VERSION 2.8)

project (maxent-cmake)

set (CMAKE_MODULE_PATH      ${PROJECT_SOURCE_DIR}/cmake)
set (EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set (LIBRARY_OUTPUT_PATH    ${PROJECT_SOURCE_DIR}/lib)
set (INCLUDE_OUTPUT_DIR     ${PROJECT_SOURCE_DIR}/include)
set (SOURCE_DIR             ${PROJECT_SOURCE_DIR}/src)

# checking section
include (CheckFunctionExists)
include (CheckIncludeFile)
include (CheckIncludeFileCXX)
include (CheckIncludeFiles)
include (CheckLibraryExists)
include (CheckSymbolExists)
include (CheckTypeSize)

# -----------------------------------------------
# checking section
# -----------------------------------------------
include (CMakeDetermineFortranCompiler)

if (CMAKE_Fortran_COMPILER)
    set (HAVE_FORTRAN   1)
endif (CMAKE_Fortran_COMPILER)

set (LINK_LIBS "")
macro (CHECK_LIBRARY_EXISTS_CONCAT LIBRARY SYMBOL VARIABLE)
    check_library_exists ("${LIBRARY};${LINK_LIBS}"
        ${SYMBOL}
        ""
        ${VARIABLE})

    if (${VARIABLE})
        set (LINK_LIBS ${LINK_LIBS} ${LIBRARY})
    endif (${VARIABLE})
endmacro (CHECK_LIBRARY_EXISTS_CONCAT)

set (USE_INCLUDES "")
macro (CHECK_INCLUDE_FILE_CONCAT FILE VARIABLE)
    check_include_files ("${USE_INCLUDES};${FILE}"
        ${VARIABLE})

    if (${VARIABLE})
        set (USE_INCLUDES ${USE_INCLUDES} ${FILE})
    endif (${VARIABLE})
endmacro (CHECK_INCLUDE_FILE_CONCAT)

check_function_exists (getopt_long  HAVE_GETOPT_LONG)
check_function_exists (getpagesize  HAVE_GETPAGESIZE)
check_function_exists (gettimeofday HAVE_GETTIMEOFDAY)

check_include_file_concat ("ieeefp.h"       HAVE_IEEEFP_H)
check_include_file_concat ("inttypes.h"     HAVE_INTTYPES_H)
check_include_file_concat ("memory.h"       HAVE_MEMORY_H)
check_include_file_concat ("stdint.h"       HAVE_STDINT_H)
check_include_file_concat ("stdlib.h"       HAVE_STDLIB_H)
check_include_file_concat ("strings.h"      HAVE_STRINGS_H)
check_include_file_concat ("string.h"       HAVE_STRING_H)
check_include_file_concat ("sys/stat.h"     HAVE_SYS_STAT_H)
check_include_file_concat ("sys/types.h"    HAVE_SYS_TYPES_H)
check_include_file_concat ("unistd.h"       HAVE_UNISTD_H)

check_library_exists_concat ("gnugetopt"    getopt      HAVE_LIBGNUGETOPT)
check_library_exists_concat ("z"            deflateEnd  HAVE_LIBZ)
if (NOT WINDOWS)
    check_library_exists_concat ("m"        random      HAVE_LIBM)
endif (NOT WINDOWS)

# -----------------------------------------------
# configure section
# -----------------------------------------------

configure_file (${CMAKE_CURRENT_SOURCE_DIR}/config.h.in
    ${CMAKE_CURRENT_SOURCE_DIR}/config.h)

configure_file (${CMAKE_CURRENT_SOURCE_DIR}/python/setup.py.in
    ${CMAKE_CURRENT_SOURCE_DIR}/python/setup.py)

# -----------------------------------------------
# building section
# -----------------------------------------------

add_subdirectory (src)

if (CMAKE_BUILD_TYPE STREQUAL "Test")
    add_subdirectory (test)
endif (CMAKE_BUILD_TYPE STREQUAL "Test")

