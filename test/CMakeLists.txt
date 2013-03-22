include_directories ( ./ ${INCLUDE_OUTPUT_PATH} ${SOURCE_DIR} )

set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${EXECUTABLE_OUTPUT_PATH}/test)

add_executable (test_display    test_display.cpp)
add_executable (test_event      test_event.cpp)
add_executable (test_itemmap    test_itemmap.cpp)
add_executable (test_gistrainer test_gistrainer.cpp)
add_executable (test_modelfile  test_modelfile.cpp)
add_executable (test_mmapfile   test_mmapfile.cpp)

target_link_libraries (test_display     maxent)
target_link_libraries (test_event       maxent)
target_link_libraries (test_itemmap     maxent)
target_link_libraries (test_gistrainer  maxent)
target_link_libraries (test_modelfile   maxent)
target_link_libraries (test_mmapfile    maxent)

enable_testing ()

add_test (test_display      ${EXECUTABLE_OUTPUT_PATH}/test/test_display)
add_test (test_event        ${EXECUTABLE_OUTPUT_PATH}/test/test_event)
add_test (test_itemmap      ${EXECUTABLE_OUTPUT_PATH}/test/test_itemmap)
add_test (test_gistrainer   ${EXECUTABLE_OUTPUT_PATH}/test/test_gistrainer)
add_test (test_modelfile    ${EXECUTABLE_OUTPUT_PATH}/test/test_modelfile)
add_test (test_mmapfile     ${EXECUTABLE_OUTPUT_PATH}/test/test_mmapfile)
