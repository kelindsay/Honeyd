cmake_minimum_required(VERSION 2.8)
project(honeyd)

set(CMAKE_COLOR_MAKEFILE ON)

# Include our extra files
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/)

include(CheckIncludeFiles)
include(CheckLibraryExists)
include(CheckFunctionExists)
include(CheckStructHasMember)
include(CheckTypeExists)
include(FindPkgConfig)
include(TestBigEndian)

#include(CheckCmakeCompat)
#include(AutoVersioning)
#include(ConfigOptions)
#include(ComplexLibrary)
include(FeatureSummary)
include(CheckCCompilerFlag)
include(GNUInstallDirs)

set(PATH_HONEYDINCLUDE "${CMAKE_INSTALL_FULL_INCLUDEDIR}/honeyd")
set(PATH_HONEYDLIB "${CMAKE_INSTALL_FULL_LIBDIR}/honeyd")
set(PATH_HONEYDDATA "${CMAKE_INSTALL_FULL_DATAROOTDIR}/honeyd")

include(FindZLIB)

# DISABLE_KQUEUE
# DISABLE_POLL
set(DLOPENLIBC "libc.so.6")
# DL_NEED_UNDERSCORE
check_struct_has_member("struct msghdr" msg_accrights sys/socket.h HAVE_ACCRIGHTS_IN_MSGHDR)
check_function_exists(asprintf HAVE_ASPRINTF)
check_include_files(assert.h HAVE_ASSERT_H)
# HAVE_BROKEN_DNET
check_struct_has_member("struct msghdr" msg_control sys/socket.h HAVE_CONTROL_IN_MSGHDR)
check_function_exists(daemon HAVE_DAEMON)
check_include_files(dlfcn.h HAVE_DLFCN_H)
# HAVE_DOPRNT
set(HAVE_DUMBNET 1)
# HAVE_DUMBNET
check_function_exists(dup2 HAVE_DUP2)
check_function_exists(err HAVE_ERR)
check_include_files(errno.h HAVE_ERRNO_H)
check_include_files(fcntl.h HAVE_FCNTL_H)
check_function_exists(fgetln HAVE_FGETLN)
check_function_exists(freeaddrinfo HAVE_FREEADDRINFO)
check_function_exists(getaddrinfo HAVE_GETADDRINFO)
check_function_exists(getnameinfo HAVE_GETNAMEINFO)
check_function_exists(getopt_long HAVE_GETOPT_LONG)
check_function_exists(gettimeofday HAVE_GETTIMEOFDAY)
check_include_files(inttypes.h HAVE_INTTYPES_H)
check_function_exists(isblank HAVE_ISBLANK)
check_function_exists(kqueue HAVE_KQUEUE)
# libedit
set(HAVE_LIBEVENT 1)
set(HAVE_LIBREADLINE 1)
if (ZLIB_FOUND)
  set(HAVE_LIBZ 1)
endif (ZLIB_FOUND)
# libevent
# libreadline
# libz
check_function_exists(memmove HAVE_MEMMOVE)
check_include_files(memory.h HAVE_MEMORY_H)
check_function_exists(memset HAVE_MEMSET)
check_include_files(net/bpf.h HAVE_NET_BPF_H)
check_include_files(paths.h HAVE_PATHS_H)
check_library_exists(pcap pcap_get_selectable_fd "" HAVE_PCAP_GET_SELECTABLE_FD)
# HAVE_PCAP_GET_SELECTED_FD
#set(HAVE_PCAP_GET_SELECTED_FD)
# HAVE_PYDNET
# HAVE_PYTHON
check_function_exists(recvmsg HAVE_RECVMSG)
check_function_exists(sendmsg HAVE_SENDMSG)
check_function_exists(setgroups HAVE_SETGROUPS)
check_function_exists(setregid HAVE_SETREGID)
check_function_exists(setruid HAVE_SETRUID)
check_function_exists(SHA1Update HAVE_SHA1UPDATE)
check_struct_has_member("struct sockaddr" sa_len sys/socket.h HAVE_SOCKADDR_SA_LEN)
check_include_files(stdarg.h HAVE_STDARG_H)
check_include_files(stdint.h HAVE_STDINT_H)
check_include_files(stdlib.h HAVE_STDLIB_H)
check_function_exists(strcasecmp HAVE_STRCASECMP)
check_function_exists(strchr HAVE_STRCHR)
check_function_exists(strdup HAVE_STRDUP)
check_include_files(strings.h HAVE_STRINGS_H)
check_include_files(string.h HAVE_STRING_H)
check_function_exists(strlcat HAVE_STRLCAT)
check_function_exists(strlcpy HAVE_STRLCPY)
check_function_exists(strncasecmp HAVE_STRNCASECMP)
check_function_exists(strsep HAVE_STRSEP)
check_function_exists(strspn HAVE_STRSPN)
check_function_exists(strtoul HAVE_STRTOUL)
check_type_exists("struct sockaddr_storage" sys/socket.h HAVE_STRUCT_SOCKADDR_STORAGE)
check_struct_has_member("struct socketaddr_un" sun_len sys/socket.h HAVE_SUN_LEN)
check_include_files(syslog.h HAVE_SYSLOG_H)
check_include_files(sys/file.h HAVE_SYS_FILE_H)
check_include_files(sys/ioccom.h HAVE_SYS_IOCCOM_H)
check_include_files(sys/ioctl.h HAVE_SYS_IOCTL_H)
check_include_files(sys/param.h HAVE_SYS_PARAM_H)
check_include_files(sys/socket.h HAVE_SYS_SOCKET_H)
check_include_files(sys/stat.h HAVE_SYS_STAT_H)
check_include_files(sys/time.h HAVE_SYS_TIME_H)
check_include_files(sys/types.h HAVE_SYS_TYPES_H)
# sys/wait.h posix.1 compatible
set(HAVE_SYS_WAIT_H 1)
set(HAVE_TIMERADD 1)
#check_function_exists(timeradd HAVE_TIMERADD)
check_include_files(time.h HAVE_TIME_H)
check_include_files(unistd.h HAVE_UNISTD_H)
check_function_exists(vprintf HAVE_VPRINTF)
# LT_OBJDIR
# NODLOPEN
# PACKAGE
# PACKGE_BUGREPORT
# PACKAGE_NAME
# PACKAGE_STRING
# PACKAGE_TARNAME
# PACKAGE_URL
# PACKAGE_VERSION
# RETSIGTYPE
# STDC_HEADERS
# TIME_WITH_SYS_TIME
# VERSION
set(VERSION "1.6e")
# YYTEXT_POINTER
# const
# ...

