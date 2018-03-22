pocs of netpbm 
========================

## 1. pstopnm-divbyzero-1

A divided by zero results to dos in pstopnm tool. 
the bug is in computeSizeResBlind function because of imageHeight is zero. 


```
$ gdb --args pstopnm ./pstopnm-divbyzero-1

Starting program: /src/netpbm-trunk/converter/other/pstopnm /work/pstopnm-divbyzero-1
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

Program received signal SIGFPE, Arithmetic exception.
0x0000000000402dd6 in computeSizeResBlind (xmax=612, ymax=792, imageWidth=165, imageHeight=0, nocrop=false, imageDimP=0x7fffffffe450) at pstopnm.c:313
313         imageDimP->xres = imageDimP->yres = MIN(xmax * 72 / imageWidth,
(gdb) bt
#0  0x0000000000402dd6 in computeSizeResBlind (xmax=612, ymax=792, imageWidth=165, imageHeight=0, nocrop=false, imageDimP=0x7fffffffe450) at pstopnm.c:313
#1  0x00000000004030ab in computeSizeRes (cmdline=..., borderedBox=..., imageDimP=0x7fffffffe450) at pstopnm.c:369
#2  0x00000000004044b8 in main (argc=2, argv=0x7fffffffe5c8) at pstopnm.c:1032

```

## 2. pstopnm-divbyzero-2


```
$ gdb --args pstopnm ./pstopnm-divbyzero-2

[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
pstopnm: Writing ppmraw format

Program received signal SIGFPE, Arithmetic exception.
0x000000000040374e in writePstrans (box=..., d=..., orientation=LANDSCAPE, pipeToGsP=0x61600000f680) at pstopnm.c:633
633             llx = box.llx - (xsize * 72 / xres - (box.urx - box.llx)) / 2;
(gdb) pstopnm: execl() of Ghostscript ('/usr/bin/gs') failed, errno=2 (No such file or directory)

(gdb) bt
#0  0x000000000040374e in writePstrans (box=..., d=..., orientation=LANDSCAPE, pipeToGsP=0x61600000f680) at pstopnm.c:633
#1  0x0000000000403f5a in feedPsToGhostScript (inputFileName=0x60300000efe0 "/work/pstopnm-divbyzero-2", borderedBox=..., imageDim=..., orientation=LANDSCAPE,
    pipeToGhostscriptFd=4, language=COMMON_POSTSCRIPT) at pstopnm.c:868
#2  0x000000000040426e in executeGhostscript (inputFileName=0x60300000efe0 "/work/pstopnm-divbyzero-2", borderedBox=..., imageDim=..., orientation=LANDSCAPE,
    ghostscriptDevice=0x60200000eff0 "ppmraw", outfileArg=0x60400000dfd0 "/work/pstopnm-divbyzero-2%03d.ppm", textalphabits=4, language=COMMON_POSTSCRIPT)
    at pstopnm.c:971
#3  0x0000000000404569 in main (argc=2, argv=0x7fffffffe5c8) at pstopnm.c:1041
```

## 3. tifftopnm-heapoverflow-1

```
$ pstopnm ./tifftopnm-heapoverflow-1

==19==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x60200000ef14 at pc 0x000000405402 bp 0x7fff16aa45f0 sp 0x7fff16aa45e0
READ of size 1 at 0x60200000ef14 thread T0
    #0 0x405401  (/src/aflbuild/installed/bin/tifftopnm+0x405401)
    #1 0x406b91  (/src/aflbuild/installed/bin/tifftopnm+0x406b91)
    #2 0x403115  (/src/aflbuild/installed/bin/tifftopnm+0x403115)
    #3 0x7ffb37fb082f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #4 0x4039e8  (/src/aflbuild/installed/bin/tifftopnm+0x4039e8)

0x60200000ef14 is located 0 bytes to the right of 4-byte region [0x60200000ef10,0x60200000ef14)
allocated by thread T0 here:
    #0 0x7ffb3895e602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x4063ae  (/src/aflbuild/installed/bin/tifftopnm+0x4063ae)

SUMMARY: AddressSanitizer: heap-buffer-overflow ??:0 ??
Shadow bytes around the buggy address:
  0x0c047fff9d90: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9da0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9db0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9dc0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9dd0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
=>0x0c047fff9de0: fa fa[04]fa fa fa fd fa fa fa fd fa fa fa fd fa
  0x0c047fff9df0: fa fa fd fa fa fa fd fa fa fa fd fd fa fa 00 00
  0x0c047fff9e00: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e10: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e20: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e30: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==19==ABORTING

```
## 4. pbmmask-heapoverflow-1

```
$ gdb --args pbmmask $POC

==28==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x60200000efef at pc 0x00000040527d bp 0x7fffffffe4f0 sp 0x7fffffffe4e0
READ of size 1 at 0x60200000efef thread T0
    #0 0x40527c  (/src/aflbuild/installed/bin/pbmmask+0x40527c)
    #1 0x7ffff67c882f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #2 0x405548  (/src/aflbuild/installed/bin/pbmmask+0x405548)

0x60200000efef is located 1 bytes to the left of 1-byte region [0x60200000eff0,0x60200000eff1)
allocated by thread T0 here:
    #0 0x7ffff6f02602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x7ffff6c1d704 in mallocz /src/netpbm/lib/util/mallocvar.c:15
    #2 0x7ffff6c1d704 in allocRowHeap /src/netpbm/lib/util/mallocvar.c:68
    #3 0x7ffff6c1d704 in pm_mallocarray2 /src/netpbm/lib/util/mallocvar.c:117
    #4 0x6075bf  (/src/aflbuild/installed/bin/pbmmask+0x6075bf)

SUMMARY: AddressSanitizer: heap-buffer-overflow ??:0 ??
Shadow bytes around the buggy address:
  0x0c047fff9da0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9db0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9dc0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9dd0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9de0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
=>0x0c047fff9df0: fa fa fa fa fa fa fa fa fa fa 01 fa fa[fa]01 fa
  0x0c047fff9e00: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e10: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e20: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e30: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e40: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==28==ABORTING


```
