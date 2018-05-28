POC of CMS
=====================




## 1-cms-out-bound-write-PrecalculateXFORM


```

gdb --args tificc $POC /tmp/out.tiff

Program received signal SIGSEGV, Segmentation fault.
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffe838 --> 0x7ffff7b21c2c (<PrecalculatedXFORM+812>:      mov    r11,QWORD PTR [rbx+0x70])
0008| 0x7fffffffe840 --> 0x3c95700000000000
0016| 0x7fffffffe848 --> 0x102000000b10
0024| 0x7fffffffe850 --> 0x0
0032| 0x7fffffffe858 --> 0x62e610 --> 0x4000200010000
0040| 0x7fffffffe860 --> 0x614820 --> 0x0
0048| 0x7fffffffe868 --> 0x100000000
0056| 0x7fffffffe870 --> 0x0
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x00007ffff7000000 in ?? ()
#0  0x00007ffff7000000 in ?? ()
#1  0x00007ffff7b21c2c in PrecalculatedXFORM (p=0x616940, in=0x62e610, out=0x614820, PixelsPerLine=0x1020, LineCount=0x1, Stride=0x7fffffffe900) at ../../cms/src/cmsxform.c:410
#2  0x00007ffff7b25285 in cmsDoTransform (Transform=Transform@entry=0x616940, InputBuffer=InputBuffer@entry=0x62e610, OutputBuffer=OutputBuffer@entry=0x614820, Size=Size@entry=0x1020) at ../.
./cms/src/cmsxform.c:189
#3  0x0000000000405ac7 in TileBasedXform (nPlanes=0x1, out=0x6124d0, in=<optimized out>, hXForm=0x616940) at ../../../cms/utils/tificc/tificc.c:408
#4  TransformImage (cDefInpProf=<optimized out>, out=<optimized out>, in=<optimized out>) at ../../../cms/utils/tificc/tificc.c:904
#5  main (argc=argc@entry=0x3, argv=argv@entry=0x7fffffffeba8) at ../../../cms/utils/tificc/tificc.c:1167
#6  0x00007ffff71fe830 in __libc_start_main (main=0x402360 <main>, argc=0x3, argv=0x7fffffffeba8, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffeb
98) at ../csu/libc-start.c:291
#7  0x0000000000408e29 in _start ()

__main__:99: UserWarning: GDB v7.11 may not support required Python API
Description: Segmentation fault on program counter
Short description: SegFaultOnPc (3/22)
Hash: a623b76741d0ee9936f43614a95f8a38.d3ce2561115efa40618f8e7190e9ac1e
Exploitability Classification: EXPLOITABLE
Explanation: The target tried to access data at an address that matches the program counter. This is likely due to the execution of a branch instruction (ex: 'call') with a bad argument, but
it could also be due to execution continuing past the end of a memory region or another cause. Regardless this likely indicates that the program counter contents are tainted and can be contro
lled by an attacker.
Other tags: AccessViolation (21/22)


```



## 2.tiff-crash-TIFFWriteEncodeTile

这个crash在libtiff的TIFFWriteEncodeTile函数中，需要在最新版中分析。

```
Stopped reason: SIGABRT
0x00007ffff733f428 in __GI_raise (sig=sig@entry=0x6) at ../sysdeps/unix/sysv/linux/raise.c:54
54      ../sysdeps/unix/sysv/linux/raise.c: No such file or directory.
gdb-peda$ bt
#0  0x00007ffff733f428 in __GI_raise (sig=sig@entry=0x6) at ../sysdeps/unix/sysv/linux/raise.c:54
#1  0x00007ffff734102a in __GI_abort () at abort.c:89
#2  0x00007ffff73817ea in __libc_message (do_abort=0x2, fmt=fmt@entry=0x7ffff749aed8 "*** Error in `%s': %s: 0x%s ***\n") at ../sysdeps/posix/libc_fatal.c:175
#3  0x00007ffff738c13e in malloc_printerr (ar_ptr=0x7ffff76ceb20 <main_arena>, ptr=0x60d430, str=0x7ffff7497d3f "malloc(): memory corruption", action=<optimized out>) at malloc.c:5006
#4  _int_malloc (av=av@entry=0x7ffff76ceb20 <main_arena>, bytes=bytes@entry=0x2000) at malloc.c:3474
#5  0x00007ffff738e184 in __GI___libc_malloc (bytes=0x2000) at malloc.c:2913
#6  0x00007ffff7933af9 in TIFFWriteBufferSetup () from /usr/lib/x86_64-linux-gnu/libtiff.so.5
#7  0x00007ffff79343b1 in TIFFWriteEncodedTile () from /usr/lib/x86_64-linux-gnu/libtiff.so.5
#8  0x0000000000402db6 in TileBasedXform (hXForm=0x60e940, in=0x609860, out=0x60a4d0, nPlanes=0x1) at ../../../cms/utils/tificc/tificc.c:412
#9  0x00000000004044db in TransformImage (in=0x609860, out=0x60a4d0, cDefInpProf=0x0) at ../../../cms/utils/tificc/tificc.c:904
#10 0x0000000000404d86 in main (argc=0x3, argv=0x7fffffffe608) at ../../../cms/utils/tificc/tificc.c:1167
#11 0x00007ffff732a830 in __libc_start_main (main=0x404c38 <main>, argc=0x3, argv=0x7fffffffe608, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffe5f8) at ../csu/libc-start.c:291
#12 0x0000000000401fe9 in _start ()


```


## 3-cms-NULL-Pointer-cmsEvalToneCurve16


```
gdb --args tificc $POC /tmp/out.tiff