# Include directories
include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${CMAKE_CURRENT_BINARY_DIR}/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

# Configure files
add_definitions("-DHAVE_CONFIG_H")
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/config.h.in ${CMAKE_CURRENT_BINARY_DIR}/config.h)

add_subdirectory(pypcap)
add_subdirectory(regress)

# set the path to the library folder
link_directories(/usr/lib)
#link_directories(/usr/local/lib)

add_definitions(-DHAVE_CONFIG_H)
add_definitions(-DPATH_HONEYDINCLUDE=\"/home/alenor/local/include/honeyd\")
add_definitions(-DPATH_HONEYDLIB=\"/home/alenor/local/lib/honeyd\")
add_definitions(-DPATH_HONEYDDATA=\"/home/alenor/local/share/honeyd\")
add_definitions(-DPATH_RRDTOOL=\"/usr/bin/rrdtool\")
add_definitions(-DHONEYD_PLUGINS_DECLARE=)
add_definitions(-DHONEYD_PLUGINS=)

include_directories (${honeyd_SOURCE_DIR}/compat)
include_directories (${honeyd_SOURCE_DIR}/compat/libdnet)

add_executable(hsniff
    hsniff.c
    tagging.c
    stats.c
    util.c
    hooks.c
    interface.c
    pfctl_osfp.c
    pf_osfp.c
    osfp.c
    network.c
    sha1.c
)

# link the libraries to the executable
target_link_libraries (hsniff pcap dumbnet event z dl)

add_executable(honeydctl
    honeydctl.c
    strlcpy.c
    strlcat.c
    sha1.c
)
target_link_libraries(honeydctl readline termcap dl)

add_library(libhoneyd
    honeyd_overload.c
    atomicio.c
    fdpass.c
)
set_target_properties(libhoneyd PROPERTIES PREFIX "")

add_executable(honeydstats
    honeydstats.c
    honeydstats_main.c
    tagging.c
    stats.c
    util.c
    histogram.c
    analyze.c
    untagging.c
    filter.c
    keycount.c
    sha1.c
)

# link the libraries to the executable
target_link_libraries (honeydstats  dumbnet event z dl)

add_executable(honeyd
    honeyd.c
    command.c
    parse.c
    lex.c
    config.c
    personality.c
    util.c
    ipfrag.c
    router.c
    tcp.c
    udp.c
    xprobe_assoc.c
    log.c
    fdpass.c
    atomicio.c
    subsystem.c
    hooks.c
    plugins.c
    plugins_config.c
    pool.c
    interface.c
    arp.c
    gre.c
    network.c
    pfctl_osfp.c
    pf_osfp.c
    condition.c
    osfp.c
    ui.c
    ethernet.c
    tagging.c
    stats.c
    dhcpclient.c
    rrdtool.c
    histogram.c
    untagging.c
    strlcpy.c
    strlcat.c
    sha1.c
)

# link the libraries to the executable
target_link_libraries (honeyd pcap dumbnet event m z dl)
