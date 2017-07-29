
OpenEXR
=========================

# exrmaketiled

./exrmaketiled ./bug-testcase/185-openexr-heapoverflow /tmp/out
```
=================================================================
==18567==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x63100003c7fe at pc 0x7fc23e95a5ab bp 0x7ffe0428e7e0 sp 0x7ffe0428e7d8
READ of size 2 at 0x63100003c7fe thread T0
    #0 0x7fc23e95a5aa in hufDecode ../../openexr/OpenEXR/IlmImf/ImfHuf.cpp:898
    #1 0x7fc23e95a5aa in Imf_2_2::hufUncompress(char const*, int, unsigned short*, int) ../../openexr/OpenEXR/IlmImf/ImfHuf.cpp:1101
    #2 0x7fc23e971ca7 in Imf_2_2::PizCompressor::uncompress(char const*, int, Imath_2_2::Box<Imath_2_2::Vec2<int> >, char const*&) ../../openexr/OpenEXR/IlmImf/ImfPizCompressor.cpp:576
    #3 0x7fc23e974663 in Imf_2_2::PizCompressor::uncompress(char const*, int, int, char const*&) ../../openexr/OpenEXR/IlmImf/ImfPizCompressor.cpp:288
    #4 0x7fc23ea66bb7 in execute ../../openexr/OpenEXR/IlmImf/ImfScanLineInputFile.cpp:544
    #5 0x7fc23d4081ea in IlmThread_2_2::ThreadPool::addTask(IlmThread_2_2::Task*) ../../openexr/IlmBase/IlmThread/IlmThreadPool.cpp:433
    #6 0x7fc23ea7c136 in Imf_2_2::ScanLineInputFile::readPixels(int, int) ../../openexr/OpenEXR/IlmImf/ImfScanLineInputFile.cpp:1617
    #7 0x7fc23e904c7c in Imf_2_2::InputFile::readPixels(int, int) ../../openexr/OpenEXR/IlmImf/ImfInputFile.cpp:815
    #8 0x4236ed in makeTiled(char const*, char const*, int, Imf_2_2::LevelMode, Imf_2_2::LevelRoundingMode, Imf_2_2::Compression, int, int, std::set<std::string, std::less<std::string>, std::allocator<std::string> > const&, Extrapolation, Extrapolation, bool) ../../openexr/OpenEXR/exrmaketiled/makeTiled.cpp:572
    #9 0x405283 in main ../../openexr/OpenEXR/exrmaketiled/main.cpp:426
    #10 0x7fc23dd55f44 in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x21f44)
    #11 0x40692c (/data/xqx/tests/openexr-test/aflbuild/build-openexr/install/bin/exrmaketiled+0x40692c)

0x63100003c7fe is located 2 bytes to the left of 76800-byte region [0x63100003c800,0x63100004f400)
allocated by thread T0 here:
    #0 0x7fc23f54a27f in operator new[](unsigned long) (/usr/lib/x86_64-linux-gnu/libasan.so.1+0x5527f)
    #1 0x7fc23e96adf0 in Imf_2_2::PizCompressor::PizCompressor(Imf_2_2::Header const&, unsigned long, unsigned long) ../../openexr/OpenEXR/IlmImf/ImfPizCompressor.cpp:194

SUMMARY: AddressSanitizer: heap-buffer-overflow ../../openexr/OpenEXR/IlmImf/ImfHuf.cpp:898 hufDecode
Shadow bytes around the buggy address:
  0x0c627ffff8a0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c627ffff8b0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c627ffff8c0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c627ffff8d0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c627ffff8e0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
=>0x0c627ffff8f0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa[fa]
  0x0c627ffff900: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c627ffff910: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c627ffff920: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c627ffff930: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c627ffff940: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
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
  Contiguous container OOB:fc
  ASan internal:           fe
==18567==ABORTING
```


results as following by using valgrind 

