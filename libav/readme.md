libav pocs
===========

## 1-

gdb --args  ../avconv -i 1-avconv-divbyzero.wav -i 1-libav-divbyzero.flac -f avi merge.avi


Program received signal SIGFPE, Arithmetic exception.
0x000000000046b38b in process_input_packet (ist=0x6cdd60, no_eof=no_eof@entry=0, pkt=0x0)
    at avtools/avconv.c:1586
1586                ist->next_dts += ((int64_t)AV_TIME_BASE * ist->dec_ctx->frame_size) /
1587                                 ist->dec_ctx->sample_rate;



