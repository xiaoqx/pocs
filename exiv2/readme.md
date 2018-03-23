exiv2 pocs
=============

1. 1-string-format

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
$gdb ./bin/.libs/lt-exiv2 -pt $POC

```sters-----------------------------------]
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

