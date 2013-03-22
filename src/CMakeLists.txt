include_directories ( ./ ../lib/ )

set (OPT_FILES "")

if (CMAKE_Fortran_COMPILER)
    set (OPT_FILES lbfgs.f lbfgstrainer.cpp)
else (CMAKE_Fortran_COMPILER)
    message (STATUS
        "[Warning] Fortran compiler not available, "
        "LBFGS module will not be built.")
endif (CMAKE_Fortran_COMPILER)

# --- Build libmaxent ---

add_library (maxent
    display.cpp
    modelfile.cpp
    trainer.cpp
    gistrainer.cpp
    maxentmodel.cpp
    mmapfile.c
    lbfgs_wrapper.c
    ${OPT_FILES})

set (GETOPT_FILES "")

if (WIN32 OR MINGW)
    set (GETOPT_FILES getopt.c getopt1.c)
endif ()

add_executable (maxent-main
    maxent.cpp
    display.cpp
    maxent_cmdline.c
    f77_dummy_main.cpp
    ${GETOPT_FILES})

target_link_libraries (maxent-main maxent)

# --- Config .hpp files ---

configure_file (maxentmodel.hpp
    ${INCLUDE_OUTPUT_DIR}/maxentmodel.hpp
    @ONLY)

configure_file (itemmap.hpp
    ${INCLUDE_OUTPUT_DIR}/itemmap.hpp
    @ONLY)

configure_file (meevent.hpp
    ${INCLUDE_OUTPUT_DIR}/ext_algorithm.hpp
    @ONLY)

configure_file (itemmap.tcc
    ${INCLUDE_OUTPUT_DIR}/itemmap.tcc
    @ONLY)

configure_file (eventspace.hpp   
    ${INCLUDE_OUTPUT_DIR}/eventspace.hpp
    @ONLY)

configure_file (eventspace.tcc
    ${INCLUDE_OUTPUT_DIR}/eventspace.tcc
    @ONLY)
