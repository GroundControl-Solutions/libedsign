cmake_minimum_required(VERSION 3.26)
project(edsign VERSION 1.0.0 LANGUAGES C)

add_library(edsign
            lib/blake2.c
            lib/ed25519.c
            lib/keypair.c
            lib/randombytes.c
            lib/scrypt.c
            lib/sign.c
            lib/util.c
            lib/verify.c
            )

add_library(well-typed::edsign ALIAS edsign)

target_sources(edsign PUBLIC
               FILE_SET public_headers
               TYPE HEADERS
               BASE_DIRS lib
               FILES
               lib/edsign.h
               )

install(TARGETS edsign
        EXPORT libedsign-targets
        FILE_SET public_headers
        )

include(CMakePackageConfigHelpers)
configure_package_config_file(libedsign-config.cmake.in
                              libedsign-config.cmake
                              INSTALL_DESTINATION "cmake"
                              )
write_basic_package_version_file(
		"${CMAKE_CURRENT_BINARY_DIR}/libedsign-config-version.cmake"
		COMPATIBILITY SameMajorVersion
)
install(FILES
        "${CMAKE_CURRENT_BINARY_DIR}/libedsign-config.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/libedsign-config-version.cmake"
        DESTINATION "cmake"
        )
install(EXPORT libedsign-targets
        FILE
        libedsign-targets.cmake
        NAMESPACE well-typed::
        DESTINATION "cmake"
        )
