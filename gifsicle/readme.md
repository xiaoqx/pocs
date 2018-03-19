gifsicle
============


## 1. gifsicle --dither

```
$ /gifsicle --dither --use-col=bw poc-1 -o /dev/null

gifsicle:./crashes/gifsicle-dither-fuzz003:id:000000,sig:06,src:000753,op:havoc,rep:8:#1: read error: unknown block type 206 at file offset 161
gifsicle:./crashes/gifsicle-dither-fuzz003:id:000000,sig:06,src:000753,op:havoc,rep:8:#0: read error: image corrupted, min_code_size too small
gifsicle:./crashes/gifsicle-dither-fuzz003:id:000000,sig:06,src:000753,op:havoc,rep:8:#0: read error: image corrupted, code out of range (19 times)
gifsicle:./crashes/gifsicle-dither-fuzz003:id:000000,sig:06,src:000753,op:havoc,rep:8:#0: read error: (not reporting more errors)
gifsicle:./crashes/gifsicle-dither-fuzz003:id:000000,sig:06,src:000753,op:havoc,rep:8:#0: warning: 115 superfluous pixels of image data
gifsicle:./crashes/gifsicle-dither-fuzz003:id:000000,sig:06,src:000753,op:havoc,rep:8:#1: read error: image corrupted, min_code_size too small
gifsicle:./crashes/gifsicle-dither-fuzz003:id:000000,sig:06,src:000753,op:havoc,rep:8:#1: read error: image corrupted, code out of range (3 times)
gifsicle:./crashes/gifsicle-dither-fuzz003:id:000000,sig:06,src:000753,op:havoc,rep:8:#1: read error: missing 16 pixels of image data
ASAN:SIGSEGV
=================================================================
==74==ERROR: AddressSanitizer: SEGV on unknown address 0x61ae787a69f4 (pc 0x0000004ad06f bp 0x60300000ed42 sp 0x7fffffffd690 T0)
    #0 0x4ad06e in kc_distance ../../gifsicle/src/kcolor.h:113
    #1 0x4ad06e in colormap_image_floyd_steinberg ../../gifsicle/src/quantize.c:1149
    #2 0x4b3a07 in dither ../../gifsicle/src/quantize.c:1488
    #3 0x4b3a07 in colormap_stream ../../gifsicle/src/quantize.c:1613
    #4 0x4f4d3c in do_colormap_change ../../gifsicle/src/gifsicle.c:904
    #5 0x4f4d3c in merge_and_write_frames ../../gifsicle/src/gifsicle.c:1030
    #6 0x4f9116 in output_frames ../../gifsicle/src/gifsicle.c:1105
    #7 0x4096ec in main ../../gifsicle/src/gifsicle.c:2173
    #8 0x7ffff659a82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #9 0x40ae88 in _start (/src/aflbuild/installed/bin/gifsicle+0x40ae88)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV ../../gifsicle/src/kcolor.h:113 kc_distance
==74==ABORTING
```
