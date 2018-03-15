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
