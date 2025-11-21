# SEGV /binutils-2.26/libiberty/./cp-demangle.c:4442:30 in d_print_comp_inner

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
4. download poc5: 
```
wget https://github.com/caozhzh/CRGF-Vul/raw/refs/heads/main/pocs/poc5
```
5. command for reproducing the error
```
cat poc5 | binutils/cxxfilt
```

## crash report
```
> cat poc5 | binutils/cxxfilt 
AddressSanitizer:DEADLYSIGNAL
=================================================================
==27996==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000000 (pc 0x000000883975 bp 0x7ffc46182f90 sp 0x7ffc461817c0 T0)
==27996==The signal is caused by a READ memory access.
==27996==Hint: address points to the zero page.
    #0 0x883975 in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4442:30
    #1 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #2 0x876069 in cplus_demangle_print_callback /binutils-2.26/libiberty/./cp-demangle.c:4097:5
    #3 0x878312 in d_demangle_callback /binutils-2.26/libiberty/./cp-demangle.c:5923:16
    #4 0x8776ef in d_demangle /binutils-2.26/libiberty/./cp-demangle.c:5945:12
    #5 0x877575 in cplus_demangle_v3 /binutils-2.26/libiberty/./cp-demangle.c:6102:10
    #6 0x850a16 in cplus_demangle /binutils-2.26/libiberty/./cplus-dem.c:864:13
    #7 0x4cc2f9 in demangle_it /binutils-2.26/binutils/cxxfilt.c:62:12
    #8 0x4cbf3c in main /binutils-2.26/binutils/cxxfilt.c:275:4
    #9 0x7fe0afbe3d79 in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x23d79)
    #10 0x41f4e9 in _start (/binutils-2.26/binutils/cxxfilt+0x41f4e9)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /binutils-2.26/libiberty/./cp-demangle.c:4442:30 in d_print_comp_inner
==27996==ABORTING
```
