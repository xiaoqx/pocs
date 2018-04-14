gegl poc
==================

## 1. gegl-outbound-write-1

```
(gdb) run ./gegl-outbound-write-1
The program being debugged has been started already.
Start it from the beginning? (y or n) y
Starting program: /src/aflbuild/installed/bin/gegl ./gegl-outbound-write-1
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

(gegl:303): GEGL-WARNING **: Failed to set operation type gegl:text, using a passthrough op instead

(gegl:303): GEGL-WARNING **: Failed to set operation type gegl:text, using a passthrough op instead

** (gegl:303): WARNING **: No display handler operation found for gegl:display
[New Thread 0x7fffef432700 (LWP 304)]

Thread 1 "gegl" received signal SIGSEGV, Segmentation fault.
__memcpy_sse2_unaligned () at ../sysdeps/x86_64/multiarch/memcpy-sse2-unaligned.S:37
37      ../sysdeps/x86_64/multiarch/memcpy-sse2-unaligned.S: No such file or directory.
(gdb) exploitable
Description: Access violation on destination operand
Short description: DestAv (8/22)
Hash: d7482cdb03f2cb0b586cd5cf74b1cb43.f4d790321ded280ed3837c295823fc52
Exploitability Classification: EXPLOITABLE
Explanation: The target crashed on an access violation at an address matching the destination operand of the instruction. This likely indicates a write access violation, which means the attacker may control the write address and/or value.
Other tags: AccessViolation (21/22)
(gdb) bt
#0  __memcpy_sse2_unaligned () at ../sysdeps/x86_64/multiarch/memcpy-sse2-unaligned.S:37
#1  0x00007ffff7a8d051 in memcpy (__len=384, __src=0x7ffff7f441a0, __dest=<optimized out>) at /usr/include/x86_64-linux-gnu/bits/string3.h:53
#2  gegl_buffer_iterate_read_simple (buffer=buffer@entry=0x7491a0, roi=roi@entry=0x7fffffffd140, buf=buf@entry=0x7ffde0cea010 "", buf_stride=buf_stride@entry=-2118217885, format=format@entry=0x63cb40,
    level=level@entry=0) at ../../../gegl/gegl/buffer/gegl-buffer-access.c:1212
#3  0x00007ffff7a9d115 in gegl_buffer_iterate_read_dispatch (buffer=0x7491a0, roi=<optimized out>, buf=0x7ffde0cea010 "", rowstride=-2118217885, format=0x63cb40, repeat_mode=GEGL_ABYSS_NONE, level=0)
    at ../../../gegl/gegl/buffer/gegl-buffer-access.c:1832
#4  0x00007ffff7aa6f41 in _gegl_buffer_get_unlocked (buffer=0x7491a0, scale=<optimized out>, rect=0x7fffffffd370, format=<optimized out>, dest_buf=0x7ffde0cea010, rowstride=0, flags=GEGL_ABYSS_NONE)
    at ../../../gegl/gegl/buffer/gegl-buffer-access.c:2055
#5  0x00007fffef8440ba in process (operation=<optimized out>, output=0x7491a0, result=<optimized out>, level=<optimized out>) at ../../../gegl/operations/external/ppm-load.c:320
#6  0x00007ffff7b41bfe in gegl_operation_source_process (operation=0x6cc260, context=<optimized out>, output_prop=<optimized out>, result=0x740450, level=0)
    at ../../../gegl/gegl/operation/gegl-operation-source.c:182
#7  0x00007ffff7b6b0ae in gegl_graph_process (path=0x73b470, level=level@entry=0) at ../../../gegl/gegl/process/gegl-graph-traversal.c:469
#8  0x00007ffff7b67fe8 in gegl_eval_manager_apply (self=0x73fb80, roi=roi@entry=0x748cc0, level=level@entry=0) at ../../../gegl/gegl/process/gegl-eval-manager.c:128
#9  0x00007ffff7b51f7e in gegl_node_apply_roi (level=0, roi=0x748cc0, self=0x696050) at ../../../gegl/gegl/graph/gegl-node.c:1081
#10 gegl_node_blit (self=0x696050, scale=1, roi=roi@entry=0x748cc0, format=format@entry=0x63cb40, destination_buf=destination_buf@entry=0x7fffe7c8e010, rowstride=-2118217885, rowstride@entry=0,
    flags=flags@entry=GEGL_BLIT_DEFAULT) at ../../../gegl/gegl/graph/gegl-node.c:1161
#11 0x00007ffff7b6f928 in render_rectangle (processor=0x734c60) at ../../../gegl/gegl/process/gegl-processor.c:518
#12 gegl_processor_render (progress=0x0, rectangle=0x734c88, processor=0x734c60) at ../../../gegl/gegl/process/gegl-processor.c:662
#13 gegl_processor_work (processor=processor@entry=0x734c60, progress=progress@entry=0x0) at ../../../gegl/gegl/process/gegl-processor.c:796
#14 0x00007ffff7b50a5a in gegl_node_process (self=<optimized out>) at ../../../gegl/gegl/graph/gegl-node.c:1827
#15 0x00000000004039ea in main (argc=<optimized out>, argv=<optimized out>) at ../../gegl/bin/gegl.c:255
```

