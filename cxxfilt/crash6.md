Use CVE-2025-66866 for:

** RESERVED ** An issue was discovered in function d_abi_tags in file cp-demangle.c in BinUtils 2.26 allows attackers to cause a denial of service via crafted PE file.

# SEGV /binutils-2.26/libiberty/./cp-demangle.c:1311:17 in d_abi_tags

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
4. download poc6: 
```
wget https://github.com/caozhzh/CRGF-Vul/raw/refs/heads/main/pocs/poc6
```
5. command for reproducing the error
```
cat poc6 | binutils/cxxfilt
```

## crash report
```
> cat poc6 | binutils/cxxfilt 
AddressSanitizer:DEADLYSIGNAL
=================================================================
==27999==ERROR: AddressSanitizer: SEGV on unknown address (pc 0x00000087e036 bp 0x7ffe227d6350 sp 0x7ffe227d62f0 T0)
==27999==The signal is caused by a READ memory access.
==27999==Hint: this fault was caused by a dereference of a high value address (see register values below).  Disassemble the provided pc to learn which register was used.
    #0 0x87e036 in d_abi_tags /binutils-2.26/libiberty/./cp-demangle.c:1311:17
    #1 0x87bede in d_unqualified_name /binutils-2.26/libiberty/./cp-demangle.c:1597:11
    #2 0x87a127 in d_name /binutils-2.26/libiberty/./cp-demangle.c:1399:12
    #3 0x8734d4 in d_class_enum_type /binutils-2.26/libiberty/./cp-demangle.c:2804:10
    #4 0x870279 in cplus_demangle_type /binutils-2.26/libiberty/./cp-demangle.c:2342:13
    #5 0x873a9c in d_pointer_to_member_type /binutils-2.26/libiberty/./cp-demangle.c:2891:8
    #6 0x870319 in cplus_demangle_type /binutils-2.26/libiberty/./cp-demangle.c:2350:13
    #7 0x87e5eb in d_parmlist /binutils-2.26/libiberty/./cp-demangle.c:2737:14
    #8 0x87a34d in d_bare_function_type /binutils-2.26/libiberty/./cp-demangle.c:2791:8
    #9 0x86f2c0 in d_encoding /binutils-2.26/libiberty/./cp-demangle.c:1296:6
    #10 0x86e717 in cplus_demangle_mangled_name /binutils-2.26/libiberty/./cp-demangle.c:1172:7
    #11 0x878118 in d_demangle_callback /binutils-2.26/libiberty/./cp-demangle.c:5894:7
    #12 0x8776ef in d_demangle /binutils-2.26/libiberty/./cp-demangle.c:5945:12
    #13 0x877575 in cplus_demangle_v3 /binutils-2.26/libiberty/./cp-demangle.c:6102:10
    #14 0x850a16 in cplus_demangle /binutils-2.26/libiberty/./cplus-dem.c:864:13
    #15 0x4cc2f9 in demangle_it /binutils-2.26/binutils/cxxfilt.c:62:12
    #16 0x4cbf3c in main /binutils-2.26/binutils/cxxfilt.c:275:4
    #17 0x7f2c3713bd79 in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x23d79)
    #18 0x41f4e9 in _start (/binutils-2.26/binutils/cxxfilt+0x41f4e9)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /binutils-2.26/libiberty/./cp-demangle.c:1311:17 in d_abi_tags
==27999==ABORTING
```
