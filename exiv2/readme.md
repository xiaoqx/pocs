exiv2 pocs
===========

## 1. 1-string-format

```
$gdb --args ./bin/.libs/lt-exiv2 -pS  $POC

Breakpoint 1, Exiv2::Internal::stringFormat (format=0x7ffff78a1879 "%8ld | 0xff%02x %-5s") at image.cpp:1013
1013                rc = vsnprintf(&buffer[0], buffer.size(), format, args);
gdb-peda$ n

Program received signal SIGSEGV, Segmentation fault.
[----------------------------------registers-----------------------------------]
RAX: 0x0
RBX: 0x7fffffffd540 --> 0x7ffffbad8001
RCX: 0xffffffffffffffff
RDX: 0x28 ('(')
RSI: 0x7fffffe8
RDI: 0x1000000000000
RBP: 0x7fffffffd530 --> 0x644b70 ("      63 | 0xfffffff")
RSP: 0x7fffffffcf50 --> 0x0
RIP: 0x7ffff6d06943 (<_IO_vfprintf_internal+7427>:      repnz scas al,BYTE PTR es:[rdi])
R8 : 0x7fffffff
R9 : 0x7ffff7fe3780 (0x00007ffff7fe3780)
R10: 0x7ffff707bfe0 --> 0x0
R11: 0x0
R12: 0x7ffff6d08f69 (<_IO_vfprintf_internal+17193>:     cmp    BYTE PTR [rbp-0x508],0x0)
R13: 0x1000000000000
R14: 0x7ffff78a1879 ("%8ld | 0xff%02x %-5s")
R15: 0x7fffffffd6e0 --> 0x3000000028 ('(')
EFLAGS: 0x10286 (carry PARITY adjust zero SIGN trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
   0x7ffff6d0693a <_IO_vfprintf_internal+7418>: xor    eax,eax
   0x7ffff6d0693c <_IO_vfprintf_internal+7420>: or     rcx,0xffffffffffffffff
   0x7ffff6d06940 <_IO_vfprintf_internal+7424>: mov    rdi,r13
=> 0x7ffff6d06943 <_IO_vfprintf_internal+7427>: repnz scas al,BYTE PTR es:[rdi]
   0x7ffff6d06945 <_IO_vfprintf_internal+7429>: mov    DWORD PTR [rbp-0x508],0x0
   0x7ffff6d0694f <_IO_vfprintf_internal+7439>: mov    rsi,rcx
   0x7ffff6d06952 <_IO_vfprintf_internal+7442>: not    rsi
   0x7ffff6d06955 <_IO_vfprintf_internal+7445>: lea    r10,[rsi-0x1]
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffcf50 --> 0x0
0008| 0x7fffffffcf58 --> 0x0
0016| 0x7fffffffcf60 --> 0x0
0024| 0x7fffffffcf68 --> 0x0
0032| 0x7fffffffcf70 --> 0x0
0040| 0x7fffffffcf78 --> 0x0
0048| 0x7fffffffcf80 --> 0x7fffffffd0b0 --> 0xffffffffffffffff
0056| 0x7fffffffcf88 --> 0x0
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x00007ffff6d06943 in _IO_vfprintf_internal (s=s@entry=0x7fffffffd540, format=<optimized out>, format@entry=0x7ffff78a1879 "%8ld | 0xff%02x %-5s", ap=ap@entry=0x7fffffffd6e0) at vfprintf.c:1661
1661    vfprintf.c: No such file or directory.
gdb-peda$ bt
#0  0x00007ffff6d06943 in _IO_vfprintf_internal (s=s@entry=0x7fffffffd540, format=<optimized out>, format@entry=0x7ffff78a1879 "%8ld | 0xff%02x %-5s", ap=ap@entry=0x7fffffffd6e0) at vfprintf.c:1661
#1  0x00007ffff6d2d499 in _IO_vsnprintf (string=0x644b70 "      63 | 0xfffffff", maxlen=<optimized out>, format=0x7ffff78a1879 "%8ld | 0xff%02x %-5s", args=0x7fffffffd6e0) at vsnprintf.c:119
#2  0x00007ffff778247d in Exiv2::Internal::stringFormat (format=0x7ffff78a1879 "%8ld | 0xff%02x %-5s") at image.cpp:1013
#3  0x00007ffff77966e9 in Exiv2::JpegBase::printStructure (this=0x644a60, out=..., option=Exiv2::kpsBasic, depth=0x0) at jpgimage.cpp:787
#4  0x000000000041cafe in Action::Print::printStructure (this=0x6447e0, out=..., option=Exiv2::kpsBasic) at actions.cpp:283
#5  0x000000000041c87b in Action::Print::run (this=0x6447e0, path="/data/xqx/projects/docker-fuzz/testcases/pics/exiv2/1-poc.jpg") at actions.cpp:246
#6  0x000000000040e337 in main (argc=0x3, argv=0x7fffffffe4b8) at exiv2.cpp:166
#7  0x00007ffff6cdcf45 in __libc_start_main (main=0x40e07e <main(int, char* const*)>, argc=0x3, argv=0x7fffffffe4b8, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffe4a8) at libc-start.c:287
#8  0x000000000040dfb9 in _start ()


```