## 2. gegl-dos-1

```
$ gdb --args gegl $POC
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

(gegl:387): GEGL-WARNING **: Failed to set operation type gegl:text, using a passthrough op instead

(gegl:387): GEGL-WARNING **: Failed to set operation type gegl:text, using a passthrough op instead

** (gegl:387): WARNING **: No display handler operation found for gegl:display
[New Thread 0x7fffef432700 (LWP 391)]

(gegl:387): GLib-ERROR **: /build/glib2.0-prJhLS/glib2.0-2.48.2/./glib/gmem.c:100: failed to allocate 18446744071565764224 bytes

Thread 1 "gegl" received signal SIGTRAP, Trace/breakpoint trap.
0x00007ffff74dba5b in g_logv () from /lib/x86_64-linux-gnu/libglib-2.0.so.0
(gdb) bt
#0  0x00007ffff74dba5b in g_logv () from /lib/x86_64-linux-gnu/libglib-2.0.so.0
#1  0x00007ffff74dbbcf in g_log () from /lib/x86_64-linux-gnu/libglib-2.0.so.0
#2  0x00007ffff74da744 in g_malloc () from /lib/x86_64-linux-gnu/libglib-2.0.so.0
#3  0x00007ffff7b6f8a2 in render_rectangle (processor=0x735460) at ../../../gegl/gegl/process/gegl-processor.c:512
#4  gegl_processor_render (progress=0x0, rectangle=0x735488, processor=0x735460) at ../../../gegl/gegl/process/gegl-processor.c:662
#5  gegl_processor_work (processor=processor@entry=0x735460, progress=progress@entry=0x0) at ../../../gegl/gegl/process/gegl-processor.c:796
#6  0x00007ffff7b50a5a in gegl_node_process (self=<optimized out>) at ../../../gegl/gegl/graph/gegl-node.c:1827
#7  0x00000000004039ea in main (argc=<optimized out>, argv=<optimized out>) at ../../gegl/bin/gegl.c:255
```

## 3. gegl-dos-2

