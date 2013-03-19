INCLUDE_DIRECTORIES( ./ )

SET(OPT_FILES "")

IF (CMAKE_Fortran_COMPILER)
    SET(OPT_FILES lbfgs.f lbfgstrainer.cpp)
ELSE ()
    MESSAGE(STATUS "[Warning] Fortran compiler not available,  LBFGS module will not be built.")
ENDIF()

# --- Build libmaxent ---

ADD_LIBRARY(maxent
    display.cpp
    modelfile.cpp
    trainer.cpp
    gistrainer.cpp
    maxentmodel.cpp
    mmapfile.c
    lbfgs_wrapper.c
    ${OPT_FILES})

SET (GETOPT_FILES "")

IF (WIN32 OR MINGW)
    SET (GETOPT_FILES getopt.c getopt1.c)
ENDIF ()

ADD_EXECUTABLE(maxent-main
    maxent.cpp
    display.cpp
    maxent_cmdline.c
    f77_dummy_main.cpp
    ${GETOPT_FILES})

TARGET_LINK_LIBRARIES(maxent-main maxent)

# --- Config .hpp files ---

CONFIGURE_FILE(maxentmodel.hpp  ${INCLUDE_DIR}/maxentmodel.hpp      @ONLY)
CONFIGURE_FILE(itemmap.hpp      ${INCLUDE_DIR}/itemmap.hpp          @ONLY)
CONFIGURE_FILE(meevent.hpp      ${INCLUDE_DIR}/ext_algorithm.hpp    @ONLY)
CONFIGURE_FILE(itemmap.tcc      ${INCLUDE_DIR}/itemmap.tcc          @ONLY)
CONFIGURE_FILE(eventspace.hpp   ${INCLUDE_DIR}/eventspace.hpp       @ONLY)
CONFIGURE_FILE(eventspace.tcc   ${INCLUDE_DIR}/eventspace.tcc       @ONLY)