## 2-invalid-memory-access

```
$ gdb ./bin/.libs/lt-exiv2 -pt $POC

RAX: 0xb8
RBX: 0x0
RCX: 0x7fffffffdb10 --> 0x404570 --> 0xd00220000502f ('/P')
RDX: 0x644ad0 --> 0x0
RSI: 0x7fffffffdbdf --> 0x2000 ('')
RDI: 0x644ad0 --> 0x0
RBP: 0x7fffffffdd70 --> 0x7fffffffde10 --> 0x7fffffffde60 --> 0x7fffffffe000 --> 0x7fffffffe200 --> 0x7fffffffe270 (--> ...)
RSP: 0x7fffffffdc40 --> 0x0
RIP: 0x7ffff77308bf (<Exiv2::Internal::printCsLensFFFF(std::ostream&, Exiv2::Value const&, Exiv2::ExifData const*)+288>:        mov    rax,QWORD PTR [rax])
R8 : 0x0
R9 : 0x648220 --> 0x644890 --> 0x0
R10: 0x7fffffffda00 --> 0x0
R11: 0x42cf1c (<std::_List_const_iterator<Exiv2::Exifdatum>::operator->() const>:       push   rbp)
R12: 0x20 (' ')
R13: 0x0
R14: 0x0
R15: 0x1
EFLAGS: 0x10206 (carry PARITY adjust zero sign trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
   0x7ffff77308b3 <Exiv2::Internal::printCsLensFFFF(std::ostream&, Exiv2::Value const&, Exiv2::ExifData const*)+276>:   mov    rdx,rax
   0x7ffff77308b6 <Exiv2::Internal::printCsLensFFFF(std::ostream&, Exiv2::Value const&, Exiv2::ExifData const*)+279>:   mov    rax,QWORD PTR [rdx]
   0x7ffff77308b9 <Exiv2::Internal::printCsLensFFFF(std::ostream&, Exiv2::Value const&, Exiv2::ExifData const*)+282>:   add    rax,0xb8
=> 0x7ffff77308bf <Exiv2::Internal::printCsLensFFFF(std::ostream&, Exiv2::Value const&, Exiv2::ExifData const*)+288>:   mov    rax,QWORD PTR [rax]
   0x7ffff77308c2 <Exiv2::Internal::printCsLensFFFF(std::ostream&, Exiv2::Value const&, Exiv2::ExifData const*)+291>:   mov    rdi,rdx
   0x7ffff77308c5 <Exiv2::Internal::printCsLensFFFF(std::ostream&, Exiv2::Value const&, Exiv2::ExifData const*)+294>:   call   rax
   0x7ffff77308c7 <Exiv2::Internal::printCsLensFFFF(std::ostream&, Exiv2::Value const&, Exiv2::ExifData const*)+296>:   mov    rdx,rax
   0x7ffff77308ca <Exiv2::Internal::printCsLensFFFF(std::ostream&, Exiv2::Value const&, Exiv2::ExifData const*)+299>:   lea    rax,[rbp-0xd0]
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffdc40 --> 0x0
0008| 0x7fffffffdc48 --> 0x1f75b4f78
0016| 0x7fffffffdc50 --> 0x101000000000000
0024| 0x7fffffffdc58 --> 0x644ac0 --> 0x64a580 --> 0x6442f0 --> 0x64d9c0 --> 0x64da90 (--> ...)
0032| 0x7fffffffdc60 --> 0x651760 --> 0x7ffff7b883d0 --> 0x7ffff7752106 (<Exiv2::ValueType<unsigned short>::~ValueType()>:      push   rbp)
0040| 0x7fffffffdc68 --> 0x7fffffffde90 --> 0x7ffff75842b8 --> 0x7ffff73324a0 (<_ZNSt19basic_ostringstreamIcSt11char_traitsIcESaIcEED1Ev>:      push   rbx)
0048| 0x7fffffffdc70 --> 0x7ffff7b862c0 --> 0x4
0056| 0x7fffffffdc78 --> 0x7ffff7b865a0 --> 0x1
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x00007ffff77308bf in Exiv2::Internal::printCsLensFFFF (os=..., value=..., metadata=0x644ac0) at canonmn_int.cpp:1773
1773                if( metadata->findKey(ExifKey("Exif.Image.Model"        ))->value().toString() == "Canon EOS 30D"
gdb-peda$ bt
#0  0x00007ffff77308bf in Exiv2::Internal::printCsLensFFFF (os=..., value=..., metadata=0x644ac0) at canonmn_int.cpp:1773
#1  0x00007ffff7731e73 in Exiv2::Internal::CanonMakerNote::printCsLensType (os=..., value=..., metadata=0x644ac0) at canonmn_int.cpp:1941
#2  0x00007ffff776ce90 in Exiv2::Exifdatum::write (this=0x651680, os=..., pMetadata=0x644ac0) at exif.cpp:226
#3  0x00007ffff779f9ba in Exiv2::Metadatum::print (this=0x651680, pMetadata=0x644ac0) at metadatum.cpp:75
#4  0x0000000000421219 in Action::Print::printMetadatum (this=0x644830, md=..., pImage=0x644ab0) at actions.cpp:759
#5  0x000000000041fda6 in Action::Print::printMetadata (this=0x644830, image=0x644ab0) at actions.cpp:556
#6  0x000000000041fcd4 in Action::Print::printList (this=0x644830) at actions.cpp:545
#7  0x000000000041c83b in Action::Print::run (this=0x644830, path="./crashes-2018-03-23-16-19/exiv2000:id:000000,sig:11,src:000000,op:flip1,pos:52") at actions.cpp:243
#8  0x000000000040e337 in main (argc=0x3, argv=0x7fffffffe498) at exiv2.cpp:166
#9  0x00007ffff6cdcf45 in __libc_start_main (main=0x40e07e <main(int, char* const*)>, argc=0x3, argv=0x7fffffffe498, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffe488) at libc-start.c:287
#10 0x000000000040dfb9 in _start ()

```