```
$ gdb --args gegl $POC
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

(gegl:394): GEGL-WARNING **: Failed to set operation type gegl:text, using a passthrough op instead

(gegl:394): GEGL-WARNING **: Failed to set operation type gegl:text, using a passthrough op instead

** (gegl:394): WARNING **: No display handler operation found for gegl:display
[New Thread 0x7fffef432700 (LWP 398)]

(gegl:394): GLib-ERROR **: /build/glib2.0-prJhLS/glib2.0-2.48.2/./glib/gmem.c:100: failed to allocate 1333333333323288 bytes

Thread 1 "gegl" received signal SIGTRAP, Trace/breakpoint trap.
0x00007ffff74dba5b in g_logv () from /lib/x86_64-linux-gnu/libglib-2.0.so.0
(gdb) bt
#0  0x00007ffff74dba5b in g_logv () from /lib/x86_64-linux-gnu/libglib-2.0.so.0
#1  0x00007ffff74dbbcf in g_log () from /lib/x86_64-linux-gnu/libglib-2.0.so.0
#2  0x00007ffff74da744 in g_malloc () from /lib/x86_64-linux-gnu/libglib-2.0.so.0
#3  0x00007fffef843f20 in process (operation=<optimized out>, output=0x7491a0, result=<optimized out>,
    level=<optimized out>) at ../../../gegl/operations/external/ppm-load.c:293
#4  0x00007ffff7b41bfe in gegl_operation_source_process (operation=0x6cc460, context=<optimized out>,
    output_prop=<optimized out>, result=0x740450, level=0)
    at ../../../gegl/gegl/operation/gegl-operation-source.c:182
#5  0x00007ffff7b6b0ae in gegl_graph_process (path=0x747160, level=level@entry=0)
    at ../../../gegl/gegl/process/gegl-graph-traversal.c:469
#6  0x00007ffff7b67fe8 in gegl_eval_manager_apply (self=0x73ff80, roi=roi@entry=0x746d40,
    level=level@entry=0) at ../../../gegl/gegl/process/gegl-eval-manager.c:128
#7  0x00007ffff7b51f7e in gegl_node_apply_roi (level=0, roi=0x746d40, self=0x696050)
    at ../../../gegl/gegl/graph/gegl-node.c:1081
#8  gegl_node_blit (self=0x696050, scale=1, roi=roi@entry=0x746d40, format=format@entry=0x63cb20,
    destination_buf=destination_buf@entry=0x7ffff7ee9010, rowstride=24, rowstride@entry=0,
    flags=flags@entry=GEGL_BLIT_DEFAULT) at ../../../gegl/gegl/graph/gegl-node.c:1161
#9  0x00007ffff7b6f928 in render_rectangle (processor=0x735460)
    at ../../../gegl/gegl/process/gegl-processor.c:518
#10 gegl_processor_render (progress=0x0, rectangle=0x735488, processor=0x735460)
    at ../../../gegl/gegl/process/gegl-processor.c:662
#11 gegl_processor_work (processor=processor@entry=0x735460, progress=progress@entry=0x0)
    at ../../../gegl/gegl/process/gegl-processor.c:796
#12 0x00007ffff7b50a5a in gegl_node_process (self=<optimized out>)
    at ../../../gegl/gegl/graph/gegl-node.c:1827
#13 0x00000000004039ea in main (argc=<optimized out>, argv=<optimized out>)
    at ../../gegl/bin/gegl.c:255
```

## 4. gegl-outbound-write-2

