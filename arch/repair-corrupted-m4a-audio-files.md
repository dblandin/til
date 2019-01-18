# Repair corrupted m4a audio files

A colleague of mine at Artsy dropped a message in the #dev Slack channel
describing a problem. Audio files recorded on an iPhone with the Voice Memos app
weren't opening with iTunes or any other media player.

Another colleague suggested to try VLC, which can usually handle any audio or
video you throw at it, but in this case, the audio seemed to be corrupted.

VLC gave the following error:

```
VLC media player 3.0.6 Vetinari (revision 3.0.6-0-g5803e85f73)
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7f68f4c53440] moov atom not found
[00007f68d4c14fe0] avformat demux error: Could not open audio.m4a: Unknown error 1094995529
```

[faad][], an open source MPEG-4 and MPEG-2 AAC decoder, gave the following error
on the provided audio file:

[faad]: https://www.audiocoding.com/faad2.html

```
 *********** Ahead Software MPEG-4 AAC Decoder V2.8.8 ******************

**** MP4 header ****
Brand:			M4A (version 0)
Compatible brands:	M4A mp42isom
invalid atom size 0 @28
parse:-1
Error opening file: /home/devon/Downloads/Alison Armstrong.m4a
```

After some searching around, I came across [an Ask Ubuntu thread][] with the
following conversion process:

[an Ask Ubuntu thread]: https://askubuntu.com/questions/1023648/recovering-corrupted-m4a-recordings

```
dd ibs=1 skip=44 if=corrupted.m4a of=raw.m4a
faad -a fixed.m4a raw.m4a
```

The Ask Ubuntu answer links to [a VideoHelp support thread][] with this insight:

> The idea is to strip out the first 44 bytes of the header, for instance with a
> hex editor on Windows, and then to rebuild an ADTS stream from the stripped file
> using FAAD.

Interesting!

[a VideoHelp support thread]: https://forum.videohelp.com/threads/355597-Moov-atom-not-found#post2238927

The commands above restored some audio but my colleague reports that there is
still some missing. I suspect that any missing data may be on the iPhone device
and that `faad` converted what it had access to as the file size between the
original file and the repaired file remained the same.