## 3-vfpirntf_internal-outofbound-read

./exiv2 -pR  $POC

```
[----------------------------------registers-----------------------------------]
RAX: 0x0
RBX: 0x7fffffffd520 --> 0x7ffffbad8001
RCX: 0xffffffffffffffff
RDX: 0x28 ('(')
RSI: 0x7fffffe8
RDI: 0x1000000000000
RBP: 0x7fffffffd510 --> 0x644b70 ("      50 | 0xfffffff")
RSP: 0x7fffffffcf30 --> 0x0
RIP: 0x7ffff6d0e943 (<_IO_vfprintf_internal+7427>:      repnz scas al,BYTE PTR es:[rdi])
R8 : 0x7fffffff
R9 : 0x7ffff7fe3780 (0x00007ffff7fe3780)
R10: 0x7ffff7083fe0 --> 0x0
R11: 0x0
R12: 0x7ffff6d10f69 (<_IO_vfprintf_internal+17193>:     cmp    BYTE PTR [rbp-0x508],0x0)
R13: 0x1000000000000
R14: 0x7ffff78a3e99 ("%8ld | 0xff%02x %-5s")
R15: 0x7fffffffd6c0 --> 0x3000000028 ('(')
EFLAGS: 0x10286 (carry PARITY adjust zero SIGN trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
   0x7ffff6d0e93a <_IO_vfprintf_internal+7418>: xor    eax,eax
   0x7ffff6d0e93c <_IO_vfprintf_internal+7420>: or     rcx,0xffffffffffffffff
   0x7ffff6d0e940 <_IO_vfprintf_internal+7424>: mov    rdi,r13
=> 0x7ffff6d0e943 <_IO_vfprintf_internal+7427>: repnz scas al,BYTE PTR es:[rdi]
   0x7ffff6d0e945 <_IO_vfprintf_internal+7429>: mov    DWORD PTR [rbp-0x508],0x0
   0x7ffff6d0e94f <_IO_vfprintf_internal+7439>: mov    rsi,rcx
   0x7ffff6d0e952 <_IO_vfprintf_internal+7442>: not    rsi
   0x7ffff6d0e955 <_IO_vfprintf_internal+7445>: lea    r10,[rsi-0x1]
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffcf30 --> 0x0
0008| 0x7fffffffcf38 --> 0x0
0016| 0x7fffffffcf40 --> 0x0
0024| 0x7fffffffcf48 --> 0x0
0032| 0x7fffffffcf50 --> 0x0
0040| 0x7fffffffcf58 --> 0x0
0048| 0x7fffffffcf60 --> 0x7fffffffd090 --> 0xffffffffffffffff
0056| 0x7fffffffcf68 --> 0x0
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x00007ffff6d0e943 in _IO_vfprintf_internal (s=s@entry=0x7fffffffd520, format=<optimized out>, format@entry=0x7ffff78a3e99 "%8ld | 0xff%02x %-5s", ap=ap@entry=0x7fffffffd6c0) at vfprintf.c:1661
1661    vfprintf.c: No such file or directory.
gdb-peda$ bt
#0  0x00007ffff6d0e943 in _IO_vfprintf_internal (s=s@entry=0x7fffffffd520, format=<optimized out>, format@entry=0x7ffff78a3e99 "%8ld | 0xff%02x %-5s", ap=ap@entry=0x7fffffffd6c0) at vfprintf.c:1661
#1  0x00007ffff6d35499 in _IO_vsnprintf (string=0x644b70 "      50 | 0xfffffff", maxlen=<optimized out>, format=0x7ffff78a3e99 "%8ld | 0xff%02x %-5s", args=0x7fffffffd6c0) at vsnprintf.c:119
#2  0x00007ffff7784e1d in Exiv2::Internal::stringFormat (format=0x7ffff78a3e99 "%8ld | 0xff%02x %-5s") at image.cpp:1013
#3  0x00007ffff7799089 in Exiv2::JpegBase::printStructure (this=0x644a80, out=..., option=Exiv2::kpsRecursive, depth=0x0) at jpgimage.cpp:787
#4  0x000000000041ca7e in Action::Print::printStructure (this=0x644800, out=..., option=Exiv2::kpsRecursive) at actions.cpp:283
#5  0x000000000041c816 in Action::Print::run (this=0x644800, path="./crashes-2018-03-23-19-59/exiv2000:id:000000,sig:11,src:000000,op:flip1,pos:2") at actions.cpp:247
#6  0x000000000040e2b7 in main (argc=0x3, argv=0x7fffffffe498) at exiv2.cpp:166
#7  0x00007ffff6ce4f45 in __libc_start_main (main=0x40dffe <main(int, char* const*)>, argc=0x3, argv=0x7fffffffe498, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffe488) at libc-start.c:287
#8  0x000000000040df39 in _start ()

a$ bt
#0  0x00007ffff6d0e943 in _IO_vfprintf_internal (s=s@entry=0x7fffffffd520, format=<optimized out>, format@entry=0x7ffff78a3e99 "%8ld | 0xff%02x %-5s", ap=ap@entry=0x7fffffffd6c0) at vfprintf.c:1661
#1  0x00007ffff6d35499 in _IO_vsnprintf (string=0x644b70 "      50 | 0xfffffff", maxlen=<optimized out>, format=0x7ffff78a3e99 "%8ld | 0xff%02x %-5s", args=0x7fffffffd6c0) at vsnprintf.c:119
#2  0x00007ffff7784e1d in Exiv2::Internal::stringFormat (format=0x7ffff78a3e99 "%8ld | 0xff%02x %-5s") at image.cpp:1013
#3  0x00007ffff7799089 in Exiv2::JpegBase::printStructure (this=0x644a80, out=..., option=Exiv2::kpsRecursive, depth=0x0) at jpgimage.cpp:787
#4  0x000000000041ca7e in Action::Print::printStructure (this=0x644800, out=..., option=Exiv2::kpsRecursive) at actions.cpp:283
#5  0x000000000041c816 in Action::Print::run (this=0x644800, path="./crashes-2018-03-23-19-59/exiv2000:id:000000,sig:11,src:000000,op:flip1,pos:2") at actions.cpp:247
#6  0x000000000040e2b7 in main (argc=0x3, argv=0x7fffffffe498) at exiv2.cpp:166
#7  0x00007ffff6ce4f45 in __libc_start_main (main=0x40dffe <main(int, char* const*)>, argc=0x3, argv=0x7fffffffe498, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffe488) at libc-start.c:287
#8  0x000000000040df39 in _start ()


```