```
$ gdb --args gegl $POC
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

(gegl:201): GEGL-WARNING **: Failed to set operation type gegl:text, using a passthrough op instead

(gegl:201): GEGL-WARNING **: Failed to set operation type gegl:text, using a passthrough op instead
LIBPNG ERROR: PNG unsigned integer out of range.libpng error: PNG unsigned integer out of range.
LIBPNG ERROR: PNG unsigned integer out of range.libpng error: PNG unsigned integer out of range.

** (gegl:201): WARNING **: No display handler operation found for gegl:display
LIBPNG ERROR: PNG unsigned integer out of range.libpng error: PNG unsigned integer out of range.
[New Thread 0x7fffef432700 (LWP 202)]

Thread 1 "gegl" received signal SIGSEGV, Segmentation fault.
babl_format_get_bytes_per_pixel (format=0x824871a0) at babl-format.c:538
538       if (format->class_type == BABL_FORMAT)
$ bt
#0  babl_format_get_bytes_per_pixel (format=0x824871a0) at babl-format.c:538
#1  0x00007ffff7b06ad5 in constructed (object=<optimized out>) at ../../../gegl/gegl/buffer/gegl-tile-backend.c:128
#2  0x00007ffff7b0f37b in gegl_tile_backend_swap_constructed (object=0x7355c0) at ../../../gegl/gegl/buffer/gegl-tile-backend-swap.c:825
#3  0x00007ffff77b1897 in ?? () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#4  0x00007ffff77b31b5 in g_object_new_valist () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#5  0x00007ffff77b3521 in g_object_new () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#6  0x00007ffff7a819a1 in gegl_buffer_constructor (type=<optimized out>, n_params=16, params=<optimized out>) at ../../../gegl/gegl/buffer/gegl-buffer.c:578
#7  0x00007ffff77b1149 in ?? () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#8  0x00007ffff77b31b5 in g_object_new_valist () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#9  0x00007ffff77b3521 in g_object_new () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#10 0x00007ffff7b5114f in gegl_node_get_cache (node=<optimized out>) at ../../../gegl/gegl/graph/gegl-node.c:2015
#11 0x00007ffff7b6e471 in gegl_processor_set_rectangle (processor=0x735460, rectangle=<optimized out>) at ../../../gegl/gegl/process/gegl-processor.c:366
#12 0x00007ffff77b170d in ?? () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#13 0x00007ffff77b31b5 in g_object_new_valist () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#14 0x00007ffff77b3521 in g_object_new () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#15 0x00007ffff7b732dc in gegl_node_new_processor (node=<optimized out>, rectangle=<optimized out>) at ../../../gegl/gegl/process/gegl-processor.c:829
#16 0x00007ffff7b50a4a in gegl_node_process (self=0x6962c0) at ../../../gegl/gegl/graph/gegl-node.c:1825
#17 0x00000000004039ea in main (argc=<optimized out>, argv=<optimized out>) at ../../gegl/bin/gegl.c:255
Description: Access violation on destination operand
Short description: DestAv (8/22)
Hash: 2de8b3adb00a42a787c6c00f820ea8be.4b5b031fbd08ebbe2eb2f77fcda9adc2
Exploitability Classification: EXPLOITABLE
Explanation: The target crashed on an access violation at an address matching the destination operand of the instruction. This likely indicates a write access violation, which means the attacker may control the write address and/or value.
Other tags: AccessViolation (21/22)

```

## 5. gegl-dos-3