Program received signal SIGSEGV, Segmentation fault.
[----------------------------------registers-----------------------------------]
RAX: 0x0
RBX: 0x7fffffffde94 --> 0x0
RCX: 0x8
RDX: 0x0
RSI: 0x0
RDI: 0x60e100 --> 0x0
RBP: 0x7fffffffdde0 --> 0x7fffffffde10 --> 0x7fffffffde60 --> 0x7fffffffe2b0 --> 0x7fffffffe360 --> 0x7fffffffe3c0 (--> ...)
RSP: 0x7fffffffddc0 --> 0xffffdde0
RIP: 0x7ffff7b78259 (<cmsEvalToneCurve16+78>:   mov    rax,QWORD PTR [rax+0x80])
R8 : 0x2ce
R9 : 0x7fffffffe3a0 --> 0x0
R10: 0x9d
R11: 0x7ffff7b7820b (<cmsEvalToneCurve16>:      push   rbp)
R12: 0x60e10b --> 0x60c6600000000000
R13: 0x7fffffffe630 --> 0x3
R14: 0x0
R15: 0x0
EFLAGS: 0x10206 (carry PARITY adjust zero sign trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
   0x7ffff7b7824d <cmsEvalToneCurve16+66>:      call   0x7ffff7b6fa60 <__assert_fail@plt>
   0x7ffff7b78252 <cmsEvalToneCurve16+71>:      mov    rax,QWORD PTR [rbp-0x18]
   0x7ffff7b78256 <cmsEvalToneCurve16+75>:      mov    rax,QWORD PTR [rax]
=> 0x7ffff7b78259 <cmsEvalToneCurve16+78>:      mov    rax,QWORD PTR [rax+0x80]
   0x7ffff7b78260 <cmsEvalToneCurve16+85>:      mov    rdx,QWORD PTR [rbp-0x18]
   0x7ffff7b78264 <cmsEvalToneCurve16+89>:      mov    rdx,QWORD PTR [rdx]
   0x7ffff7b78267 <cmsEvalToneCurve16+92>:      lea    rsi,[rbp-0xa]
   0x7ffff7b7826b <cmsEvalToneCurve16+96>:      lea    rcx,[rbp-0x1c]
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffddc0 --> 0xffffdde0
0008| 0x7fffffffddc8 --> 0x60e100 --> 0x0
0016| 0x7fffffffddd0 --> 0x7fffffffde94 --> 0x0
0024| 0x7fffffffddd8 --> 0x286060b593741800
0032| 0x7fffffffdde0 --> 0x7fffffffde10 --> 0x7fffffffde60 --> 0x7fffffffe2b0 --> 0x7fffffffe360 --> 0x7fffffffe3c0 (--> ...)
0040| 0x7fffffffdde8 --> 0x7ffff7b781d2 (<cmsEvalToneCurveFloat+110>:   mov    WORD PTR [rbp-0x2],ax)
0048| 0x7fffffffddf0 --> 0xffffde10
0056| 0x7fffffffddf8 --> 0x60e100 --> 0x0
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x00007ffff7b78259 in cmsEvalToneCurve16 (Curve=0x60e100, v=0x0) at ../../cms/src/cmsgamma.c:1376
1376        Curve ->InterpParams ->Interpolation.Lerp16(&v, &out, Curve ->InterpParams);
gdb-peda$ p Curve->InterpParams
$1 = (cmsInterpParams *) 0x0


```

## 4-cms-invalid-access-AllocateToneCurveStruct

```

gdb --args tificc $POC /tmp/out.tiff

Program received signal SIGSEGV, Segmentation fault.
[----------------------------------registers-----------------------------------]
RAX: 0xfffffffc
RBX: 0x7fffffffdea8 --> 0x7fff00000000
RCX: 0xffffffffffffffff
RDX: 0x7ffff7b74c00 (<AllocateToneCurveStruct+1122>:    rex.RB loopne 0x7ffff7b74c4b <AllocateToneCurveStruct+1197>)
RSI: 0x60e6c0 --> 0x4003333333333333
RDI: 0xfffffffc
RBP: 0x7fffffffddf0 --> 0x7fffffffde20 --> 0x7fffffffde70 --> 0x7fffffffe2c0 --> 0x7fffffffe370 --> 0x7fffffffe3d0 (--> ...)
RSP: 0x7fffffffdda8 --> 0x7ffff7b75ecb (<EvalSegmentedFn+621>:  movq   rax,xmm0)
RIP: 0x7ffff7b74c4b (<AllocateToneCurveStruct+1197>:    (bad))
R8 : 0x2ce
R9 : 0x7fffffffe3b0 --> 0x0
R10: 0x15b
R11: 0x7ffff7b97389 (<cmsDoTransform>:  push   rbp)
R12: 0x60c141 --> 0xb800007ffff7b74c
R13: 0x7fffffffe640 --> 0x3
R14: 0x0
R15: 0x0
EFLAGS: 0x10202 (carry parity adjust zero sign trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
=> 0x7ffff7b74c4b <AllocateToneCurveStruct+1197>:       (bad)
   0x7ffff7b74c4c <AllocateToneCurveStruct+1198>:       je     0x7ffff7b74c65 <AllocateToneCurveStruct+1223>
   0x7ffff7b74c4e <AllocateToneCurveStruct+1200>:       mov    rax,QWORD PTR [rbp-0x20]
   0x7ffff7b74c52 <AllocateToneCurveStruct+1204>:       mov    rdx,QWORD PTR [rax+0x20]
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffdda8 --> 0x7ffff7b75ecb (<EvalSegmentedFn+621>: movq   rax,xmm0)
0008| 0x7fffffffddb0 --> 0x1
0016| 0x7fffffffddb8 --> 0x0
0024| 0x7fffffffddc0 --> 0x0
0032| 0x7fffffffddc8 --> 0x609df0 --> 0x60c180 --> 0x0
0040| 0x7fffffffddd0 --> 0x4237ffff80018000
0048| 0x7fffffffddd8 --> 0xc9cc2500
0056| 0x7fffffffdde0 --> 0x0
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x00007ffff7b74c4b in AllocateToneCurveStruct (ContextID=0x0, nEntries=0x0, nSegments=0x1, Segments=0x7ffff7b75ecb <EvalSegmentedFn+621>, Values=0x7fffffffddf0) at ../../cms/src/cmsgamma.c:304
304         if (p -> Evals) _cmsFree(ContextID, p -> Evals);
gdb-peda$ p p
$1 = (cmsToneCurve *) 0x4237ffff80018000
gdb-peda$ p *p
Cannot access memory at address 0x4237ffff80018000
```


## 5-cms-invalid-access-cmsReadHeader


```
gdb --args tificc $POC /tmp/out.tiff
Program received signal SIGSEGV, Segmentation fault.
0x00007ffff7b80000 in _cmsReadHeader (Icc=0x1b7d90508ff21000) at ../../cms/src/cmsio0.c:699
699             cmsSignalError(Icc ->ContextID, cmsERROR_BAD_SIGNATURE, "not an ICC profile, invalid signature");
[----------------------------------registers-----------------------------------]
RAX: 0x7ffff7b80000 (<_cmsReadHeader+150>:      add    BYTE PTR [rax],al)
RBX: 0x7ffff4d764dc --> 0x0
RCX: 0x1fe020
RDX: 0x7ffff4d764dc --> 0x0
RSI: 0x7fffffffe2d0 --> 0x0
RDI: 0x60c410 --> 0x0
RBP: 0x7fffffffe330 --> 0x7fffffffe390 --> 0x7fffffffe400 --> 0x7fffffffe4f0 --> 0x7fffffffe520 --> 0x405ad0 (<__libc_csu_init>:        push   r15)
RSP: 0x7fffffffe288 --> 0x7ffff7b97b36 (<PrecalculatedXFORM+249>:       mov    rbx,rax)
RIP: 0x7ffff7b80000 (<_cmsReadHeader+150>:      add    BYTE PTR [rax],al)
R8 : 0x2ce
R9 : 0x7fffffffe370 --> 0x0
R10: 0x15b
R11: 0x7ffff7b97389 (<cmsDoTransform>:  push   rbp)
R12: 0x60c422 --> 0x8ee00007ffff7b8
R13: 0x7fffffffe600 --> 0x3
R14: 0x0
R15: 0x0
EFLAGS: 0x10287 (CARRY PARITY adjust zero SIGN trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
=> 0x7ffff7b80000 <_cmsReadHeader+150>: add    BYTE PTR [rax],al
   0x7ffff7b80002 <_cmsReadHeader+152>: add    al,ch
   0x7ffff7b80004 <_cmsReadHeader+154>: push   0xffffffffb8fffefb
   0x7ffff7b80009 <_cmsReadHeader+159>: add    BYTE PTR [rax],al
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffe288 --> 0x7ffff7b97b36 (<PrecalculatedXFORM+249>:      mov    rbx,rax)
0008| 0x7fffffffe290 --> 0x0
0016| 0x7fffffffe298 --> 0x7fffffffe370 --> 0x0
0024| 0x7fffffffe2a0 --> 0x1fe02000000001
0032| 0x7fffffffe2a8 --> 0x60aee0 --> 0x0
0040| 0x7fffffffe2b0 --> 0x7ffff4d71010 --> 0x0
0048| 0x7fffffffe2b8 --> 0x60c410 --> 0x0
0056| 0x7fffffffe2c0 --> 0x71600000000
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
gdb-peda$ bt
#0  0x00007ffff7b80000 in _cmsReadHeader (Icc=0x1b7d90508ff21000) at ../../cms/src/cmsio0.c:699
#1  0x00007ffff7b973f7 in cmsDoTransform (Transform=0x60c410, InputBuffer=0x7ffff4d71010, OutputBuffer=0x60aee0, Size=0x1fe020) at ../../cms/src/cmsxform.c:189
#2  0x0000000000402d73 in TileBasedXform (hXForm=0x60c410, in=0x609860, out=0x60a5c0, nPlanes=0x1) at ../../../cms/utils/tificc/tificc.c:408
#3  0x00000000004044db in TransformImage (in=0x609860, out=0x60a5c0, cDefInpProf=0x0) at ../../../cms/utils/tificc/tificc.c:904
#4  0x0000000000404d86 in main (argc=0x3, argv=0x7fffffffe608) at ../../../cms/utils/tificc/tificc.c:1167
#5  0x00007ffff732a830 in __libc_start_main (main=0x404c38 <main>, argc=0x3, argv=0x7fffffffe608, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffe5f8) at ../csu/libc-start.c:291
#6  0x0000000000401fe9 in _start ()
gdb-peda$ p Icc
$3635 = (_cmsICCPROFILE *) 0x1b7d90508ff21000
gdb-peda$ p Icc->ContextID
Cannot access memory at address 0x1b7d90508ff21008


```


## 6-cms-invalid-access-Pack3Bytes


```

gdb --args tificc $POC /tmp/out.tiff
Program received signal SIGSEGV, Segmentation fault.
[----------------------------------registers-----------------------------------]
RAX: 0x643001
RBX: 0x7ffff5f94fd1 --> 0x0
RCX: 0x0
RDX: 0x643000 ('')
RSI: 0x0
RDI: 0x60c8f0 --> 0x4001900040091
RBP: 0x7fffffffe280 --> 0x7fffffffe330 --> 0x7fffffffe390 --> 0x7fffffffe400 --> 0x7fffffffe4f0 --> 0x7fffffffe520 (--> ...)
RSP: 0x7fffffffe280 --> 0x7fffffffe330 --> 0x7fffffffe390 --> 0x7fffffffe400 --> 0x7fffffffe4f0 --> 0x7fffffffe520 (--> ...)
RIP: 0x7ffff7b9095a (<Pack3Bytes+108>:  mov    BYTE PTR [rdx],cl)
R8 : 0x1
R9 : 0x7fffffffe370 --> 0x0
R10: 0x15b
R11: 0x7ffff7b97389 (<cmsDoTransform>:  push   rbp)
R12: Cannot access memory address
R13: 0x7fffffffe600 --> 0x3
R14: 0x0
R15: 0x0
EFLAGS: 0x10247 (CARRY PARITY adjust ZERO sign trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
   0x7ffff7b9094b <Pack3Bytes+93>:      imul   ecx,ecx,0xff01
   0x7ffff7b90951 <Pack3Bytes+99>:      add    ecx,0x800000
   0x7ffff7b90957 <Pack3Bytes+105>:     shr    ecx,0x18
=> 0x7ffff7b9095a <Pack3Bytes+108>:     mov    BYTE PTR [rdx],cl
   0x7ffff7b9095c <Pack3Bytes+110>:     pop    rbp
   0x7ffff7b9095d <Pack3Bytes+111>:     ret
   0x7ffff7b9095e <Pack3BytesOptimized>:        push   rbp
   0x7ffff7b9095f <Pack3BytesOptimized+1>:      mov    rbp,rsp
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffe280 --> 0x7fffffffe330 --> 0x7fffffffe390 --> 0x7fffffffe400 --> 0x7fffffffe4f0 --> 0x7fffffffe520 (--> ...)
0008| 0x7fffffffe288 --> 0x7ffff7b97b7f (<PrecalculatedXFORM+322>:      mov    r12,rax)
0016| 0x7fffffffe290 --> 0x0
0024| 0x7fffffffe298 --> 0x7fffffffe370 --> 0x0
0032| 0x7fffffffe2a0 --> 0x1fe02000000001
0040| 0x7fffffffe2a8 --> 0x60d040 --> 0x0
0048| 0x7fffffffe2b0 --> 0x7ffff5f5f010 --> 0x0
0056| 0x7fffffffe2b8 --> 0x60c8f0 --> 0x4001900040091
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x00007ffff7b9095a in Pack3Bytes (info=0x60c8f0, wOut=0x0, output=0x643001 <error: Cannot access memory at address 0x643001>, Stride=0x0) at ../../cms/src/cmspack.c:1834
1834        *output++ = FROM_16_TO_8(wOut[2]);
gdb-peda$ p wOut[2]
Cannot access memory at address 0x4



```


## 7-cms-null-pointer-cmsPipelineCheckAndRetreiveStages

```
gdb --args tificc $POC /tmp/out.tiff
Program received signal SIGILL, Illegal instruction.
[----------------------------------registers-----------------------------------]
RAX: 0x7ffff7b85252 (<cmsPipelineCheckAndRetreiveStages+297>:   sbb    al,0xff)
RBX: 0x620c44 --> 0x52d552c552b552a5
RCX: 0x3030 ('00')
RDX: 0x620c44 --> 0x52d552c552b552a5
RSI: 0x7fffffffe2d0 --> 0x529552855275
RDI: 0x60c430 ("QQQQQ", 'R' <repeats 13 times>, "\270\367\377\177")
RBP: 0x7fffffffe330 --> 0x7fffffffe390 --> 0x7fffffffe400 --> 0x7fffffffe4f0 --> 0x7fffffffe520 --> 0x405ad0 (<__libc_csu_init>:        push   r15)
RSP: 0x7fffffffe288 --> 0x7ffff7b97b36 (<PrecalculatedXFORM+249>:       mov    rbx,rax)
RIP: 0x7ffff7b85254 (<cmsPipelineCheckAndRetreiveStages+299>:   (bad))
R8 : 0x1
R9 : 0x7fffffffe370 --> 0x0
R10: 0x15b
R11: 0x7ffff7b97389 (<cmsDoTransform>:  push   rbp)
R12: 0x60c442 --> 0x8ee00007ffff7b8
R13: 0x7fffffffe600 --> 0x3
R14: 0x0
R15: 0x0
EFLAGS: 0x10213 (CARRY parity ADJUST zero sign trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
=> 0x7ffff7b85254 <cmsPipelineCheckAndRetreiveStages+299>:      (bad)
   0x7ffff7b85255 <cmsPipelineCheckAndRetreiveStages+300>:      push   QWORD PTR [rdx+rcx*1-0x48]
   0x7ffff7b85259 <cmsPipelineCheckAndRetreiveStages+304>:      add    BYTE PTR [rax],al
   0x7ffff7b8525b <cmsPipelineCheckAndRetreiveStages+306>:      add    BYTE PTR [rax],al
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffe288 --> 0x7ffff7b97b36 (<PrecalculatedXFORM+249>:      mov    rbx,rax)
0008| 0x7fffffffe290 --> 0x0
0016| 0x7fffffffe298 --> 0x7fffffffe370 --> 0x0
0024| 0x7fffffffe2a0 --> 0x303000000001
0032| 0x7fffffffe2a8 --> 0x60af00 --> 0x40007ff6eb
0040| 0x7fffffffe2b0 --> 0x61e1c0 --> 0x7ffff76ceb78 --> 0x6302e0 --> 0x0
0048| 0x7fffffffe2b8 --> 0x60c430 ("QQQQQ", 'R' <repeats 13 times>, "\270\367\377\177")
0056| 0x7fffffffe2c0 --> 0x71600000000
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGILL
0x00007ffff7b85254 in cmsPipelineCheckAndRetreiveStages (Lut=0x0, n=0x0) at ../../cms/src/cmslut.c:129
129             if (mpe ->Type != Type) 
gdb-peda$ p mpe
$1 = (cmsStage *) 0x0
gdb-peda$ bt
#0  0x00007ffff7b85254 in cmsPipelineCheckAndRetreiveStages (Lut=0x0, n=0x0) at ../../cms/src/cmslut.c:129
#1  0x00007ffff7b973f7 in cmsDoTransform (Transform=0x60c430, InputBuffer=0x61e1c0, OutputBuffer=0x60af00, Size=0x3030) at ../../cms/src/cmsxform.c:189
#2  0x0000000000402d73 in TileBasedXform (hXForm=0x60c430, in=0x609860, out=0x60a5e0, nPlanes=0x1) at ../../../cms/utils/tificc/tificc.c:408
#3  0x00000000004044db in TransformImage (in=0x609860, out=0x60a5e0, cDefInpProf=0x0) at ../../../cms/utils/tificc/tificc.c:904
#4  0x0000000000404d86 in main (argc=0x3, argv=0x7fffffffe608) at ../../../cms/utils/tificc/tificc.c:1167
#5  0x00007ffff732a830 in __libc_start_main (main=0x404c38 <main>, argc=0x3, argv=0x7fffffffe608, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffe5f8) at ../csu/libc-start.c:291
#6  0x0000000000401fe9 in _start ()

```

##  8-cms-crash-UnrollDoubleTo16


```
gdb --args tificc $POC /tmp/out.tiff
Program received signal SIGILL, Illegal instruction.
[----------------------------------registers-----------------------------------]
RAX: 0x7ffff7b8f000 (<UnrollDoubleTo16+304>:    rex.RB fsubr st,st(0))
RBX: 0x7ffff4d79518 --> 0x0
RCX: 0x1fe020
RDX: 0x7ffff4d79518 --> 0x0
RSI: 0x7fffffffe2d0 --> 0x0
RDI: 0x60ea40 --> 0x0
RBP: 0x7fffffffe330 --> 0x7fffffffe390 --> 0x7fffffffe400 --> 0x7fffffffe4f0 --> 0x7fffffffe520 --> 0x405ad0 (<__libc_csu_init>:        push   r15)
RSP: 0x7fffffffe288 --> 0x7ffff7b97b36 (<PrecalculatedXFORM+249>:       mov    rbx,rax)
RIP: 0x7ffff7b8f003 (<UnrollDoubleTo16+307>:    (bad))
R8 : 0x2ce
R9 : 0x7fffffffe370 --> 0x0
R10: 0x15b
R11: 0x7ffff7b97389 (<cmsDoTransform>:  push   rbp)
R12: 0x60ea51 --> 0xee00007ffff7b8f0
R13: 0x7fffffffe600 --> 0x3
R14: 0x0
R15: 0x0
EFLAGS: 0x10283 (CARRY parity adjust zero SIGN trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
=> 0x7ffff7b8f003 <UnrollDoubleTo16+307>:       (bad)
   0x7ffff7b8f004 <UnrollDoubleTo16+308>:       jmp    0x7ffff7b8f005 <UnrollDoubleTo16+309>
   0x7ffff7b8f006 <UnrollDoubleTo16+310>:       jmp    QWORD PTR [rsi-0x77]
   0x7ffff7b8f009 <UnrollDoubleTo16+313>:       rex.RB movs BYTE PTR es:[rdi],BYTE PTR ds:[rsi]
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffe288 --> 0x7ffff7b97b36 (<PrecalculatedXFORM+249>:      mov    rbx,rax)
0008| 0x7fffffffe290 --> 0x0
0016| 0x7fffffffe298 --> 0x7fffffffe370 --> 0x0
0024| 0x7fffffffe2a0 --> 0x1fe02000000001
0032| 0x7fffffffe2a8 --> 0x60c900 --> 0x0
0040| 0x7fffffffe2b0 --> 0x7ffff4d71010 --> 0x0
0048| 0x7fffffffe2b8 --> 0x60ea40 --> 0x0
0056| 0x7fffffffe2c0 --> 0xb1b00000000
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGILL
0x00007ffff7b8f003 in UnrollDoubleTo16 (info=0x7ffff4d79518, wIn=0x60ea51, accum=0x7fffffffe600 "\003", Stride=0x0) at ../../cms/src/cmspack.c:989
989             vi = _cmsQuickSaturateWord(v * maximum);
gdb-peda$ bt
#0  0x00007ffff7b8f003 in UnrollDoubleTo16 (info=0x7ffff4d79518, wIn=0x60ea51, accum=0x7fffffffe600 "\003", Stride=0x0) at ../../cms/src/cmspack.c:989
#1  0x00007ffff7b973f7 in cmsDoTransform (Transform=0x60ea40, InputBuffer=0x7ffff4d71010, OutputBuffer=0x60c900, Size=0x1fe020) at ../../cms/src/cmsxform.c:189
#2  0x0000000000402d73 in TileBasedXform (hXForm=0x60ea40, in=0x609860, out=0x60a5b0, nPlanes=0x1) at ../../../cms/utils/tificc/tificc.c:408
#3  0x00000000004044db in TransformImage (in=0x609860, out=0x60a5b0, cDefInpProf=0x0) at ../../../cms/utils/tificc/tificc.c:904
#4  0x0000000000404d86 in main (argc=0x3, argv=0x7fffffffe608) at ../../../cms/utils/tificc/tificc.c:1167
#5  0x00007ffff732a830 in __libc_start_main (main=0x404c38 <main>, argc=0x3, argv=0x7fffffffe608, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffe5f8) at ../csu/libc-start.c:291
#6  0x0000000000401fe9 in _start ()


```


## 9-cms-heap-overflow

```

[----------------------------------registers-----------------------------------]
RAX: 0x0
RBX: 0x65 ('e')
RCX: 0xffffffffffffffff
RDX: 0x6
RSI: 0x1390
RDI: 0x1390
RBP: 0x7fffffffdad0 --> 0x50 ('P')
RSP: 0x7fffffffd738 --> 0x7ffff734102a (<__GI_abort+362>:       mov    rdx,QWORD PTR fs:0x10)
RIP: 0x7ffff733f428 (<__GI_raise+56>:   cmp    rax,0xfffffffffffff000)
R8 : 0x6
R9 : 0x0
R10: 0x8
R11: 0x206
R12: 0x65 ('e')
R13: 0x7fffffffd8e8 --> 0x7fffffffde80 --> 0x7fffffffdf14 --> 0xf4e7901000000000
R14: 0x7fffffffd8e8 --> 0x7fffffffde80 --> 0x7fffffffdf14 --> 0xf4e7901000000000
R15: 0x2
EFLAGS: 0x206 (carry PARITY adjust zero sign trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
   0x7ffff733f41e <__GI_raise+46>:      mov    eax,0xea
   0x7ffff733f423 <__GI_raise+51>:      movsxd rdi,ecx
   0x7ffff733f426 <__GI_raise+54>:      syscall
=> 0x7ffff733f428 <__GI_raise+56>:      cmp    rax,0xfffffffffffff000
   0x7ffff733f42e <__GI_raise+62>:      ja     0x7ffff733f450 <__GI_raise+96>
   0x7ffff733f430 <__GI_raise+64>:      repz ret
   0x7ffff733f432 <__GI_raise+66>:      nop    WORD PTR [rax+rax*1+0x0]
   0x7ffff733f438 <__GI_raise+72>:      test   ecx,ecx
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffd738 --> 0x7ffff734102a (<__GI_abort+362>:      mov    rdx,QWORD PTR fs:0x10)
0008| 0x7fffffffd740 --> 0x20 (' ')
0016| 0x7fffffffd748 --> 0x0
0024| 0x7fffffffd750 --> 0x0
0032| 0x7fffffffd758 --> 0x0
0040| 0x7fffffffd760 --> 0x0
0048| 0x7fffffffd768 --> 0x0
0056| 0x7fffffffd770 --> 0x0
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGABRT
0x00007ffff733f428 in __GI_raise (sig=sig@entry=0x6) at ../sysdeps/unix/sysv/linux/raise.c:54
54      ../sysdeps/unix/sysv/linux/raise.c: No such file or directory.
gdb-peda$ bt
#0  0x00007ffff733f428 in __GI_raise (sig=sig@entry=0x6) at ../sysdeps/unix/sysv/linux/raise.c:54
#1  0x00007ffff734102a in __GI_abort () at abort.c:89
#2  0x00007ffff73817ea in __libc_message (do_abort=0x2, fmt=fmt@entry=0x7ffff749aed8 "*** Error in `%s': %s: 0x%s ***\n") at ../sysdeps/posix/libc_fatal.c:175
#3  0x00007ffff738c13e in malloc_printerr (ar_ptr=0x7ffff76ceb20 <main_arena>, ptr=0x610740, str=0x7ffff7497d3f "malloc(): memory corruption", action=<optimized out>) at malloc.c:5006
#4  _int_malloc (av=av@entry=0x7ffff76ceb20 <main_arena>, bytes=bytes@entry=0x40) at malloc.c:3474
#5  0x00007ffff738e184 in __GI___libc_malloc (bytes=0x40) at malloc.c:2913
#6  0x00007ffff7b73542 in _cmsMallocDefaultFn (ContextID=0x4, size=0x40) at ../../cms/src/cmserr.c:97
#7  0x00007ffff7b73921 in _cmsMalloc (ContextID=0x4, size=0x40) at ../../cms/src/cmserr.c:267
#8  0x00007ffff7b73564 in _cmsMallocZeroDefaultFn (ContextID=0x4, size=0x40) at ../../cms/src/cmserr.c:106
#9  0x00007ffff7b7395d in _cmsMallocZero (ContextID=0x4, size=0x40) at ../../cms/src/cmserr.c:274
#10 0x00007ffff7b84f63 in _cmsStageAllocPlaceholder (ContextID=0x4, Type=cmsSigMatrixElemType, InputChannels=0x3, OutputChannels=0x3, EvalPtr=0x7ffff7b85896 <EvaluateMatrix>, DupElemPtr=0x7ffff7b85995 <MatrixElemDup>, FreePtr=0x7ffff7b85a5d <MatrixElemTypeFree>, Data=0x0) at ../../cms/src/cmslut.c:40
#11 0x00007ffff7b85b9c in cmsStageAllocMatrix (ContextID=0x4, Rows=0x3, Cols=0x3, Matrix=0x7ffff7bbe400 <V2ToV4.7436>, Offset=0x0) at ../../cms/src/cmslut.c:394
#12 0x00007ffff7b87501 in _cmsStageAllocLabV2ToV4 (ContextID=0x4) at ../../cms/src/cmslut.c:1030
#13 0x00007ffff7b75ecb in EvalSegmentedFn (g=0x60ee90, R=0) at ../../cms/src/cmsgamma.c:694
#14 0x00007ffff7b78205 in cmsEvalToneCurveFloat (Curve=0x60ee90, v=0) at ../../cms/src/cmsgamma.c:1366
#15 0x00007ffff7b85426 in EvaluateCurves (In=0x7fffffffde60, Out=0x7fffffffe060, mpe=0x60eae0) at ../../cms/src/cmslut.c:182
#16 0x00007ffff7b87c5d in _LUTeval16 (In=0x1390, Out=0x7fffffffe2f0, D=0x0) at ../../cms/src/cmslut.c:1334
#17 0x00007ffff7b97b5e in PrecalculatedXFORM (p=0x60e940, in=0x7ffff4e79010, out=0x60f050, PixelsPerLine=0x1e803d, LineCount=0x1, Stride=0x7fffffffe370) at ../../cms/src/cmsxform.c:411
#18 0x00007ffff7b973f7 in cmsDoTransform (Transform=0x60e940, InputBuffer=0x7ffff4e79010, OutputBuffer=0x60f050, Size=0x1e803d) at ../../cms/src/cmsxform.c:189
#19 0x0000000000402d73 in TileBasedXform (hXForm=0x60e940, in=0x609860, out=0x60a4d0, nPlanes=0x1) at ../../../cms/utils/tificc/tificc.c:408
#20 0x00000000004044db in TransformImage (in=0x609860, out=0x60a4d0, cDefInpProf=0x0) at ../../../cms/utils/tificc/tificc.c:904
#21 0x0000000000404d86 in main (argc=0x3, argv=0x7fffffffe608) at ../../../cms/utils/tificc/tificc.c:1167
#22 0x00007ffff732a830 in __libc_start_main (main=0x404c38 <main>, argc=0x3, argv=0x7fffffffe608, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffe5f8) at ../csu/libc-start.c:291
#23 0x0000000000401fe9 in _start ()

```


## 10-cms-invalid-read-PackPlanarBytes


```
gdb --args tificc $POC /tmp/out.tiff
Program received signal SIGSEGV, Segmentation fault.
[----------------------------------registers-----------------------------------]
RAX: 0x70c810
RBX: 0x7ffff5959014 --> 0x0
RCX: 0x100020
RDX: 0x0
RSI: 0x7fffffffe300 --> 0x0
RDI: 0x60c2a0 --> 0x4101900441094
RBP: 0x7fffffffe290 --> 0x7fffffffe340 --> 0x7fffffffe3a0 --> 0x7fffffffe410 --> 0x7fffffffe500 --> 0x7fffffffe530 (--> ...)
RSP: 0x7fffffffe290 --> 0x7fffffffe340 --> 0x7fffffffe3a0 --> 0x7fffffffe410 --> 0x7fffffffe500 --> 0x7fffffffe530 (--> ...)
RIP: 0x7ffff7b9003e (<PackPlanarBytes+167>:     mov    BYTE PTR [rax],dl)
R8 : 0x2ce
R9 : 0x7fffffffe380 --> 0x0
R10: 0x15b
R11: 0x7ffff7b97389 (<cmsDoTransform>:  push   rbp)
R12: 0x60c7f0 --> 0x7ffff76cf100 --> 0x7ffff76cf0e8 --> 0x7ffff76cf0d8 --> 0x7ffff76cf0c8 --> 0x7ffff76cf0b8 (--> ...)
R13: 0x7fffffffe610 --> 0x3
R14: 0x0
R15: 0x0
EFLAGS: 0x10246 (carry PARITY adjust ZERO sign trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
   0x7ffff7b90036 <PackPlanarBytes+159>:        not    edx
   0x7ffff7b90038 <PackPlanarBytes+161>:        jmp    0x7ffff7b9003e <PackPlanarBytes+167>
   0x7ffff7b9003a <PackPlanarBytes+163>:        movzx  edx,BYTE PTR [rbp-0x21]
=> 0x7ffff7b9003e <PackPlanarBytes+167>:        mov    BYTE PTR [rax],dl
   0x7ffff7b90040 <PackPlanarBytes+169>:        mov    edx,ecx
   0x7ffff7b90042 <PackPlanarBytes+171>:        add    rax,rdx
   0x7ffff7b90045 <PackPlanarBytes+174>:        add    DWORD PTR [rbp-0x20],0x1
   0x7ffff7b90049 <PackPlanarBytes+178>:        mov    edx,DWORD PTR [rbp-0x20]
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffe290 --> 0x7fffffffe340 --> 0x7fffffffe3a0 --> 0x7fffffffe410 --> 0x7fffffffe500 --> 0x7fffffffe530 (--> ...)
0008| 0x7fffffffe298 --> 0x7ffff7b97b7f (<PrecalculatedXFORM+322>:      mov    r12,rax)
0016| 0x7fffffffe2a0 --> 0x0
0024| 0x7fffffffe2a8 --> 0x7fffffffe380 --> 0x0
0032| 0x7fffffffe2b0 --> 0x10002000000001
0040| 0x7fffffffe2b8 --> 0x60c7f0 --> 0x7ffff76cf100 --> 0x7ffff76cf0e8 --> 0x7ffff76cf0d8 --> 0x7ffff76cf0c8 (--> ...)
0048| 0x7fffffffe2c0 --> 0x7ffff5959010 --> 0x0
0056| 0x7fffffffe2c8 --> 0x60c2a0 --> 0x4101900441094
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x00007ffff7b9003e in PackPlanarBytes (info=0x60c2a0, wOut=0x7fffffffe300, output=0x70c810 <error: Cannot access memory at address 0x70c810>, Stride=0x100020) at ../../cms/src/cmspack.c:1458
1458            *(cmsUInt8Number*)  output = (cmsUInt8Number) (Reverse ? REVERSE_FLAVOR_8(v) : v);
gdb-peda$ p output
$1 = (cmsUInt8Number *) 0x70c810 <error: Cannot access memory at address 0x70c810>


```



##  11-cms-invalid-write-cmsPipelineCheckAndRetreiveStages

```
gdb --args tificc $POC /tmp/out.tiff
Program received signal SIGILL, Illegal instruction.
[----------------------------------registers-----------------------------------]
RAX: 0x7ffff7b85300 (<cmsPipelineCheckAndRetreiveStages+471>:   test   DWORD PTR [rax],ebp)
RBX: 0x7fffffffe300 --> 0xffff
RCX: 0x7fffffffde70 --> 0x3f800000
RDX: 0x60ea50 --> 0x0
RSI: 0x7fffffffe070 --> 0x3f800000
RDI: 0x7fffffffde70 --> 0x3f800000
RBP: 0x7fffffffe290 --> 0x7fffffffe340 --> 0x7fffffffe3a0 --> 0x7fffffffe410 --> 0x7fffffffe500 --> 0x7fffffffe530 (--> ...)
RSP: 0x7fffffffde48 --> 0x7ffff7b87c5d (<_LUTeval16+197>:       mov    eax,DWORD PTR [rbp-0x434])
RIP: 0x7ffff7b85302 (<cmsPipelineCheckAndRetreiveStages+473>:   (bad))
R8 : 0x2ce
R9 : 0x7fffffffe380 --> 0x0
R10: 0x15b
R11: 0x7ffff7b97389 (<cmsDoTransform>:  push   rbp)
R12: 0x60cc49 --> 0x5afb53fb4bfb44fb
R13: 0x7fffffffe610 --> 0x3
R14: 0x0
R15: 0x0
EFLAGS: 0x10282 (carry parity adjust zero SIGN trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
=> 0x7ffff7b85302 <cmsPipelineCheckAndRetreiveStages+473>:      (bad)
   0x7ffff7b85303 <cmsPipelineCheckAndRetreiveStages+474>:      (bad)
   0x7ffff7b85304 <cmsPipelineCheckAndRetreiveStages+475>:      dec    DWORD PTR [rax-0x75]
   0x7ffff7b85307 <cmsPipelineCheckAndRetreiveStages+478>:      xchg   ebp,eax
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffde48 --> 0x7ffff7b87c5d (<_LUTeval16+197>:      mov    eax,DWORD PTR [rbp-0x434])
0008| 0x7fffffffde50 --> 0x7ffff7b68410 --> 0x5f6e6f6d675f5f00 ('')
0016| 0x7fffffffde58 --> 0x100000000
0024| 0x7fffffffde60 --> 0x60ea50 --> 0x0
0032| 0x7fffffffde68 --> 0x609ee0 --> 0x60ea50 --> 0x0
0040| 0x7fffffffde70 --> 0x3f800000
0048| 0x7fffffffde78 --> 0x7fff00000000
0056| 0x7fffffffde80 --> 0x7fffffffdf14 --> 0x609c9800000000
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGILL
0x00007ffff7b85302 in cmsPipelineCheckAndRetreiveStages (Lut=0x608088, n=0x0) at ../../cms/src/cmslut.c:143
143                 *ElemPtr = mpe;
gdb-peda$ bt
#0  0x00007ffff7b85302 in cmsPipelineCheckAndRetreiveStages (Lut=0x608088, n=0x0) at ../../cms/src/cmslut.c:143
#1  0x00007ffff7b97b5e in PrecalculatedXFORM (p=0x60c2a0, in=0x626580, out=0x60c7f0, PixelsPerLine=0x1e20, LineCount=0x1, Stride=0x7fffffffe380) at ../../cms/src/cmsxform.c:411
#2  0x00007ffff7b973f7 in cmsDoTransform (Transform=0x60c2a0, InputBuffer=0x626580, OutputBuffer=0x60c7f0, Size=0x1e20) at ../../cms/src/cmsxform.c:189
#3  0x0000000000402d73 in TileBasedXform (hXForm=0x60c2a0, in=0x609860, out=0x60a480, nPlanes=0x3) at ../../../cms/utils/tificc/tificc.c:408
#4  0x00000000004044db in TransformImage (in=0x609860, out=0x60a480, cDefInpProf=0x0) at ../../../cms/utils/tificc/tificc.c:904
#5  0x0000000000404d86 in main (argc=0x3, argv=0x7fffffffe618) at ../../../cms/utils/tificc/tificc.c:1167
#6  0x00007ffff732a830 in __libc_start_main (main=0x404c38 <main>, argc=0x3, argv=0x7fffffffe618, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffe608) at ../csu/libc-start.c:291
#7  0x0000000000401fe9 in _start ()
gdb-peda$ p ElemPtr
$1 = (void **) 0xc0dfff9fe2b9c1c6
gdb-peda$ p *ElemPtr
Cannot access memory at address 0xc0dfff9fe2b9c1c6

```

## 12-cms-invalid-access-EvaluateCurves

```
gdb --args tificc $POC /tmp/out.tiff
Program received signal SIGSEGV, Segmentation fault.
[----------------------------------registers-----------------------------------]
RAX: 0xd54f63cff6bf3700
RBX: 0x7fffffffe320 --> 0xffffffff
RCX: 0x7fffffffde90 --> 0x3f8000003f800000
RDX: 0x8b48108b
RSI: 0x7fffffffe090 --> 0x3f8000003f800000
RDI: 0x7fffffffde90 --> 0x3f8000003f800000
RBP: 0x7fffffffe2b0 --> 0x7fffffffe360 --> 0x7fffffffe3c0 --> 0x7fffffffe430 --> 0x7fffffffe520 --> 0x7fffffffe550 (--> ...)
RSP: 0x7fffffffde68 --> 0x7ffff7b87c5d (<_LUTeval16+197>:       mov    eax,DWORD PTR [rbp-0x434])
RIP: 0x7ffff7b85405 (<EvaluateCurves+143>:      mov    rax,QWORD PTR [rax+0x8])
R8 : 0x2ce
R9 : 0x7fffffffe3a0 --> 0x0
R10: 0x15b
R11: 0x7ffff7b97389 (<cmsDoTransform>:  push   rbp)
R12: 0x60ccc9 --> 0x97fd90fd89fd82fd
R13: 0x7fffffffe630 --> 0x3
R14: 0x0
R15: 0x0
EFLAGS: 0x10206 (carry PARITY adjust zero sign trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
   0x7ffff7b853fc <EvaluateCurves+134>: add    rax,rdx
   0x7ffff7b853ff <EvaluateCurves+137>: mov    edx,DWORD PTR [rax]
   0x7ffff7b85401 <EvaluateCurves+139>: mov    rax,QWORD PTR [rbp-0x18]
=> 0x7ffff7b85405 <EvaluateCurves+143>: mov    rax,QWORD PTR [rax+0x8]
   0x7ffff7b85409 <EvaluateCurves+147>: mov    ecx,DWORD PTR [rbp-0x1c]
   0x7ffff7b8540c <EvaluateCurves+150>: shl    rcx,0x3
   0x7ffff7b85410 <EvaluateCurves+154>: add    rax,rcx
   0x7ffff7b85413 <EvaluateCurves+157>: mov    rax,QWORD PTR [rax]
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffde68 --> 0x7ffff7b87c5d (<_LUTeval16+197>:      mov    eax,DWORD PTR [rbp-0x434])
0008| 0x7fffffffde70 --> 0x7ffff7b68410 --> 0x5f6e6f6d675f5f00 ('')
0016| 0x7fffffffde78 --> 0x100000000
0024| 0x7fffffffde80 --> 0x60ea30 ("&.6>K[k{\227\267\326\367", '\377' <repeats 13 times>, "S\270\367\377\177")
0032| 0x7fffffffde88 --> 0x609ec0 --> 0x60ea30 ("&.6>K[k{\227\267\326\367", '\377' <repeats 13 times>, "S\270\367\377\177")
0040| 0x7fffffffde90 --> 0x3f8000003f800000
0048| 0x7fffffffde98 --> 0x7fff00000000
0056| 0x7fffffffdea0 --> 0x7fffffffdf34 --> 0x609c9800000000
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x00007ffff7b85405 in EvaluateCurves (In=0x40efffe000000000, Out=0x100000001, mpe=0x1) at ../../cms/src/cmslut.c:182
182             Out[i] = cmsEvalToneCurveFloat(Data ->TheCurves[i], In[i]);
gdb-peda$ p Out
$1 = (cmsFloat32Number *) 0x100000001
gdb-peda$ p Out[i]
Cannot access memory at address 0x100000001


```

## 13-cms-null-pointer-FastIdentity16

```

gdb --args tificc $POC /tmp/out.tiff
Program received signal SIGSEGV, Segmentation fault.
[----------------------------------registers-----------------------------------]
RAX: 0x0
RBX: 0x7ffff7fc9a98 --> 0x0
RCX: 0x7fffffffe2e0 --> 0x0
RDX: 0x0
RSI: 0x7fffffffe300 --> 0x0
RDI: 0x7fffffffe2e0 --> 0x0
RBP: 0x7fffffffe290 --> 0x7fffffffe340 --> 0x7fffffffe3a0 --> 0x7fffffffe410 --> 0x7fffffffe500 --> 0x7fffffffe530 (--> ...)
RSP: 0x7fffffffe290 --> 0x7fffffffe340 --> 0x7fffffffe3a0 --> 0x7fffffffe410 --> 0x7fffffffe500 --> 0x7fffffffe530 (--> ...)
RIP: 0x7ffff7bbaa37 (<FastIdentity16+56>:       mov    eax,DWORD PTR [rax+0x8])
R8 : 0x7fffffffe2e0 --> 0x0
R9 : 0x7fffffffe380 --> 0x0
R10: 0x15b
R11: 0x7ffff7b97389 (<cmsDoTransform>:  push   rbp)
R12: 0x615ca3 --> 0x50f849f842f83af8
R13: 0x7fffffffe610 --> 0x3
R14: 0x0
R15: 0x0
EFLAGS: 0x10202 (carry parity adjust zero sign trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
   0x7ffff7bbaa2c <FastIdentity16+45>:  mov    WORD PTR [rdx],ax
   0x7ffff7bbaa2f <FastIdentity16+48>:  add    DWORD PTR [rbp-0xc],0x1
   0x7ffff7bbaa33 <FastIdentity16+52>:  mov    rax,QWORD PTR [rbp-0x8]
=> 0x7ffff7bbaa37 <FastIdentity16+56>:  mov    eax,DWORD PTR [rax+0x8]
   0x7ffff7bbaa3a <FastIdentity16+59>:  cmp    eax,DWORD PTR [rbp-0xc]
   0x7ffff7bbaa3d <FastIdentity16+62>:  ja     0x7ffff7bbaa16 <FastIdentity16+23>
   0x7ffff7bbaa3f <FastIdentity16+64>:  nop
   0x7ffff7bbaa40 <FastIdentity16+65>:  pop    rbp
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffe290 --> 0x7fffffffe340 --> 0x7fffffffe3a0 --> 0x7fffffffe410 --> 0x7fffffffe500 --> 0x7fffffffe530 (--> ...)
0008| 0x7fffffffe298 --> 0x7ffff7b97b5e (<PrecalculatedXFORM+289>:      mov    rax,QWORD PTR [rbp-0x78])
0016| 0x7fffffffe2a0 --> 0x0
0024| 0x7fffffffe2a8 --> 0x7fffffffe380 --> 0x0
0032| 0x7fffffffe2b0 --> 0x832000000001
0040| 0x7fffffffe2b8 --> 0x60df60 --> 0x0
0048| 0x7fffffffe2c0 --> 0x7ffff7fba010 --> 0x0
0056| 0x7fffffffe2c8 --> 0x60c2a0 --> 0x4101900041092
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x00007ffff7bbaa37 in FastIdentity16 (In=0x7fffffffe2e0, Out=0x7fffffffe300, D=0x0) at ../../cms/src/cmsopt.c:1371
1371        for (i=0; i < Lut ->InputChannels; i++) 
gdb-peda$ bt
#0  0x00007ffff7bbaa37 in FastIdentity16 (In=0x7fffffffe2e0, Out=0x7fffffffe300, D=0x0) at ../../cms/src/cmsopt.c:1371
#1  0x00007ffff7b97b5e in PrecalculatedXFORM (p=0x60c2a0, in=0x7ffff7fba010, out=0x60df60, PixelsPerLine=0x8320, LineCount=0x1, Stride=0x7fffffffe380) at ../../cms/src/cmsxform.c:411
#2  0x00007ffff7b973f7 in cmsDoTransform (Transform=0x60c2a0, InputBuffer=0x7ffff7fba010, OutputBuffer=0x60df60, Size=0x8320) at ../../cms/src/cmsxform.c:189
#3  0x0000000000402d73 in TileBasedXform (hXForm=0x60c2a0, in=0x609860, out=0x60a480, nPlanes=0x3) at ../../../cms/utils/tificc/tificc.c:408
#4  0x00000000004044db in TransformImage (in=0x609860, out=0x60a480, cDefInpProf=0x0) at ../../../cms/utils/tificc/tificc.c:904
#5  0x0000000000404d86 in main (argc=0x3, argv=0x7fffffffe618) at ../../../cms/utils/tificc/tificc.c:1167
#6  0x00007ffff732a830 in __libc_start_main (main=0x404c38 <main>, argc=0x3, argv=0x7fffffffe618, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffe608) at ../csu/libc-start.c:291
#7  0x0000000000401fe9 in _start ()
gdb-peda$ p Lut
$1 = (cmsPipeline *) 0x0


```



##  14-cms-invalid-access-Unroll1ByteReversed

```

gdb --args tificc $POC /tmp/out.tiff
Program received signal SIGSEGV, Segmentation fault.
[----------------------------------registers-----------------------------------]
RAX: 0x7ffff7b8e600 (<Unroll1ByteReversed+28>:  mov    r8d,edi)
RBX: 0x6166ee --> 0x882588158805168e
RCX: 0x1810
RDX: 0x168e
RSI: 0x168e
RDI: 0xff9f168e
RBP: 0x7fffffffe340 --> 0x7fffffffe3a0 --> 0x7fffffffe410 --> 0x7fffffffe500 --> 0x7fffffffe530 --> 0x405ad0 (<__libc_csu_init>:        push   r15)
RSP: 0x7fffffffe298 --> 0x7ffff7b97b36 (<PrecalculatedXFORM+249>:       mov    rbx,rax)
RIP: 0x7ffff7b8e61b (<Unroll1ByteReversed+55>:  mov    WORD PTR [rcx],dx)
R8 : 0x60e930 --> 0x87870087870087
R9 : 0x7fffffffe380 --> 0x0
R10: 0x15b
R11: 0x7ffff7b97389 (<cmsDoTransform>:  push   rbp)
R12: 0x60e941 --> 0xee00007ffff7b8e6
R13: 0x7fffffffe610 --> 0x3
R14: 0x0
R15: 0x0
EFLAGS: 0x10206 (carry PARITY adjust zero sign trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
   0x7ffff7b8e612 <Unroll1ByteReversed+46>:     movzx  esi,WORD PTR [rsi]
   0x7ffff7b8e615 <Unroll1ByteReversed+49>:     mov    WORD PTR [rdx],si
   0x7ffff7b8e618 <Unroll1ByteReversed+52>:     movzx  edx,WORD PTR [rdx]
=> 0x7ffff7b8e61b <Unroll1ByteReversed+55>:     mov    WORD PTR [rcx],dx
   0x7ffff7b8e61e <Unroll1ByteReversed+58>:     add    rax,0x1
   0x7ffff7b8e622 <Unroll1ByteReversed+62>:     pop    rbp
   0x7ffff7b8e623 <Unroll1ByteReversed+63>:     ret
   0x7ffff7b8e624 <UnrollAnyWords>:     push   rbp
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffe298 --> 0x7ffff7b97b36 (<PrecalculatedXFORM+249>:      mov    rbx,rax)
0008| 0x7fffffffe2a0 --> 0x0
0016| 0x7fffffffe2a8 --> 0x7fffffffe380 --> 0x0
0024| 0x7fffffffe2b0 --> 0x181000000001
0032| 0x7fffffffe2b8 --> 0x60ade0 --> 0x7f00f20000f6f2
0040| 0x7fffffffe2c0 --> 0x60f040 --> 0x7ffff76cf2e8 --> 0x7ffff76cf2d8 --> 0x7ffff76cf2c8 --> 0x7ffff76cf2b8 (--> ...)
0048| 0x7fffffffe2c8 --> 0x60e930 --> 0x87870087870087
0056| 0x7fffffffe2d0 --> 0x13cb00000000
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x00007ffff7b8e61b in Unroll1ByteReversed (info=0xff9f168e, wIn=0x1810, accum=0x7ffff7b8e600 <Unroll1ByteReversed+28> "A\211\370\017\266\070@\017\266\377D\t\307\367\327f\211>\017\267\066f\211\062\017\267\022f\211\021H\203\300\001]\303UH\211\345ATSH\203\354\060H\211\370I\211\364H\211\323\213\020\301\352\003\203\342\017\211U\320\213\020\301\352\v\203\342\001\211U\324\213\020\301\352\n\203\342\001\211U\330\213\020\301\352\r\203\342\001\211U\334\213\020\301\352\016\203\342\001\211U\340\213", Stride=0x1810) at ../../cms/src/cmspack.c:462
462         wIn[0] = wIn[1] = wIn[2] = REVERSE_FLAVOR_16(FROM_8_TO_16(*accum)); accum++;     // L *)
gdb-peda$ bt
#0  0x00007ffff7b8e61b in Unroll1ByteReversed (info=0xff9f168e, wIn=0x1810, accum=0x7ffff7b8e600 <Unroll1ByteReversed+28> "A\211\370\017\266\070@\017\266\377D\t\307\367\327f\211>\017\267\066f\211\062\017\267\022f\211\021H\203\300\001]\303UH\211\345ATSH\203\354\060H\211\370I\211\364H\211\323\213\020\301\352\003\203\342\017\211U\320\213\020\301\352\v\203\342\001\211U\324\213\020\301\352\n\203\342\001\211U\330\213\020\301\352\r\203\342\001\211U\334\213\020\301\352\016\203\342\001\211U\340\213", Stride=0x1810) at ../../cms/src/cmspack.c:462
#1  0x00007ffff7b973f7 in cmsDoTransform (Transform=0x60e930, InputBuffer=0x60f040, OutputBuffer=0x60ade0, Size=0x1810) at ../../cms/src/cmsxform.c:189
#2  0x0000000000402d73 in TileBasedXform (hXForm=0x60e930, in=0x609860, out=0x60a4c0, nPlanes=0x1) at ../../../cms/utils/tificc/tificc.c:408
#3  0x00000000004044db in TransformImage (in=0x609860, out=0x60a4c0, cDefInpProf=0x0) at ../../../cms/utils/tificc/tificc.c:904
#4  0x0000000000404d86 in main (argc=0x3, argv=0x7fffffffe618) at ../../../cms/utils/tificc/tificc.c:1167
#5  0x00007ffff732a830 in __libc_start_main (main=0x404c38 <main>, argc=0x3, argv=0x7fffffffe618, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffe608) at ../csu/libc-start.c:291
#6  0x0000000000401fe9 in _start ()
gdb-peda$ p wIn[0]
Cannot access memory at address 0x1810

```


## 15-cms-null-pointer-UnrollAnyWords

```
gdb --args tificc $POC /tmp/out.tiff
Program received signal SIGSEGV, Segmentation fault.
[----------------------------------registers-----------------------------------]
RAX: 0x7ffff7b8e6c3 (<UnrollAnyWords+159>:      or     BYTE PTR [rbx+0x148ec45],cl)
RBX: 0x62577e --> 0xc45cc452c448c43e
RCX: 0x2e10
RDX: 0x62577e --> 0xc45cc452c448c43e
RSI: 0x7fffffffe2e0 --> 0xc402c3f8c3eec3e4
RDI: 0x60e930 --> 0xc2c2c2c2c2c200c2
RBP: 0x7fffffffe340 --> 0x7fffffffe3a0 --> 0x7fffffffe410 --> 0x7fffffffe500 --> 0x7fffffffe530 --> 0x405ad0 (<__libc_csu_init>:        push   r15)
RSP: 0x7fffffffe298 --> 0x7ffff7b97b36 (<PrecalculatedXFORM+249>:       mov    rbx,rax)
RIP: 0x7ffff7b8e6c3 (<UnrollAnyWords+159>:      or     BYTE PTR [rbx+0x148ec45],cl)
R8 : 0x1
R9 : 0x7fffffffe380 --> 0x0
R10: 0x15b
R11: 0x7ffff7b97389 (<cmsDoTransform>:  push   rbp)
R12: 0x60e941 --> 0xee00007ffff7b8e6
R13: 0x7fffffffe610 --> 0x3
R14: 0x0
R15: 0x0
EFLAGS: 0x10287 (CARRY PARITY adjust zero SIGN trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
=> 0x7ffff7b8e6c3 <UnrollAnyWords+159>: or     BYTE PTR [rbx+0x148ec45],cl
   0x7ffff7b8e6c9 <UnrollAnyWords+165>: ror    BYTE PTR [rcx-0x73],0x14
   0x7ffff7b8e6cd <UnrollAnyWords+169>: add    al,0x83
   0x7ffff7b8e6cf <UnrollAnyWords+171>: jge    0x7ffff7b8e6ad <UnrollAnyWords+137>
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffe298 --> 0x7ffff7b97b36 (<PrecalculatedXFORM+249>:      mov    rbx,rax)
0008| 0x7fffffffe2a0 --> 0x0
0016| 0x7fffffffe2a8 --> 0x7fffffffe380 --> 0x0
0024| 0x7fffffffe2b0 --> 0x2e1000000001
0032| 0x7fffffffe2b8 --> 0x60ade0 --> 0xeb000000e7
0040| 0x7fffffffe2c0 --> 0x61e080 --> 0x60e7f0 --> 0xb500b5b500b5b500
0048| 0x7fffffffe2c8 --> 0x60e930 --> 0xc2c2c2c2c2c200c2
0056| 0x7fffffffe2d0 --> 0x13cb00000000
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x00007ffff7b8e6c3 in UnrollAnyWords (info=0x7ffff7b8e6c3 <UnrollAnyWords+159>, wIn=0x60e941, accum=0x62577e ">\304H\304R\304\\\304e\304o\304y\304\203\304\215\304\227\304\241\304\253\304\265\304\277\304\311\304\323\304\335\304\347\304\361\304\373\304\005\305\017\305\031\305#\305,\305\066\305@\305J\305T\305^\305h\305r\305|\305\206\305\220\305\231\305\243\305\255\305\267\305\301\305\313\305\325\305\337\305\351\305\362\305\374\305\006\306\020\306\032\306$\306.\306\067\306A\306K\306U\306_\306i\306s\306|\306\206\306\220\306\232\306\244\306\256\306\267\306\301\306\313\306\325\306\337\306\350\306\362\306\374\306\006\307\020\307\031\307#\307-\307\067\307A\307J\307T\307^\307h\307r\307{\307\205\307\217\307\231\307\242\307\254\307\266\307\300\307\311\307\323\307\335\307\347\307\360\307\372\307\004\310\016\310"..., Stride=0x2e10) at ../../cms/src/cmspack.c:496
496                 v = CHANGE_ENDIAN(v);
gdb-peda$ bt
#0  0x00007ffff7b8e6c3 in UnrollAnyWords (info=0x7ffff7b8e6c3 <UnrollAnyWords+159>, wIn=0x60e941, accum=0x62577e ">\304H\304R\304\\\304e\304o\304y\304\203\304\215\304\227\304\241\304\253\304\265\304\277\304\311\304\323\304\335\304\347\304\361\304\373\304\005\305\017\305\031\305#\305,\305\066\305@\305J\305T\305^\305h\305r\305|\305\206\305\220\305\231\305\243\305\255\305\267\305\301\305\313\305\325\305\337\305\351\305\362\305\374\305\006\306\020\306\032\306$\306.\306\067\306A\306K\306U\306_\306i\306s\306|\306\206\306\220\306\232\306\244\306\256\306\267\306\301\306\313\306\325\306\337\306\350\306\362\306\374\306\006\307\020\307\031\307#\307-\307\067\307A\307J\307T\307^\307h\307r\307{\307\205\307\217\307\231\307\242\307\254\307\266\307\300\307\311\307\323\307\335\307\347\307\360\307\372\307\004\310\016\310"..., Stride=0x2e10) at ../../cms/src/cmspack.c:496
#1  0x00007ffff7b973f7 in cmsDoTransform (Transform=0x60e930, InputBuffer=0x61e080, OutputBuffer=0x60ade0, Size=0x2e10) at ../../cms/src/cmsxform.c:189
#2  0x0000000000402d73 in TileBasedXform (hXForm=0x60e930, in=0x609860, out=0x60a4c0, nPlanes=0x1) at ../../../cms/utils/tificc/tificc.c:408
#3  0x00000000004044db in TransformImage (in=0x609860, out=0x60a4c0, cDefInpProf=0x0) at ../../../cms/utils/tificc/tificc.c:904
#4  0x0000000000404d86 in main (argc=0x3, argv=0x7fffffffe618) at ../../../cms/utils/tificc/tificc.c:1167
#5  0x00007ffff732a830 in __libc_start_main (main=0x404c38 <main>, argc=0x3, argv=0x7fffffffe618, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffe608) at ../csu/libc-start.c:291
#6  0x0000000000401fe9 in _start ()
gdb-peda$ p v
$1 = 0x0


```