## 4-DataBuf-abort-1

```
$ gdb --args ./exiv2 -pR $POC

[----------------------------------registers-----------------------------------]
RAX: 0x0
RBX: 0x640a98 --> 0x7ffff70861c0 --> 0xfbad2887
RCX: 0xffffffffffffffff
RDX: 0x6
RSI: 0xe75
RDI: 0xe75
RBP: 0x7ffff73608a2 ("std::bad_alloc")
RSP: 0x7fffffffdf98 --> 0x7ffff6cfd028 (<__GI_abort+328>:       mov    rdx,QWORD PTR fs:0x10)
RIP: 0x7ffff6cf9c37 (<__GI_raise+55>:   cmp    rax,0xfffffffffffff000)
R8 : 0xa ('\n')
R9 : 0x7ffff7fe3780 (0x00007ffff7fe3780)
R10: 0x8
R11: 0x202
R12: 0x7ffff0000950 --> 0x0
R13: 0x7fffffffe4b0 --> 0x3
R14: 0x0
R15: 0x0
EFLAGS: 0x202 (carry parity adjust zero sign trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
   0x7ffff6cf9c2d <__GI_raise+45>:      movsxd rdi,ecx
   0x7ffff6cf9c30 <__GI_raise+48>:      mov    eax,0xea
   0x7ffff6cf9c35 <__GI_raise+53>:      syscall
=> 0x7ffff6cf9c37 <__GI_raise+55>:      cmp    rax,0xfffffffffffff000
   0x7ffff6cf9c3d <__GI_raise+61>:      ja     0x7ffff6cf9c5d <__GI_raise+93>
   0x7ffff6cf9c3f <__GI_raise+63>:      repz ret
   0x7ffff6cf9c41 <__GI_raise+65>:      nop    DWORD PTR [rax+0x0]
   0x7ffff6cf9c48 <__GI_raise+72>:      test   ecx,ecx
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffdf98 --> 0x7ffff6cfd028 (<__GI_abort+328>:      mov    rdx,QWORD PTR fs:0x10)
0008| 0x7fffffffdfa0 --> 0x20 (' ')
0016| 0x7fffffffdfa8 --> 0x0
0024| 0x7fffffffdfb0 --> 0x0
0032| 0x7fffffffdfb8 --> 0x0
0040| 0x7fffffffdfc0 --> 0x0
0048| 0x7fffffffdfc8 --> 0x0
0056| 0x7fffffffdfd0 --> 0x0
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGABRT
0x00007ffff6cf9c37 in __GI_raise (sig=sig@entry=0x6) at ../nptl/sysdeps/unix/sysv/linux/raise.c:56
56      ../nptl/sysdeps/unix/sysv/linux/raise.c: No such file or directory.
gdb-peda$ bt
#0  0x00007ffff6cf9c37 in __GI_raise (sig=sig@entry=0x6) at ../nptl/sysdeps/unix/sysv/linux/raise.c:56
#1  0x00007ffff6cfd028 in __GI_abort () at abort.c:89
#2  0x00007ffff7302535 in __gnu_cxx::__verbose_terminate_handler() () from /usr/lib/x86_64-linux-gnu/libstdc++.so.6
#3  0x00007ffff73006d6 in ?? () from /usr/lib/x86_64-linux-gnu/libstdc++.so.6
#4  0x00007ffff7300703 in std::terminate() () from /usr/lib/x86_64-linux-gnu/libstdc++.so.6
#5  0x00007ffff7300922 in __cxa_throw () from /usr/lib/x86_64-linux-gnu/libstdc++.so.6
#6  0x00007ffff7300e0d in operator new(unsigned long) () from /usr/lib/x86_64-linux-gnu/libstdc++.so.6
#7  0x00007ffff7300ea9 in operator new[](unsigned long) () from /usr/lib/x86_64-linux-gnu/libstdc++.so.6
#8  0x000000000042bfbc in Exiv2::DataBuf::DataBuf (this=0x7fffffffe240, size=0xfffffffffffffffe) at ../include/exiv2/types.hpp:206
#9  0x00007ffff7793273 in Exiv2::Jp2Image::printStructure (this=0x644af0, out=..., option=Exiv2::kpsRecursive, depth=0x0) at jp2image.cpp:507
#10 0x000000000041ca7e in Action::Print::printStructure (this=0x6447d0, out=..., option=Exiv2::kpsRecursive) at actions.cpp:283
#11 0x000000000041c816 in Action::Print::run (this=0x6447d0, path="/data/xqx/projects/xiaoqx-pocs/exiv2/4-DataBuf-abort-1") at actions.cpp:247
#12 0x000000000040e2b7 in main (argc=0x3, argv=0x7fffffffe4b8) at exiv2.cpp:166
#13 0x00007ffff6ce4f45 in __libc_start_main (main=0x40dffe <main(int, char* const*)>, argc=0x3, argv=0x7fffffffe4b8, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffe4a8) at libc-start.c:287
#14 0x000000000040df39 in _start ()


Description: Abort signal
Short description: AbortSignal (20/22)
Hash: f4d11dd33ec4a3410221da428e990ecd.7df5c29d65892b74ce337b4abda05326
Exploitability Classification: UNKNOWN
Explanation: The target is stopped on a SIGABRT. SIGABRTs are often generated by libc and compiled check-code to indicate potentially exploitable conditions. Unfortunately this command does not yet further analyze these crashes.


```

