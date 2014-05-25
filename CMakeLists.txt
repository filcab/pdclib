cmake_minimum_required(VERSION 2.8.10)
project(pdclib C CXX ASM)

# Most of this was lifted from BigCheese's nucleo-toolchain

# Example flags that should allow us to compile pdclib for the teensy3
#if (NOT CMAKE_C_FLAGS)
#  message("* Setting up default CFLAGS for the pdclib (teensy3 target)")
#  set(TEENSY3_TARGET "--target=arm-none-eabi")
#  set(TEENSY3_FLAGS "${TEENSY3_TARGET} -mcpu=cortex-m4 -mthumb -D__MK20DX128__")
#  set(COMMON_FLAGS "-ffunction-sections -fdata-sections -fcolor-diagnostics -fno-exceptions ${TEENSY3_FLAGS}")
#  set(CMAKE_C_FLAGS "-std=c11 ${COMMON_FLAGS} ${CMAKE_C_FLAGS}")
#  set(CMAKE_CXX_FLAGS "-std=c++11 ${COMMON_FLAGS} ${CMAKE_CXX_FLAGS}")
#  set(CMAKE_ASM_FLAGS "${COMMON_FLAGS} ${CMAKE_ASM_FLAGS}")
#  set(CMAKE_SIZEOF_VOID_P 4)
#endif()

# platform-specific includes and sources
include_directories(SYSTEM platform/example/includes platform/example/internals)

include_directories(internals)
include_directories(includes)
include_directories(opt/nothread)
include_directories(opt/tss_errno)

add_definitions(-fno-builtin -nostdinc)

set(PLATFORM_SRCS
  platform/example/functions/_PDCLIB/_PDCLIB_Exit.c
  platform/example/functions/_PDCLIB/_PDCLIB_fileops.c
  platform/example/functions/_PDCLIB/_PDCLIB_open.c
  platform/example/functions/_PDCLIB/allocpages.c
  platform/example/functions/_PDCLIB/freepages.c
  platform/example/functions/_PDCLIB/rename.c
  platform/example/functions/_PDCLIB/stdinit.c

  platform/example/functions/signal/raise.c
  platform/example/functions/signal/signal.c

  platform/example/functions/stdio/remove.c
  platform/example/functions/stdio/tmpfile.c

  platform/example/functions/stdlib/getenv.c
  platform/example/functions/stdlib/system.c
  )


