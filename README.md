# esp-libhelix-mp3

ESP32 (and others) component for the libhelix-mp3 mp3 decoding library.

# Usage tips

MP3Decode() only handles the decoding of the MP3 stream contained within the supplied buffer.
If the supplied data contains an ID3 tag header, the decoding may return prematurely if the
ID3 data contain bytes that resembles an MP3 synchronization data pattern.

Removing the ID3 data from the source data, or calling MP3Decode() again to resume processing
may be necessary.

See [esp-audio-player](https://github.com/chmorgan/esp-audio-player/blob/main/audio_mp3.cpp) for an example of how to do this.

```c
// decoding can be done like the below pseudocode
// errors from MP3Decode *can* occur if the mp3 sync word
// is found in other data fields, such as the ID3 data, however
// this approach will recover from that situation
while(mp3_bytes_available)
{
    ...
    int offset = MP3FindSyncWord(read_ptr, mp3_bytes_available);

    read_ptr += offset;
    mp3_bytes_available -= offset;

    int status = MP3Decode(mp3decoder, read_ptr, mp3_bytes_available, ...);
    ...
}

```