```
valgrind ./exrmaketiled ../../../../../bug-testcase/185-openexr-heapoverflow /tmp/out
==18448== Memcheck, a memory error detector
==18448== Copyright (C) 2002-2013, and GNU GPL'd, by Julian Seward et al.
==18448== Using Valgrind-3.10.1 and LibVEX; rerun with -h for copyright info
==18448== Command: ./exrmaketiled ../../../../../bug-testcase/185-openexr-heapoverflow /tmp/out
==18448==
==18448== Invalid read of size 2
==18448==    at 0x52FBDCB: hufDecode (ImfHuf.cpp:898)
==18448==    by 0x52FBDCB: Imf_2_2::hufUncompress(char const*, int, unsigned short*, int) (ImfHuf.cpp:1101)
==18448==    by 0x52FDB29: Imf_2_2::PizCompressor::uncompress(char const*, int, Imath_2_2::Box<Imath_2_2::Vec2<int> >, char const*&) (ImfPizCompressor.cpp:576)
==18448==    by 0x52FDF26: Imf_2_2::PizCompressor::uncompress(char const*, int, int, char const*&) (ImfPizCompressor.cpp:288)
==18448==    by 0x532099C: Imf_2_2::(anonymous namespace)::LineBufferTask::execute() (ImfScanLineInputFile.cpp:544)
==18448==    by 0x649B708: IlmThread_2_2::ThreadPool::addTask(IlmThread_2_2::Task*) (IlmThreadPool.cpp:433)
==18448==    by 0x5323DDA: Imf_2_2::ScanLineInputFile::readPixels(int, int) (ImfScanLineInputFile.cpp:1617)
==18448==    by 0x52F1445: Imf_2_2::InputFile::readPixels(int, int) (ImfInputFile.cpp:815)
==18448==    by 0x4094D3: makeTiled(char const*, char const*, int, Imf_2_2::LevelMode, Imf_2_2::LevelRoundingMode, Imf_2_2::Compression, int, int, std::set<std::string, std::less<std::string>, std::allocator<std::string> > const&, Extrapolation, Extrapolation, bool) (makeTiled.cpp:572)
==18448==    by 0x403F02: main (main.cpp:426)
==18448==  Address 0x6a3effe is 2 bytes before a block of size 76,800 alloc'd
==18448==    at 0x4C2B800: operator new[](unsigned long) (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==18448==    by 0x52FD6E7: Imf_2_2::PizCompressor::PizCompressor(Imf_2_2::Header const&, unsigned long, unsigned long) (ImfPizCompressor.cpp:194)
==18448==    by 0x52FCC3F: Imf_2_2::newCompressor(Imf_2_2::Compression, unsigned long, Imf_2_2::Header const&) (ImfCompressor.cpp:148)
==18448==    by 0x5324910: Imf_2_2::ScanLineInputFile::initialize(Imf_2_2::Header const&) (ImfScanLineInputFile.cpp:1120)
==18448==    by 0x5324C3B: Imf_2_2::ScanLineInputFile::ScanLineInputFile(Imf_2_2::InputPartData*) (ImfScanLineInputFile.cpp:1166)
==18448==    by 0x52F0324: Imf_2_2::InputFile::initialize() (ImfInputFile.cpp:592)
==18448==    by 0x52F0867: Imf_2_2::InputFile::InputFile(Imf_2_2::InputPartData*) (ImfInputFile.cpp:477)
==18448==    by 0x534542B: Imf_2_2::InputFile* Imf_2_2::MultiPartInputFile::getInputPart<Imf_2_2::InputFile>(int) (ImfMultiPartInputFile.cpp:185)
==18448==    by 0x5345E7D: Imf_2_2::InputPart::InputPart(Imf_2_2::MultiPartInputFile&, int) (ImfInputPart.cpp:44)
==18448==    by 0x409174: makeTiled(char const*, char const*, int, Imf_2_2::LevelMode, Imf_2_2::LevelRoundingMode, Imf_2_2::Compression, int, int, std::set<std::string, std::less<std::string>, std::allocator<std::string> > const&, Extrapolation, Extrapolation, bool) (makeTiled.cpp:535)
==18448==    by 0x403F02: main (main.cpp:426)
==18448==
Error reading pixel data from image file "../../../../../bug-testcase/185-openexr-heapoverflow". Error in Huffman-encoded data (invalid code).
==18448==
==18448== HEAP SUMMARY:
==18448==     in use at exit: 74,192 bytes in 31 blocks
==18448==   total heap usage: 551 allocs, 520 frees, 3,823,049 bytes allocated
==18448==
==18448== LEAK SUMMARY:
==18448==    definitely lost: 0 bytes in 0 blocks
==18448==    indirectly lost: 0 bytes in 0 blocks
==18448==      possibly lost: 0 bytes in 0 blocks
==18448==    still reachable: 74,192 bytes in 31 blocks
==18448==         suppressed: 0 bytes in 0 blocks
==18448== Rerun with --leak-check=full to see details of leaked memory
==18448==
==18448== For counts of detected and suppressed errors, rerun with: -v
==18448== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)
```

