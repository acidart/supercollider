install(FILES ${CMAKE_SOURCE_DIR}/COPYING
    DESTINATION SuperCollider
    RENAME LICENSE.txt
)
install(FILES ${CMAKE_SOURCE_DIR}/README_WINDOWS.md
    DESTINATION SuperCollider
    RENAME README.md
)
install(FILES ${CMAKE_SOURCE_DIR}/platform/windows/Resources/sc_cube.ico
    DESTINATION .
)

# This target invokes fixup_bundle to copy dependencies
# into the installation directory:
add_custom_target( bundle
    COMMAND ${CMAKE_COMMAND}
        "-DSC_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}"
        "-DSC_DEPENDENCY_DIRS=${CMAKE_LIBRARY_PATH}"
        -P "${CMAKE_CURRENT_SOURCE_DIR}/bundle.cmake"
    COMMENT "Bundling the dependencies:"
    VERBATIM
)

# This target invokes a custom NSIS script to create
# the installer, because CPack would not create a suitable
# installer for sced:
add_custom_target( installer
    COMMAND ${CMAKE_COMMAND}
        "-DSC_VERSION=${PROJECT_VERSION}"
        "-DSC_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}"
        "-DSC_ED=${SC_ED}"
        "-DNSIS_SCRIPT=${CMAKE_CURRENT_SOURCE_DIR}/supercollider.nsi"
        -P "${CMAKE_CURRENT_SOURCE_DIR}/package.cmake"
    COMMENT "Building the installer:"
    VERBATIM
)
