cmake_minimum_required(VERSION 3.25)

project(staggering-test-startup)

include(CTest)

set(stagger_seconds 0.2)
function(stagger_test_start testname)
    add_test(
        NAME "stagger:${testname}"
        COMMAND ${CMAKE_COMMAND} -E sleep ${stagger_seconds}
    )
    set_property(
        TEST "stagger:${testname}"
        PROPERTY FIXTURES_SETUP "staggerstartup:${testname}"
    )
    set_property(
        TEST "stagger:${testname}"
        PROPERTY RESOURCE_LOCK stagger_synchronization
    )

    set_property(
        TEST "${testname}"
        APPEND PROPERTY FIXTURES_REQUIRED "staggerstartup:${testname}"
    )
endfunction()

add_test(NAME test-1 COMMAND bash "${CMAKE_CURRENT_SOURCE_DIR}/slow-test.sh" 1)
add_test(NAME test-2 COMMAND bash "${CMAKE_CURRENT_SOURCE_DIR}/slow-test.sh" 2)
add_test(NAME test-3 COMMAND bash "${CMAKE_CURRENT_SOURCE_DIR}/slow-test.sh" 3)
add_test(NAME test-4 COMMAND bash "${CMAKE_CURRENT_SOURCE_DIR}/slow-test.sh" 4)
add_test(NAME test-5 COMMAND bash "${CMAKE_CURRENT_SOURCE_DIR}/slow-test.sh" 5)
add_test(NAME test-6 COMMAND bash "${CMAKE_CURRENT_SOURCE_DIR}/slow-test.sh" 6)
add_test(NAME test-7 COMMAND bash "${CMAKE_CURRENT_SOURCE_DIR}/slow-test.sh" 7)
add_test(NAME test-8 COMMAND bash "${CMAKE_CURRENT_SOURCE_DIR}/slow-test.sh" 8)
add_test(NAME test-9 COMMAND bash "${CMAKE_CURRENT_SOURCE_DIR}/slow-test.sh" 9)
add_test(NAME test-10 COMMAND bash "${CMAKE_CURRENT_SOURCE_DIR}/slow-test.sh" 10)

option(STAGGER_TESTS "Stagger test starts" ON)
if(STAGGER_TESTS)
    stagger_test_start(test-1)
    stagger_test_start(test-2)
    stagger_test_start(test-3)
    stagger_test_start(test-4)
    stagger_test_start(test-5)
    stagger_test_start(test-6)
    stagger_test_start(test-7)
    stagger_test_start(test-8)
    stagger_test_start(test-9)
    stagger_test_start(test-10)
endif()
