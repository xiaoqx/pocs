
bugs of opencv
==================

# 1. out-of-bound write in FillColorRow4

An out of bound write error occurs when reads it by using cv::imread.

```
==14475== Copyright (C) 2002-2013, and GNU GPL'd, by Julian Seward et al.
==14475== Using Valgrind-3.10.1 and LibVEX; rerun with -h for copyright info
==14475== Command: ./gtest.elf ../../../fuzz-tests/GaussianBlur-test/out/crashes/id:000001,sig:06,src:000001,op:flip1,pos:21
==14475==
==14475== Warning: set address range perms: large range [0x3a044040, 0xfa044c88) (undefined)
==14475== Invalid write of size 4
==14475==    at 0x514CBC3: FillColorRow4(unsigned char*, unsigned char*, int, PaletteEntry*) (utils.cpp:496)
==14475==    by 0x5169284: cv::BmpDecoder::readData(cv::Mat&) (grfmt_bmp.cpp:251)
==14475==    by 0x5134A8B: cv::imread_(cv::String const&, int, int, cv::Mat*) (loadsave.cpp:454)
==14475==    by 0x5135741: cv::imread(cv::String const&, int) (loadsave.cpp:565)
==14475==    by 0x400DFA: main (33_GaussianBlur.cpp:34)
==14475==  Address 0xfffffffff4044c20 is not stack'd, malloc'd or (recently) free'd
==14475==
==14475==
==14475== Process terminating with default action of signal 11 (SIGSEGV)
==14475==  Access not within mapped region at address 0xFFFFFFFFF4044C20
==14475==    at 0x514CBC3: FillColorRow4(unsigned char*, unsigned char*, int, PaletteEntry*) (utils.cpp:496)
==14475==    by 0x5169284: cv::BmpDecoder::readData(cv::Mat&) (grfmt_bmp.cpp:251)
==14475==    by 0x5134A8B: cv::imread_(cv::String const&, int, int, cv::Mat*) (loadsave.cpp:454)
==14475==    by 0x5135741: cv::imread(cv::String const&, int) (loadsave.cpp:565)
==14475==    by 0x400DFA: main (33_GaussianBlur.cpp:34)
==14475==  If you believe this happened as a result of a stack
==14475==  overflow in your program's main thread (unlikely but
==14475==  possible), you can try to increase the size of the
==14475==  main thread stack using the --main-stacksize= flag.
==14475==  The main thread stack size used in this run was 8388608.
==14475==
==14475== HEAP SUMMARY:
==14475==     in use at exit: 3,238,103,506 bytes in 397 blocks
==14475==   total heap usage: 458 allocs, 61 frees, 3,238,113,514 bytes allocated
==14475==
==14475== LEAK SUMMARY:
==14475==    definitely lost: 0 bytes in 0 blocks
==14475==    indirectly lost: 0 bytes in 0 blocks
==14475==      possibly lost: 3,221,236,783 bytes in 114 blocks
==14475==    still reachable: 16,866,723 bytes in 283 blocks
==14475==         suppressed: 0 bytes in 0 blocks
==14475== Rerun with --leak-check=full to see details of leaked memory
==14475==
==14475== For counts of detected and suppressed errors, rerun with: -v
==14475== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)
Segmentation fault

```

# 2. A heap-based buf overflow results to invalid write in fseek.

