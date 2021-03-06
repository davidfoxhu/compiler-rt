configure_lit_site_cfg(
  ${CMAKE_CURRENT_SOURCE_DIR}/lit.site.cfg.in
  ${CMAKE_CURRENT_BINARY_DIR}/lit.site.cfg
  )

if(COMPILER_RT_CAN_EXECUTE_TESTS)
  # Run UBSan output tests only if we're sure that clang would produce
  # working binaries.
  set(UBSAN_TEST_DEPS
    ${SANITIZER_COMMON_LIT_TEST_DEPS}
    asan
    ${UBSAN_RUNTIME_LIBRARIES})
  set(UBSAN_TEST_PARAMS
    ubsan_site_config=${CMAKE_CURRENT_BINARY_DIR}/lit.site.cfg
    )
  add_lit_testsuite(check-ubsan "Running UndefinedBehaviorSanitizer tests"
    ${CMAKE_CURRENT_BINARY_DIR}
    PARAMS ${UBSAN_TEST_PARAMS}
    DEPENDS ${UBSAN_TEST_DEPS}
    )
  set_target_properties(check-ubsan PROPERTIES FOLDER "UBSan unittests")
endif()
