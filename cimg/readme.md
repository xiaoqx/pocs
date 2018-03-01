cimg crashes
================

code:
http://cimg.eu/
https://github.com/dtschump/CImg

## 1. cimg-heap-overflow-1

$cimgload cimg-heap-overflow-1

```
=================================================================
==6193==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x7f91792ff800 at pc 0x00000049dd13 bp 0x7ffee7a8e890 sp 0x7ffee7a8e880
READ of size 1 at 0x7f91792ff800 thread T0
    #0 0x49dd12 in cimg_library::CImg<unsigned char>::_load_bmp(_IO_FILE*, char const*) ../CImg.h:48457
    #1 0x4b84a4 in cimg_library::CImg<unsigned char>::load_bmp(char const*) ../CImg.h:48280
    #2 0x4b84a4 in cimg_library::CImg<unsigned char>::load(char const*) ../CImg.h:48122
    #3 0x4022fa in cimg_library::CImg<unsigned char>::assign(char const*) ../CImg.h:11514
    #4 0x4022fa in cimg_library::CImg<unsigned char>::CImg(char const*) ../CImg.h:11161
    #5 0x4022fa in main /src/CImg/fuzz-test/bmp-test.cpp:25
    #6 0x7f917bb8f82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

0x7f91792ff800 is located 0 bytes to the right of 6287360-byte region [0x7f9178d00800,0x7f91792ff800)
allocated by thread T0 here:
    #0 0x7f917ca906b2 in operator new[](unsigned long) (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x996b2)
    #1 0x433fb6 in cimg_library::CImg<unsigned char>::assign(unsigned int, unsigned int, unsigned int, unsigned int) ../CImg.h:11379
    #2 0x49a75d in cimg_library::CImg<unsigned char>::assign(unsigned int, unsigned int, unsigned int, unsigned int, unsigned char const&) ../CImg.h:11399
    #3 0x49a75d in cimg_library::CImg<unsigned char>::_load_bmp(_IO_FILE*, char const*) ../CImg.h:48348
    #4 0x4b84a4 in cimg_library::CImg<unsigned char>::load_bmp(char const*) ../CImg.h:48280
    #5 0x4b84a4 in cimg_library::CImg<unsigned char>::load(char const*) ../CImg.h:48122
    #6 0x4022fa in cimg_library::CImg<unsigned char>::assign(char const*) ../CImg.h:11514
    #7 0x4022fa in cimg_library::CImg<unsigned char>::CImg(char const*) ../CImg.h:11161
    #8 0x4022fa in main /src/CImg/fuzz-test/bmp-test.cpp:25

SUMMARY: AddressSanitizer: heap-buffer-overflow ../CImg.h:48457 cimg_library::CImg<unsigned char>::_load_bmp(_IO_FILE*, char const*)
Shadow bytes around the buggy address:
  0x0ff2af257eb0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0ff2af257ec0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0ff2af257ed0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0ff2af257ee0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0ff2af257ef0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0ff2af257f00:[fa]fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0ff2af257f10: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0ff2af257f20: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0ff2af257f30: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0ff2af257f40: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0ff2af257f50: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07
  Heap left redzone:       fa
  Heap right redzone:      fb
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack partial redzone:   f4
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
==6193==ABORTING
```


## 2. cimg-double-free-1

```
=================================================================
==6191==ERROR: AddressSanitizer: attempting double-free on 0x62100001a500 in thread T0:
    #0 0x7f1be4f702ca in __interceptor_free (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x982ca)
    #1 0x7f1be40cbc55 in _IO_default_finish (/lib/x86_64-linux-gnu/libc.so.6+0x7bc55)
    #2 0x7f1be40bd29e in fclose (/lib/x86_64-linux-gnu/libc.so.6+0x6d29e)
    #3 0x7f1be4f6f7cd in fclose (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x977cd)
    #4 0x40a518 in cimg_library::cimg::fclose(_IO_FILE*) ../CImg.h:6187
    #5 0x49b051 in cimg_library::CImg<unsigned char>::_load_bmp(_IO_FILE*, char const*) ../CImg.h:48467
    #6 0x4b84a4 in cimg_library::CImg<unsigned char>::load_bmp(char const*) ../CImg.h:48280
    #7 0x4b84a4 in cimg_library::CImg<unsigned char>::load(char const*) ../CImg.h:48122
    #8 0x4022fa in cimg_library::CImg<unsigned char>::assign(char const*) ../CImg.h:11514
    #9 0x4022fa in cimg_library::CImg<unsigned char>::CImg(char const*) ../CImg.h:11161
    #10 0x4022fa in main /src/CImg/fuzz-test/bmp-test.cpp:25
    #11 0x7f1be407082f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

0x62100001a500 is located 0 bytes inside of 4096-byte region [0x62100001a500,0x62100001b500)
freed by thread T0 here:
    #0 0x7f1be4f702ca in __interceptor_free (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x982ca)
    #1 0x7f1be40cbe3c in _IO_default_finish (/lib/x86_64-linux-gnu/libc.so.6+0x7be3c)

previously allocated by thread T0 here:
    #0 0x7f1be4f70602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x7f1be40bd1d4 in _IO_file_doallocate (/lib/x86_64-linux-gnu/libc.so.6+0x6d1d4)

SUMMARY: AddressSanitizer: double-free ??:0 __interceptor_free
==6191==ABORTING


```

## 3. cimg-crash-1 

segmentfault for allocate failed.

```
==6194==WARNING: AddressSanitizer failed to allocate 0x001800000c00 bytes
==6194==AddressSanitizer's allocator is terminating the process instead of returning 0
==6194==If you don't like this behavior set allocator_may_return_null=1
==6194==AddressSanitizer CHECK failed: ../../../../src/libsanitizer/sanitizer_common/sanitizer_allocator.cc:147 "((0)) != (0)" (0x0, 0x0)
    #0 0x7f88deb49631  (/usr/lib/x86_64-linux-gnu/libasan.so.2+0xa0631)
    #1 0x7f88deb4e5e3 in __sanitizer::CheckFailed(char const*, int, char const*, unsigned long long, unsigned long long) (/usr/lib/x86_64-linux-gnu/libasan.so.2+0xa55e3)
    #2 0x7f88deac6425  (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x1d425)
    #3 0x7f88deb4c865  (/usr/lib/x86_64-linux-gnu/libasan.so.2+0xa3865)
    #4 0x7f88deacbb4d  (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x22b4d)
    #5 0x7f88deb4267e in operator new[](unsigned long) (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x9967e)
    #6 0x433fb6 in cimg_library::CImg<unsigned char>::assign(unsigned int, unsigned int, unsigned int, unsigned int) ../CImg.h:11379
    #7 0x49a9d5 in cimg_library::CImg<unsigned char>::_load_bmp(_IO_FILE*, char const*) ../CImg.h:48366
    #8 0x4b84a4 in cimg_library::CImg<unsigned char>::load_bmp(char const*) ../CImg.h:48280
    #9 0x4b84a4 in cimg_library::CImg<unsigned char>::load(char const*) ../CImg.h:48122
    #10 0x4022fa in cimg_library::CImg<unsigned char>::assign(char const*) ../CImg.h:11514
    #11 0x4022fa in cimg_library::CImg<unsigned char>::CImg(char const*) ../CImg.h:11161
    #12 0x4022fa in main /src/CImg/fuzz-test/bmp-test.cpp:25
    #13 0x7f88ddc4182f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)


```

## 4. others

...

