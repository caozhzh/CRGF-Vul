# SEGV /binutils-2.26/libiberty/./cp-demangle.c:3471:7 in d_discriminator

## Environment
docker image:silkeh/clang:12(Debian GNU/Linux 11 64 bit, clang 12.0.1)
binutils 2.26

## Steps to reproduce
1. start docker container
```
docker run -it --rm silkeh/clang:12 bash
```
2. download file
```
wget http://ftp.gnu.org/gnu/binutils/binutils-2.26.tar.gz
tar -xzf binutils-2.26.tar.gz 
cd binutils-2.26
```
3. compile
```
export CC="clang"
export CFLAGS="-g -Wno-error -fsanitize=address"
# avoid memory leak warnings detected by the LeakSanitizer (LSan) tool.
export ASAN_OPTIONS=detect_leaks=0
./configure
make -j
```
4. download poc2: 
```
wget https://github.com/caozhzh/CRGF-Vul/raw/refs/heads/main/pocs/poc2
```
5. command for reproducing the error
```
cat poc2 | binutils/cxxfilt
```

## crash report
```
> cat poc2 | binutils/cxxfilt 
AddressSanitizer:DEADLYSIGNAL
=================================================================
==28008==ERROR: AddressSanitizer: SEGV on unknown address (pc 0x00000087c531 bp 0x7fffed5a5110 sp 0x7fffed5a50d0 T0)
==28008==The signal is caused by a READ memory access.
==28008==Hint: this fault was caused by a dereference of a high value address (see register values below).  Disassemble the provided pc to learn which register was used.
    #0 0x87c531 in d_discriminator /binutils-2.26/libiberty/./cp-demangle.c:3471:7
    #1 0x87bd00 in d_unqualified_name /binutils-2.26/libiberty/./cp-demangle.c:1576:13
    #2 0x87a127 in d_name /binutils-2.26/libiberty/./cp-demangle.c:1399:12
    #3 0x86ec18 in d_encoding /binutils-2.26/libiberty/./cp-demangle.c:1257:12
    #4 0x86e717 in cplus_demangle_mangled_name /binutils-2.26/libiberty/./cp-demangle.c:1172:7
    #5 0x878118 in d_demangle_callback /binutils-2.26/libiberty/./cp-demangle.c:5894:7
    #6 0x8776ef in d_demangle /binutils-2.26/libiberty/./cp-demangle.c:5945:12
    #7 0x877575 in cplus_demangle_v3 /binutils-2.26/libiberty/./cp-demangle.c:6102:10
    #8 0x850a16 in cplus_demangle /binutils-2.26/libiberty/./cplus-dem.c:864:13
    #9 0x4cc2f9 in demangle_it /binutils-2.26/binutils/cxxfilt.c:62:12
    #10 0x4cbf3c in main /binutils-2.26/binutils/cxxfilt.c:275:4
    #11 0x7fc7dd4bcd79 in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x23d79)
    #12 0x41f4e9 in _start (/binutils-2.26/binutils/cxxfilt+0x41f4e9)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /binutils-2.26/libiberty/./cp-demangle.c:3471:7 in d_discriminator
==28008==ABORTING
```
