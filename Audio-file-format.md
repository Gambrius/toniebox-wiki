# SD card structure
Every box contains a SD card which is used to store the tonies audio data.

## downloaded audio files
The downloaded audio files for tonies are stored on the SD card in a directory for every tonie.

Directories are named by the last 4 bytes of the tonie ISO15693 UID in hex format, e.g. <SD>\DEADBEEF.
Within that folder there are files in the format 000000XX where XX is a value coded in hexadecimal.

## predefined files
Upon initalization the box extracts the content of the flash-internal sfx.bin into two directories with several audio files:

    00000000\000000*
    00000001\000000*

# Audio file format
In general the audio files are just OGG files with some custom header

So the files are structured like this:

    <file>   ::= <header> <audio_data>
    <header> ::= <header_len> <header_data>
    
    where:
    <header_len>  length of <header_data> in big endian uint32, usually 0x00000FFC
    <header_data> protobuf coded info fields like SHA1 hash, audio length, etc
    <audio_data>  Ogg audio file

# Header format

The file header is coded using protobuf and contains these fields:

    1. [string]   Audio data SHA-1 hash
    2. [variant]  Audio data length in bytes
    3. [variant]  Audio-ID of OGG audio file
    4. [string]   Ogg page numbers for Chapters
    5. [string]   fill bytes „00“ up to <header_len>
   
To decode the protobuf content, you can use the online decoder at https://protogen.marcgravell.com/decode


