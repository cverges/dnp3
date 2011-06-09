AC_INIT([opendnp3], [0.9.5-rc1], [open-dnp3@googlegroups.com])
AC_COPYRIGHT([(C) 2010, 2011 Green Energy Corp.  All rights reserved.])

srcdir="`cd $srcdir; pwd`"

AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_MACRO_DIR([config/autotools/m4])

AM_INIT_AUTOMAKE([subdir-objects])

AC_PREFIX_DEFAULT([/usr])

AC_CONFIG_FILES([Makefile])
AC_CONFIG_HEADERS([config.h])

AC_LANG([C++])

AX_BOOST_BASE([1.43],, AC_MSG_ERROR(missing boost))
AX_BOOST_DATE_TIME
AX_BOOST_PROGRAM_OPTIONS
AX_BOOST_SYSTEM
AX_BOOST_THREAD
AX_BOOST_UNIT_TEST_FRAMEWORK

AC_ARG_ENABLE(lcov,
  AS_HELP_STRING([--enable-lcov], [Enable gcov/lcov support]),
    [ac_lcov_enabled=$enableval], [ac_lcov_enabled=no])
if test "$ac_lcov_enabled" = yes; then
  AC_CHECK_PROGS(gcov, gcov)
  if ! test "$gcov"; then
    AC_MSG_ERROR([Could not find gcov utility, check your gcc instalation])
  fi
  AC_CHECK_LIB(gcov, __gcov_init, [], [])
  AC_CHECK_PROGS(lcov, lcov)
  AC_CHECK_PROGS(genhtml, genhtml)
  AC_CHECK_PROGS(gendesc, gendesc)
  AC_CHECK_PROGS(geninfo, geninfo)
  if ! test "$lcov" || ! test "$genhtml" || ! test "$gendesc" || 
     ! test "$geninfo"; then
    AC_MSG_ERROR([Could not find lcov utilities, do you have lcov installed?])
  fi
  CFLAGS="--coverage -g -O0"
  CXXFLAGS="--coverage -g -O0"
  LDFLAGS="--coverage"
  enable_static=yes
  enable_shared=no
fi
AM_CONDITIONAL(LCOV, [test "$ac_lcov_enabled" = yes])

AC_CHECK_HEADERS([arpa/inet.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([ctype.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([errno.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([fcntl.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([float.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([getopt.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([inttypes.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([math.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([pthread.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([setjmp.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([signal.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([stdarg.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([stdio.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([stdlib.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([string.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([sys/select.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([sys/stat.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([sys/types.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([sys/wait.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([syslog.h]	,,AC_MSG_ERROR(missing header))
AC_CHECK_HEADERS([unistd.h]	,,AC_MSG_ERROR(missing header))

AC_CHECK_LIB([c],	[atexit]		,,AC_MSG_ERROR(missing library))
AC_CHECK_LIB([pthread],	[pthread_join]		,,AC_MSG_ERROR(missing library))

AC_CHECK_HEADERS([tinyxml.h],			ac_tinyxml_h_found=yes,		ac_tinyxml_h_found=no)
AC_CHECK_LIB([tinyxml],	[_ZTI9TiXmlNode],	ac_tinyxml_lib_found=yes,	ac_tinyxml_lib_found=no)

AC_ARG_ENABLE(testset,
  AS_HELP_STRING([--enable-testset], [Enable building the testset application]),
    [ac_testset_enabled=$enableval], [ac_testset_enabled=no])
if test "$ac_testset_enabled" = yes; then
  if test "$ac_tinyxml_h_found" = no -o "$ac_tinyxml_lib_found" = no; then
    AC_MSG_ERROR("missing one or more components of the tinyxml library -- unable to build the testset application!")
  fi
fi
AM_CONDITIONAL(ENABLE_TESTSET, [test "$ac_testset_enabled" = yes])

AC_PROG_AWK
AC_PROG_CXX
AC_PROG_GREP
AC_PROG_INSTALL
AC_PROG_MKDIR_P
AC_PROG_SED

LT_INIT
dnl AX_LIBTOOLIZE_CFLAGS(CFLAGS)
dnl AX_LIBTOOLIZE_CFLAGS(CXXFLAGS)

AC_FUNC_FORK

AC_STRUCT_TIMEZONE

AC_TYPE_SIZE_T
AC_TYPE_SSIZE_T

AM_PROG_CC_C_O

DX_DOXYGEN_FEATURE(ON)
DX_DOT_FEATURE(OFF)
DX_HTML_FEATURE(ON)
DX_CHM_FEATURE(OFF)
DX_CHI_FEATURE(OFF)
DX_MAN_FEATURE(OFF)
DX_RTF_FEATURE(OFF)
DX_XML_FEATURE(OFF)
DX_PDF_FEATURE(OFF)
DX_PS_FEATURE(OFF)
DX_INIT_DOXYGEN($PACKAGE_NAME, [config/doxygen.config])

AC_OUTPUT

echo "
$PACKAGE_NAME $PACKAGE_VERSION configuration:
-----------------------------------------------------
Source code location:       ${srcdir}
Host system type:           ${host}
Install path (prefix):      ${prefix}
Boost libraries:            ${BOOSTLIBDIR}

CXX:                        ${CXX}
CXXFLAGS:                   ${CXXFLAGS}
LDFLAGS:                    ${LDFLAGS}

lcov support:               ${ac_lcov_enabled}
testset application:        ${ac_testset_enabled}
shared libraries:           ${enable_shared}
static libraries:           ${enable_static}
-----------------------------------------------------

When ready, type 'make' to build.
"
