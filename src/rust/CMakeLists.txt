if (CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(CARGO_CMD cargo build)
    set(TARGET_DIR "debug")
else ()
    set(CARGO_CMD cargo build --release)
    set(TARGET_DIR "release")
endif ()

set(CCX_RUST_SO "${CMAKE_CURRENT_BINARY_DIR}/${TARGET_DIR}/libccx_rust.so")

add_custom_target(ccx_rust ALL
    COMMENT "Compiling ccx_rust module"
    COMMAND CARGO_TARGET_DIR=${CMAKE_CURRENT_BINARY_DIR} ${CARGO_CMD} 
    COMMAND cp ${CCX_RUST_SO} ${CMAKE_CURRENT_BINARY_DIR}
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
set_target_properties(ccx_rust PROPERTIES LOCATION ${CCX_RUST_SO})

add_test(NAME ccx_rust_test 
    COMMAND cargo test
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