## 5-printStructure-outbound-read-1

```
$ valgrind exiv2 $POC

==29031== Memcheck, a memory error detector
==29031== Copyright (C) 2002-2013, and GNU GPL'd, by Julian Seward et al.
==29031== Using Valgrind-3.10.1 and LibVEX; rerun with -h for copyright info
==29031== Command: ./installed/bin/exiv2 crashes-2018-03-27-15-54/exiv2000:id:000007,sig:11,src:000947,op:havoc,rep:4
==29031== 
==29031== Invalid read of size 1
==29031==    at 0x523B295: Exiv2::IptcData::printStructure(std::ostream&, unsigned char const*, unsigned long, unsigned int) (iptc.cpp:354)
==29031==    by 0x52316CC: Exiv2::Image::printIFDStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, unsigned int, bool, char, int) (image.cpp:470)
==29031==    by 0x5231E0F: Exiv2::Image::printTiffStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, int, unsigned long) (image.cpp:533)
==29031==    by 0x52CB2FA: Exiv2::TiffImage::printStructure(std::ostream&, Exiv2::PrintStructureOption, int) (tiffimage.cpp:344)
==29031==    by 0x52CA550: Exiv2::TiffImage::readMetadata() (tiffimage.cpp:187)
==29031==    by 0x41CBE8: Action::Print::printSummary() (actions.cpp:296)
==29031==    by 0x41C7A6: Action::Print::run(std::string const&) (actions.cpp:242)
==29031==    by 0x40E2B6: main (exiv2.cpp:166)
==29031==  Address 0x68b5ba2 is 0 bytes after a block of size 2 alloc'd
==29031==    at 0x4C2B800: operator new[](unsigned long) (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==29031==    by 0x5231653: Exiv2::Image::printIFDStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, unsigned int, bool, char, int) (image.cpp:467)
==29031==    by 0x5231E0F: Exiv2::Image::printTiffStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, int, unsigned long) (image.cpp:533)
==29031==    by 0x52CB2FA: Exiv2::TiffImage::printStructure(std::ostream&, Exiv2::PrintStructureOption, int) (tiffimage.cpp:344)
==29031==    by 0x52CA550: Exiv2::TiffImage::readMetadata() (tiffimage.cpp:187)
==29031==    by 0x41CBE8: Action::Print::printSummary() (actions.cpp:296)
==29031==    by 0x41C7A6: Action::Print::run(std::string const&) (actions.cpp:242)
==29031==    by 0x40E2B6: main (exiv2.cpp:166)
==29031== 
==29031== 
==29031== Process terminating with default action of signal 11 (SIGSEGV)
==29031==  Access not within mapped region at address 0x6C9B000
==29031==    at 0x523B295: Exiv2::IptcData::printStructure(std::ostream&, unsigned char const*, unsigned long, unsigned int) (iptc.cpp:354)
==29031==    by 0x52316CC: Exiv2::Image::printIFDStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, unsigned int, bool, char, int) (image.cpp:470)
==29031==    by 0x5231E0F: Exiv2::Image::printTiffStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, int, unsigned long) (image.cpp:533)
==29031==    by 0x52CB2FA: Exiv2::TiffImage::printStructure(std::ostream&, Exiv2::PrintStructureOption, int) (tiffimage.cpp:344)
==29031==    by 0x52CA550: Exiv2::TiffImage::readMetadata() (tiffimage.cpp:187)
==29031==    by 0x41CBE8: Action::Print::printSummary() (actions.cpp:296)
==29031==    by 0x41C7A6: Action::Print::run(std::string const&) (actions.cpp:242)
==29031==    by 0x40E2B6: main (exiv2.cpp:166)
==29031==  If you believe this happened as a result of a stack
==29031==  overflow in your program's main thread (unlikely but
==29031==  possible), you can try to increase the size of the
==29031==  main thread stack using the --main-stacksize= flag.
==29031==  The main thread stack size used in this run was 8388608.
==29031== 
==29031== HEAP SUMMARY:
==29031==     in use at exit: 33,395 bytes in 697 blocks
==29031==   total heap usage: 920 allocs, 223 frees, 46,386 bytes allocated
==29031== 
==29031== LEAK SUMMARY:
==29031==    definitely lost: 0 bytes in 0 blocks
==29031==    indirectly lost: 0 bytes in 0 blocks
==29031==      possibly lost: 14,018 bytes in 348 blocks
==29031==    still reachable: 19,377 bytes in 349 blocks
==29031==         suppressed: 0 bytes in 0 blocks
==29031== Rerun with --leak-check=full to see details of leaked memory
==29031== 
==29031== For counts of detected and suppressed errors, rerun with: -v
==29031== ERROR SUMMARY: 4084831 errors from 1 contexts (suppressed: 0 from 0)
```
## 6-binaryToString-outbound-read-1

