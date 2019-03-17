cmake_minimum_required(VERSION 3.0)
project(cmdutils)

include(ExternalProject)

option(INSTALL_HERE "" ON)

set(ROOT_DIR ${CMAKE_SOURCE_DIR}/compiled)

if (INSTALL_HERE)
    set(ADDITIONAL_FLAGS --prefix=${ROOT_DIR})
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


ExternalProject_Add(
        libtelnet
        BUILD_BYPRODUCTS
        URL               https://opensource.apple.com/tarballs/libtelnet/libtelnet-13.tar.gz
        CONFIGURE_COMMAND ""
        BUILD_COMMAND     ""
        INSTALL_COMMAND   xcodebuild install ARCHS="x86_64" -project <SOURCE_DIR>/libtelnet.xcodeproj SYMROOT=<BINARY_DIR> DSTROOT=${ROOT_DIR}

        BUILD_ALWAYS      OFF
        BUILD_IN_SOURCE   OFF)

ExternalProject_Add(
        telnet
        DEPENDS           libtelnet
        BUILD_BYPRODUCTS  compiled/bin/telnet
        URL               https://opensource.apple.com/tarballs/remote_cmds/remote_cmds-54.50.1.tar.gz
        CMAKE_ARGS        -DCMAKE_INSTALL_PREFIX=${ROOT_PATH} -DCMAKE_INSTALL_PREFIX=${ROOT_DIR}
        UPDATE_COMMAND    cp ${CMAKE_SOURCE_DIR}/patch/remote_cmds.txt <SOURCE_DIR>/CMakeLists.txt

        BUILD_ALWAYS      OFF
        BUILD_IN_SOURCE   OFF)


install(PROGRAMS compiled/bin/tnftp
        DESTINATION bin
        RENAME ftp)

install(FILES compiled/share/man/man1/tnftp.1
        DESTINATION share/man/man1
        RENAME ftp.1)

install(PROGRAMS compiled/bin/telnet
        DESTINATION bin)
