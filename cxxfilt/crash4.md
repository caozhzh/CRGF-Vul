# stack-overflow /binutils-2.26/libiberty/./cp-demangle.c:4320 in d_print_comp_inner

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
4. download poc4: 
```
wget https://github.com/caozhzh/CRGF-Vul/raw/refs/heads/main/pocs/poc4
```
5. command for reproducing the error
```
cat poc4 | binutils/cxxfilt
```

## crash report
```
> cat poc4 | binutils/cxxfilt 
AddressSanitizer:DEADLYSIGNAL
=================================================================
==27993==ERROR: AddressSanitizer: stack-overflow on address 0x7ffd7bab7a80 (pc 0x0000008829ff bp 0x7ffd7bab9250 sp 0x7ffd7bab7a80 T0)
    #0 0x8829ff in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4320
    #1 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #2 0x887af7 in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4976:2
    #3 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #4 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #5 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #6 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #7 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #8 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #9 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #10 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #11 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #12 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #13 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #14 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #15 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #16 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #17 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #18 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #19 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #20 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #21 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #22 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #23 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #24 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #25 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #26 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #27 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #28 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #29 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #30 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #31 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #32 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #33 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #34 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #35 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #36 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #37 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #38 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #39 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #40 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #41 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #42 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #43 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #44 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #45 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #46 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #47 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #48 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #49 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #50 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #51 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #52 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #53 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #54 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #55 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #56 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #57 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #58 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #59 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #60 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #61 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #62 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #63 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #64 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #65 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #66 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #67 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #68 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #69 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #70 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #71 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #72 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #73 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #74 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #75 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #76 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #77 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #78 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #79 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #80 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #81 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #82 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #83 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #84 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #85 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #86 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #87 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #88 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #89 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #90 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #91 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #92 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #93 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #94 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #95 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #96 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #97 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #98 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #99 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #100 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #101 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #102 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #103 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #104 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #105 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #106 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #107 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #108 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #109 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #110 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #111 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #112 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #113 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #114 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #115 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #116 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #117 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #118 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #119 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #120 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #121 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #122 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #123 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #124 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #125 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #126 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #127 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #128 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #129 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #130 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #131 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #132 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #133 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #134 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #135 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #136 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #137 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #138 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #139 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #140 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #141 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #142 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #143 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #144 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #145 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #146 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #147 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #148 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #149 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #150 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #151 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #152 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #153 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #154 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #155 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #156 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #157 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #158 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #159 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #160 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #161 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #162 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #163 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #164 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #165 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #166 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #167 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #168 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #169 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #170 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #171 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #172 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #173 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #174 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #175 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #176 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #177 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #178 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #179 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #180 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #181 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #182 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #183 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #184 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #185 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #186 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #187 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #188 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #189 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #190 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #191 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #192 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #193 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #194 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #195 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #196 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #197 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #198 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #199 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #200 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #201 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #202 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #203 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #204 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #205 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #206 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #207 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #208 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #209 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #210 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #211 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #212 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #213 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #214 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #215 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #216 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #217 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #218 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #219 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #220 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #221 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #222 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #223 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #224 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #225 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #226 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #227 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #228 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #229 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #230 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #231 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #232 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #233 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #234 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #235 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #236 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #237 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #238 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #239 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #240 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #241 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #242 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #243 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #244 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #245 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #246 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4
    #247 0x876931 in d_print_comp /binutils-2.26/libiberty/./cp-demangle.c:5394:3
    #248 0x887c8f in d_print_comp_inner /binutils-2.26/libiberty/./cp-demangle.c:4988:4

SUMMARY: AddressSanitizer: stack-overflow /binutils-2.26/libiberty/./cp-demangle.c:4320 in d_print_comp_inner
==27993==ABORTING
```