```
$ valgrind exiv2 $POC

==29386== Memcheck, a memory error detector
==29386== Copyright (C) 2002-2013, and GNU GPL'd, by Julian Seward et al.
==29386== Using Valgrind-3.10.1 and LibVEX; rerun with -h for copyright info
==29386== Command: ./installed/bin/exiv2 crashes-2018-03-27-15-54/exiv2000:id:000020,sig:11,src:001299+000137,op:splice,rep:2
==29386== 
==29386== Invalid read of size 1
==29386==    at 0x5233FE8: Exiv2::Internal::binaryToString(unsigned char const*, unsigned long, unsigned long) (image.cpp:1031)
==29386==    by 0x523B43C: Exiv2::IptcData::printStructure(std::ostream&, unsigned char const*, unsigned long, unsigned int) (iptc.cpp:364)
==29386==    by 0x52316CC: Exiv2::Image::printIFDStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, unsigned int, bool, char, int) (image.cpp:470)
==29386==    by 0x5231E0F: Exiv2::Image::printTiffStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, int, unsigned long) (image.cpp:533)
==29386==    by 0x52CB2FA: Exiv2::TiffImage::printStructure(std::ostream&, Exiv2::PrintStructureOption, int) (tiffimage.cpp:344)
==29386==    by 0x52CA550: Exiv2::TiffImage::readMetadata() (tiffimage.cpp:187)
==29386==    by 0x41CBE8: Action::Print::printSummary() (actions.cpp:296)
==29386==    by 0x41C7A6: Action::Print::run(std::string const&) (actions.cpp:242)
==29386==    by 0x40E2B6: main (exiv2.cpp:166)
==29386==  Address 0x68b55b5 is 0 bytes after a block of size 21 alloc'd
==29386==    at 0x4C2B800: operator new[](unsigned long) (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==29386==    by 0x5231653: Exiv2::Image::printIFDStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, unsigned int, bool, char, int) (image.cpp:467)
==29386==    by 0x5231E0F: Exiv2::Image::printTiffStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, int, unsigned long) (image.cpp:533)
==29386==    by 0x52CB2FA: Exiv2::TiffImage::printStructure(std::ostream&, Exiv2::PrintStructureOption, int) (tiffimage.cpp:344)
==29386==    by 0x52CA550: Exiv2::TiffImage::readMetadata() (tiffimage.cpp:187)
==29386==    by 0x41CBE8: Action::Print::printSummary() (actions.cpp:296)
==29386==    by 0x41C7A6: Action::Print::run(std::string const&) (actions.cpp:242)
==29386==    by 0x40E2B6: main (exiv2.cpp:166)
==29386== 
==29386== Invalid read of size 1
==29386==    at 0x523B4B9: Exiv2::IptcData::printStructure(std::ostream&, unsigned char const*, unsigned long, unsigned int) (iptc.cpp:357)
==29386==    by 0x52316CC: Exiv2::Image::printIFDStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, unsigned int, bool, char, int) (image.cpp:470)
==29386==    by 0x5231E0F: Exiv2::Image::printTiffStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, int, unsigned long) (image.cpp:533)
==29386==    by 0x52CB2FA: Exiv2::TiffImage::printStructure(std::ostream&, Exiv2::PrintStructureOption, int) (tiffimage.cpp:344)
==29386==    by 0x52CA550: Exiv2::TiffImage::readMetadata() (tiffimage.cpp:187)
==29386==    by 0x41CBE8: Action::Print::printSummary() (actions.cpp:296)
==29386==    by 0x41C7A6: Action::Print::run(std::string const&) (actions.cpp:242)
==29386==    by 0x40E2B6: main (exiv2.cpp:166)
==29386==  Address 0x68b56b8 is 88 bytes inside a block of size 537 free'd
==29386==    at 0x4C2C2BC: operator delete(void*) (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==29386==    by 0x5703540: std::basic_ostringstream<char, std::char_traits<char>, std::allocator<char> >::~basic_ostringstream() (in /usr/lib/x86_64-linux-gnu/libstdc++.so.6.0.19)
==29386==    by 0x520D7BB: Exiv2::IptcDataSets::dataSetName(unsigned short, unsigned short) (datasets.cpp:494)
==29386==    by 0x523B399: Exiv2::IptcData::printStructure(std::ostream&, unsigned char const*, unsigned long, unsigned int) (iptc.cpp:362)
==29386==    by 0x52316CC: Exiv2::Image::printIFDStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, unsigned int, bool, char, int) (image.cpp:470)
==29386==    by 0x5231E0F: Exiv2::Image::printTiffStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, int, unsigned long) (image.cpp:533)
==29386==    by 0x52CB2FA: Exiv2::TiffImage::printStructure(std::ostream&, Exiv2::PrintStructureOption, int) (tiffimage.cpp:344)
==29386==    by 0x52CA550: Exiv2::TiffImage::readMetadata() (tiffimage.cpp:187)
==29386==    by 0x41CBE8: Action::Print::printSummary() (actions.cpp:296)
==29386==    by 0x41C7A6: Action::Print::run(std::string const&) (actions.cpp:242)
==29386==    by 0x40E2B6: main (exiv2.cpp:166)
==29386== 
==29386== Invalid read of size 1
==29386==    at 0x523B295: Exiv2::IptcData::printStructure(std::ostream&, unsigned char const*, unsigned long, unsigned int) (iptc.cpp:354)
==29386==    by 0x52316CC: Exiv2::Image::printIFDStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, unsigned int, bool, char, int) (image.cpp:470)
==29386==    by 0x5231E0F: Exiv2::Image::printTiffStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, int, unsigned long) (image.cpp:533)
==29386==    by 0x52CB2FA: Exiv2::TiffImage::printStructure(std::ostream&, Exiv2::PrintStructureOption, int) (tiffimage.cpp:344)
==29386==    by 0x52CA550: Exiv2::TiffImage::readMetadata() (tiffimage.cpp:187)
==29386==    by 0x41CBE8: Action::Print::printSummary() (actions.cpp:296)
==29386==    by 0x41C7A6: Action::Print::run(std::string const&) (actions.cpp:242)
==29386==    by 0x40E2B6: main (exiv2.cpp:166)
==29386==  Address 0x68b6382 is 0 bytes after a block of size 2 alloc'd
==29386==    at 0x4C2B800: operator new[](unsigned long) (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==29386==    by 0x5231653: Exiv2::Image::printIFDStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, unsigned int, bool, char, int) (image.cpp:467)
==29386==    by 0x5231E0F: Exiv2::Image::printTiffStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, int, unsigned long) (image.cpp:533)
==29386==    by 0x52CB2FA: Exiv2::TiffImage::printStructure(std::ostream&, Exiv2::PrintStructureOption, int) (tiffimage.cpp:344)
==29386==    by 0x52CA550: Exiv2::TiffImage::readMetadata() (tiffimage.cpp:187)
==29386==    by 0x41CBE8: Action::Print::printSummary() (actions.cpp:296)
==29386==    by 0x41C7A6: Action::Print::run(std::string const&) (actions.cpp:242)
==29386==    by 0x40E2B6: main (exiv2.cpp:166)
==29386== 
==29386== 
==29386== Process terminating with default action of signal 11 (SIGSEGV)
==29386==  Access not within mapped region at address 0x6C9B000
==29386==    at 0x523B295: Exiv2::IptcData::printStructure(std::ostream&, unsigned char const*, unsigned long, unsigned int) (iptc.cpp:354)
==29386==    by 0x52316CC: Exiv2::Image::printIFDStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, unsigned int, bool, char, int) (image.cpp:470)
==29386==    by 0x5231E0F: Exiv2::Image::printTiffStructure(Exiv2::BasicIo&, std::ostream&, Exiv2::PrintStructureOption, int, unsigned long) (image.cpp:533)
==29386==    by 0x52CB2FA: Exiv2::TiffImage::printStructure(std::ostream&, Exiv2::PrintStructureOption, int) (tiffimage.cpp:344)
==29386==    by 0x52CA550: Exiv2::TiffImage::readMetadata() (tiffimage.cpp:187)
==29386==    by 0x41CBE8: Action::Print::printSummary() (actions.cpp:296)
==29386==    by 0x41C7A6: Action::Print::run(std::string const&) (actions.cpp:242)
==29386==    by 0x40E2B6: main (exiv2.cpp:166)
==29386==  If you believe this happened as a result of a stack
==29386==  overflow in your program's main thread (unlikely but
==29386==  possible), you can try to increase the size of the
==29386==  main thread stack using the --main-stacksize= flag.
==29386==  The main thread stack size used in this run was 8388608.
==29386== 
==29386== HEAP SUMMARY:
==29386==     in use at exit: 33,451 bytes in 698 blocks
==29386==   total heap usage: 929 allocs, 231 frees, 47,782 bytes allocated
==29386== 
==29386== LEAK SUMMARY:
==29386==    definitely lost: 0 bytes in 0 blocks
==29386==    indirectly lost: 0 bytes in 0 blocks
==29386==      possibly lost: 14,026 bytes in 348 blocks
==29386==    still reachable: 19,425 bytes in 350 blocks
==29386==         suppressed: 0 bytes in 0 blocks
==29386== Rerun with --leak-check=full to see details of leaked memory
==29386== 
==29386== For counts of detected and suppressed errors, rerun with: -v
==29386== ERROR SUMMARY: 4082854 errors from 3 contexts (suppressed: 0 from 0)

```