```
==25260== Memcheck, a memory error detector
==25260== Copyright (C) 2002-2013, and GNU GPL'd, by Julian Seward et al.
==25260== Using Valgrind-3.10.1 and LibVEX; rerun with -h for copyright info
==25260== Command: ./gtest.elf ../../../fuzz-tests/GaussianBlur-test/out/crashes/id:000002,sig:06,src:000001,op:flip1,pos:47
==25260==
==25260== Invalid write of size 2
==25260==    at 0x4C2F7E3: memcpy@@GLIBC_2.14 (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==25260==    by 0x5178F29: cv::RLByteStream::getBytes(void*, int) (bitstrm.cpp:235)
==25260==    by 0x51683B8: cv::BmpDecoder::readHeader() (grfmt_bmp.cpp:122)
==25260==    by 0x5134548: cv::imread_(cv::String const&, int, int, cv::Mat*) (loadsave.cpp:412)
==25260==    by 0x5135741: cv::imread(cv::String const&, int) (loadsave.cpp:565)
==25260==    by 0x400DFA: main (33_GaussianBlur.cpp:34)
==25260==  Address 0xecb66c0 is 0 bytes after a block of size 1,264 alloc'd
==25260==    at 0x4C2B0E0: operator new(unsigned long) (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==25260==    by 0x513AD5C: cv::Ptr<cv::BmpDecoder> cv::makePtr<cv::BmpDecoder>() (ptr.inl.hpp:301)
==25260==    by 0x5167CE3: cv::BmpDecoder::newDecoder() const (grfmt_bmp.cpp:76)
==25260==    by 0x51322DB: cv::findDecoder(cv::String const&) (loadsave.cpp:199)
==25260==    by 0x5134301: cv::imread_(cv::String const&, int, int, cv::Mat*) (loadsave.cpp:384)
==25260==    by 0x5135741: cv::imread(cv::String const&, int) (loadsave.cpp:565)
==25260==    by 0x400DFA: main (33_GaussianBlur.cpp:34)
==25260==
==25260== Source and destination overlap in memcpy(0xecb676a, 0xecb67f0, 632)
==25260==    at 0x4C2F71C: memcpy@@GLIBC_2.14 (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==25260==    by 0x5178F29: cv::RLByteStream::getBytes(void*, int) (bitstrm.cpp:235)
==25260==    by 0x51683B8: cv::BmpDecoder::readHeader() (grfmt_bmp.cpp:122)
==25260==    by 0x5134548: cv::imread_(cv::String const&, int, int, cv::Mat*) (loadsave.cpp:412)
==25260==    by 0x5135741: cv::imread(cv::String const&, int) (loadsave.cpp:565)
==25260==    by 0x400DFA: main (33_GaussianBlur.cpp:34)
==25260==
==25260== Invalid read of size 8
==25260==    at 0x74928F5: fseek (fseek.c:38)
==25260==    by 0x51784A6: cv::RBaseStream::readBlock() (bitstrm.cpp:104)
==25260==    by 0x5178F9E: cv::RLByteStream::getBytes(void*, int) (bitstrm.cpp:233)
==25260==    by 0x51683B8: cv::BmpDecoder::readHeader() (grfmt_bmp.cpp:122)
==25260==    by 0x5134548: cv::imread_(cv::String const&, int, int, cv::Mat*) (loadsave.cpp:412)
==25260==    by 0x5135741: cv::imread(cv::String const&, int) (loadsave.cpp:565)
==25260==    by 0x400DFA: main (33_GaussianBlur.cpp:34)
==25260==  Address 0x33333333333303b is not stack'd, malloc'd or (recently) free'd
==25260==
==25260==
==25260== Process terminating with default action of signal 11 (SIGSEGV)
==25260==  General Protection Fault
==25260==    at 0x74928F5: fseek (fseek.c:38)
==25260==    by 0x51784A6: cv::RBaseStream::readBlock() (bitstrm.cpp:104)
==25260==    by 0x5178F9E: cv::RLByteStream::getBytes(void*, int) (bitstrm.cpp:233)
==25260==    by 0x51683B8: cv::BmpDecoder::readHeader() (grfmt_bmp.cpp:122)
==25260==    by 0x5134548: cv::imread_(cv::String const&, int, int, cv::Mat*) (loadsave.cpp:412)
==25260==    by 0x5135741: cv::imread(cv::String const&, int) (loadsave.cpp:565)
==25260==    by 0x400DFA: main (33_GaussianBlur.cpp:34)
==25260== Invalid read of size 8
==25260==    at 0x749CF52: _IO_flush_all_lockp (genops.c:844)
==25260==    by 0x749D0C9: _IO_cleanup (genops.c:1013)
==25260==    by 0x7589DBA: __libc_freeres (in /lib/x86_64-linux-gnu/libc-2.19.so)
==25260==    by 0x4A256BC: _vgnU_freeres (in /usr/lib/valgrind/vgpreload_core-amd64-linux.so)
==25260==    by 0xFFF0002DF: ???
==25260==  Address 0x3311110000030018 is not stack'd, malloc'd or (recently) free'd
==25260==
==25260==
==25260== Process terminating with default action of signal 11 (SIGSEGV)
==25260==  General Protection Fault
==25260==    at 0x749CF52: _IO_flush_all_lockp (genops.c:844)
==25260==    by 0x749D0C9: _IO_cleanup (genops.c:1013)
==25260==    by 0x7589DBA: __libc_freeres (in /lib/x86_64-linux-gnu/libc-2.19.so)
==25260==    by 0x4A256BC: _vgnU_freeres (in /usr/lib/valgrind/vgpreload_core-amd64-linux.so)
==25260==    by 0xFFF0002DF: ???
==25260==
==25260== HEAP SUMMARY:
==25260==     in use at exit: 97,530 bytes in 393 blocks
==25260==   total heap usage: 454 allocs, 61 frees, 107,538 bytes allocated
==25260==
==25260== LEAK SUMMARY:
==25260==    definitely lost: 0 bytes in 0 blocks
==25260==    indirectly lost: 0 bytes in 0 blocks
==25260==      possibly lost: 8,167 bytes in 113 blocks
==25260==    still reachable: 89,363 bytes in 280 blocks
==25260==         suppressed: 0 bytes in 0 blocks
==25260== Rerun with --leak-check=full to see details of leaked memory
==25260==
==25260== For counts of detected and suppressed errors, rerun with: -v
==25260== ERROR SUMMARY: 260 errors from 4 contexts (suppressed: 0 from 0)
Segmentation fault
```
