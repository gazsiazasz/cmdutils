cmake_minimum_required(VERSION 3.0)
project(cmdutils)

include(ExternalProject)

option(INSTALL_HERE "" ON)

if (INSTALL_HERE)
    set(ADDITIONAL_FLAGS --prefix=${CMAKE_SOURCE_DIR}/compiled)
endif (INSTALL_HERE)

#[[
ExternalProject_Add(
        tnftpd
        URL               ftp://ftp.netbsd.org/pub/NetBSD/misc/tnftp/tnftpd-20130325.tar.gz
        CONFIGURE_COMMAND <SOURCE_DIR>/configure ${ADDITIONAL_FLAGS}

        BUILD_ALWAYS      OFF
        BUILD_IN_SOURCE   OFF)
]]

ExternalProject_Add(
        tnftp
        BUILD_BYPRODUCTS  compiled/bin/tnftp compiled/share/man/man1/tnftp.1
        URL               ftp://ftp.netbsd.org/pub/NetBSD/misc/tnftp/tnftp-20151004.tar.gz
        CONFIGURE_COMMAND <SOURCE_DIR>/configure ${ADDITIONAL_FLAGS}

        BUILD_ALWAYS      OFF
        BUILD_IN_SOURCE   OFF)
message(${CMAKE_INSTALL_PREFIX})


install(PROGRAMS compiled/bin/tnftp
        DESTINATION bin
        RENAME ftp)

install(FILES compiled/share/man/man1/tnftp.1
        DESTINATION share/man/man1
        RENAME ftp.1)