add_library(pdc
  ${PLATFORM_SRCS}

  opt/tss_errno/errno.c

  opt/basecodecs/_PDCLIB_ascii.c

  opt/nothread/call_once.c
  opt/nothread/cnd_init.c
  opt/nothread/cnd_signal.c
  opt/nothread/cnd_wait.c
  opt/nothread/mtx_destroy.c
  opt/nothread/mtx_init.c
  opt/nothread/mtx_lock.c
  opt/nothread/mtx_timedlock.c
  opt/nothread/mtx_trylock.c
  opt/nothread/mtx_unlock.c
  opt/nothread/thrd_yield.c
  opt/nothread/tss_create.c
  opt/nothread/tss_delete.c
  opt/nothread/tss_get.c
  opt/nothread/tss_set.c

  functions/ctype/isalnum.c
  functions/ctype/isalpha.c
  functions/ctype/isblank.c
  functions/ctype/iscntrl.c
  functions/ctype/isdigit.c
  functions/ctype/isgraph.c
  functions/ctype/islower.c
  functions/ctype/isprint.c
  functions/ctype/ispunct.c
  functions/ctype/isspace.c
  functions/ctype/isupper.c
  functions/ctype/isxdigit.c
  functions/ctype/tolower.c
  functions/ctype/toupper.c

  functions/inttypes/imaxabs.c
  functions/inttypes/imaxdiv.c
  functions/inttypes/strtoimax.c
  functions/inttypes/strtoumax.c

  functions/locale/freelocale.c
  functions/locale/localeconv.c
  functions/locale/setlocale.c
  functions/locale/UnicodeData.py
  functions/locale/uselocale.c
  functions/locale/_PDCLIB_mb_cur_max.c
  functions/locale/_PDCLIB_unicodedata.c

  functions/stdio/clearerr.c
  functions/stdio/fclose.c
  functions/stdio/feof.c
  functions/stdio/ferror.c
  functions/stdio/fflush.c
  functions/stdio/fgetc.c
  functions/stdio/fgetpos.c
  functions/stdio/fgets.c
  functions/stdio/flockfile.c
  functions/stdio/fopen.c
  functions/stdio/fprintf.c
  functions/stdio/fputc.c
  functions/stdio/fputs.c
  functions/stdio/fread.c
  functions/stdio/freopen.c
  functions/stdio/fscanf.c
  functions/stdio/fseek.c
  functions/stdio/fsetpos.c
  functions/stdio/ftell.c
  functions/stdio/ftrylockfile.c
  functions/stdio/funlockfile.c
  functions/stdio/fwrite.c
  functions/stdio/getc.c
  functions/stdio/getchar.c
  functions/stdio/gets.c
  functions/stdio/perror.c
  functions/stdio/printf.c
  functions/stdio/putc.c
  functions/stdio/putchar.c
  functions/stdio/puts.c
  functions/stdio/rename.c
  functions/stdio/rewind.c
  functions/stdio/scanf.c
  functions/stdio/setbuf.c
  functions/stdio/setvbuf.c
  functions/stdio/snprintf.c
  functions/stdio/sprintf.c
  functions/stdio/sscanf.c
  functions/stdio/testfile.txt
  functions/stdio/tmpnam.c
  functions/stdio/ungetc.c
  functions/stdio/vfprintf.c
  functions/stdio/vfscanf.c
  functions/stdio/vprintf.c
  functions/stdio/vscanf.c
  functions/stdio/vsnprintf.c
  functions/stdio/vsprintf.c
  functions/stdio/vsscanf.c
  functions/stdio/_PDCLIB_filemode.c
  functions/stdio/_PDCLIB_fillbuffer.c
  functions/stdio/_PDCLIB_flushbuffer.c
  functions/stdio/_PDCLIB_ftell64.c
  functions/stdio/_PDCLIB_fvopen.c
  functions/stdio/_PDCLIB_prepread.c
  functions/stdio/_PDCLIB_prepwrite.c
  functions/stdio/_PDCLIB_print.c
  functions/stdio/_PDCLIB_scan.c
  functions/stdio/_PDCLIB_seek.c

  functions/stdlib/abort.c
  functions/stdlib/abs.c
  functions/stdlib/atexit.c
  functions/stdlib/atoi.c
  functions/stdlib/atol.c
  functions/stdlib/atoll.c
  functions/stdlib/bsearch.c
  functions/stdlib/div.c
  functions/stdlib/exit.c
  functions/stdlib/labs.c
  functions/stdlib/ldiv.c
  functions/stdlib/llabs.c
  functions/stdlib/lldiv.c
  functions/stdlib/qsort.c
  functions/stdlib/rand.c
  functions/stdlib/srand.c
  functions/stdlib/strtol.c
  functions/stdlib/strtoll.c
  functions/stdlib/strtoul.c
  functions/stdlib/strtoull.c
  functions/stdlib/_Exit.c

  functions/string/memchr.c
  functions/string/memcmp.c
  functions/string/memcpy.c
  functions/string/memmove.c
  functions/string/memset.c
  functions/string/strcat.c
  functions/string/strchr.c
  functions/string/strcmp.c
  functions/string/strcoll.c
  functions/string/strcpy.c
  functions/string/strcspn.c
  functions/string/strdup.c
  functions/string/strerror.c
  functions/string/strlen.c
  functions/string/strncat.c
  functions/string/strncmp.c
  functions/string/strncpy.c
  functions/string/strndup.c
  functions/string/strnlen.c
  functions/string/strpbrk.c
  functions/string/strrchr.c
  functions/string/strspn.c
  functions/string/strstr.c
  functions/string/strtok.c
  functions/string/strxfrm.c

  functions/uchar/c16rtomb.c
  functions/uchar/c32rtomb.c
  functions/uchar/mbrtoc16.c
  functions/uchar/mbrtoc32.c
  functions/uchar/_PDCLIB_c16slen.c
  functions/uchar/_PDCLIB_c32slen.c
  functions/uchar/_PDCLIB_c32srtombs.c
  functions/uchar/_PDCLIB_mbsrtoc32s.c

  functions/wchar/mbrtowc.c
  functions/wchar/mbsinit.c
  functions/wchar/wcrtomb.c
  functions/wchar/wcscat.c
  functions/wchar/wcschr.c
  functions/wchar/wcscmp.c
  functions/wchar/wcscpy.c
  functions/wchar/wcscspn.c
  functions/wchar/wcslen.c
  functions/wchar/wcsncat.c
  functions/wchar/wcsncmp.c
  functions/wchar/wcsncpy.c
  functions/wchar/wcspbrk.c
  functions/wchar/wcsrchr.c
  functions/wchar/wcsspn.c
  functions/wchar/wcsstr.c
  functions/wchar/wcstok.c
  functions/wchar/wmemchr.c
  functions/wchar/wmemcmp.c
  functions/wchar/wmemcpy.c
  functions/wchar/wmemmove.c

  functions/wctype/iswalnum.c
  functions/wctype/iswalpha.c
  functions/wctype/iswblank.c
  functions/wctype/iswcntrl.c
  functions/wctype/iswctype.c
  functions/wctype/iswdigit.c
  functions/wctype/iswgraph.c
  functions/wctype/iswlower.c
  functions/wctype/iswprint.c
  functions/wctype/iswpunct.c
  functions/wctype/iswspace.c
  functions/wctype/iswupper.c
  functions/wctype/iswxdigit.c
  functions/wctype/towctrans.c
  functions/wctype/towlower.c
  functions/wctype/towupper.c
  functions/wctype/wctrans.c
  functions/wctype/wctype.c

  functions/_PDCLIB/assert.c
  functions/_PDCLIB/Readme.txt
  functions/_PDCLIB/stdarg.c
  functions/_PDCLIB/_PDCLIB_atomax.c
  functions/_PDCLIB/_PDCLIB_closeall.c
  functions/_PDCLIB/_PDCLIB_digits.c
  functions/_PDCLIB/_PDCLIB_seed.c
  functions/_PDCLIB/_PDCLIB_strtox_main.c
  functions/_PDCLIB/_PDCLIB_strtox_prelim.c
  )