```
Starting program: /src/aflbuild/installed/bin/gegl /work/crashes/'gegl000:id:000000,sig:11,src:000069,op:havoc,rep:2'
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

(gegl:155): GEGL-WARNING **: Failed to set operation type gegl:text, using a passthrough op instead

(gegl:155): GEGL-WARNING **: Failed to set operation type gegl:text, using a passthrough op instead

Program received signal SIGSEGV, Segmentation fault.
0x00007ffff091bb94 in ?? () from /usr/lib/x86_64-linux-gnu/libjpeg.so.8
(gdb) bt
#0  0x00007ffff091bb94 in ?? () from /usr/lib/x86_64-linux-gnu/libjpeg.so.8
#1  0x00007ffff091c4df in ?? () from /usr/lib/x86_64-linux-gnu/libjpeg.so.8
#2  0x00007ffff091a8ed in ?? () from /usr/lib/x86_64-linux-gnu/libjpeg.so.8
#3  0x00007ffff0913bc7 in jpeg_consume_input () from /usr/lib/x86_64-linux-gnu/libjpeg.so.8
#4  0x00007ffff0913ea3 in jpeg_read_header () from /usr/lib/x86_64-linux-gnu/libjpeg.so.8
#5  0x00007fffef63d223 in gegl_jpg_load_query_jpg (stream=stream@entry=0x748890, width=width@entry=0x7fffffffd670, height=height@entry=0x7fffffffd674, out_format=out_format@entry=0x7fffffffd678)
    at ../../../gegl/operations/external/jpg-load.c:188
#6  0x00007fffef63d7f4 in gegl_jpg_load_get_bounding_box (operation=operation@entry=0x6cc260) at ../../../gegl/operations/external/jpg-load.c:302
#7  0x00007ffff7b244f6 in gegl_operation_get_bounding_box (self=self@entry=0x6cc260) at ../../../gegl/gegl/operation/gegl-operation.c:197
#8  0x00007ffff7b6909f in gegl_graph_prepare (path=0x73c880) at ../../../gegl/gegl/process/gegl-graph-traversal.c:191
#9  0x00007ffff7b67955 in gegl_eval_manager_prepare (self=0x740080) at ../../../gegl/gegl/process/gegl-eval-manager.c:93
#10 0x00007ffff7b67a01 in gegl_eval_manager_get_bounding_box (self=self@entry=0x740080) at ../../../gegl/gegl/process/gegl-eval-manager.c:102
#11 0x00007ffff7b50357 in gegl_node_get_bounding_box (self=self@entry=0x696530) at ../../../gegl/gegl/graph/gegl-node.c:1810
#12 0x00007ffff7b52851 in gegl_node_property_changed (gobject=<optimized out>, arg1=0x727360, user_data=0x696530) at ../../../gegl/gegl/graph/gegl-node.c:1352
#13 0x00007ffff77abfa5 in g_closure_invoke () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#14 0x00007ffff77bdfc1 in ?? () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#15 0x00007ffff77c6d5c in g_signal_emit_valist () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#16 0x00007ffff77c708f in g_signal_emit () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#17 0x00007ffff77b04d4 in ?? () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#18 0x00007ffff77afd88 in ?? () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#19 0x00007ffff77b2b0b in g_object_thaw_notify () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#20 0x00007ffff7b5b206 in gegl_node_set_valist (self=self@entry=0x696530, first_property_name=<optimized out>, first_property_name@entry=0x7ffff20e505d "path", var_args=var_args@entry=0x7fffffffde20)
    at ../../../gegl/gegl/graph/gegl-node.c:1546
#21 0x00007ffff7b5bc04 in gegl_node_set (self=0x696530, first_property_name=first_property_name@entry=0x7ffff20e505d "path") at ../../../gegl/gegl/graph/gegl-node.c:1442
#22 0x00007ffff20d6e35 in do_setup (operation=operation@entry=0x64c500, path=0x73ba20 "/work/crashes/gegl000:id:000000,sig:11,src:000069,op:havoc,rep:2", uri=0x739770 "")
    at ../../../gegl/operations/core/load.c:262
#23 0x00007ffff20d7fac in my_set_property (gobject=<optimized out>, property_id=1, value=0x7fffffffe000, pspec=0x7351b0) at ../../../gegl/operations/core/load.c:345
#24 0x00007ffff77b42eb in g_object_set_property () from /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0
#25 0x00007ffff7b5b0cc in gegl_node_set_valist (self=self@entry=0x696390, first_property_name=<optimized out>, first_property_name@entry=0x73cce0 "path", var_args=var_args@entry=0x7fffffffe120)
    at ../../../gegl/gegl/graph/gegl-node.c:1537
#26 0x00007ffff7b5bc04 in gegl_node_set (self=self@entry=0x696390, first_property_name=first_property_name@entry=0x73cce0 "path") at ../../../gegl/gegl/graph/gegl-node.c:1442
#27 0x00007ffff7a6377e in param_set (pd=<optimized out>, new=0x696390, param_name=<optimized out>, param_value=0x73cc90 "gegl000:id:000000,sig:11,src:000069,op:havoc,rep:2") at ../../gegl/gegl/gegl-xml.c:145
#28 0x00007ffff7a65fa7 in start_element (context=<optimized out>, element_name=<optimized out>, attribute_names=<optimized out>, attribute_values=<optimized out>, user_data=0x7fffffffe3e0,
    error=0x7fffffffe300) at ../../gegl/gegl/gegl-xml.c:420
#29 0x00007ffff74d85a3 in ?? () from /lib/x86_64-linux-gnu/libglib-2.0.so.0
#30 0x00007ffff74d9763 in g_markup_parse_context_parse () from /lib/x86_64-linux-gnu/libglib-2.0.so.0
#31 0x00007ffff7a6f744 in gegl_node_new_from_xml (xmldata=0x684a20 "<gegl><gegl:load path='gegl000:id:000000,sig:11,src:000069,op:havoc,rep:2'/></gegl>", path_root=path_root@entry=0x64ad80 "/work/crashes")
    at ../../gegl/gegl/gegl-xml.c:576
#32 0x0000000000403060 in main (argc=<optimized out>, argv=<optimized out>) at ../../gegl/bin/gegl.c:216
```


