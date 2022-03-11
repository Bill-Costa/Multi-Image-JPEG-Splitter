# Splitting Stacked JPEG Images from Leia Image Format (LIF) File #

**NOTE:** This technique of splitting does not appear to work for MPO
files.

**PRO-TIP:** Save output from `hexdump -C` to a file for easy
looky-see of JPEG file structure.

## References ##

- [Visualizing and Understanding JPEG Format](https://github.com/corkami/formats/blob/master/image/jpeg.md)

- [Unraveling the JPEG](https://parametric.press/issue-01/unraveling-the-jpeg/)

- [For LIF format files] ... you can search the file for occurrences
  of the unique JPG starting signature (two bytes: FF D8), and split
  the file into its four constituent JPG
  sub-files. [Link-to-Reference](https://photo-3d.groups.io/g/main/message/131743)

- JPEG files (compressed images) start with an image marker which
  always contains the marker code hex values FF D8 FF. It does not
  have a length of the file embedded, thus we need to find JPEG
  trailer, which is FF D9.
  [Link-to-Reference](https://www.file-recovery.com/jpg-signature-format.htm)

- Magic number FF D8 FF [Link-to-Reference](https://www.ntfs.com/jpeg-signature-format.htm)

- JPEG File Interchange Format (JFIF) [Link-to-Reference](https://en.wikipedia.org/wiki/JPEG_File_Interchange_Format)

- All JPEG files should start FFD8FFE0. The JPEG file format is
  properly called JFIF, and is [described in detail here](https://www.ecma-international.org/publications/files/ECMA-TR/ECMA%20TR-098.pdf).  
  [Link-to-Reference](https://community.adobe.com/t5/photoshop-ecosystem-discussions/is-there-a-way-to-create-jpeg-files-without-the-photoshop-file-signature/td-p/11001989)

- **NOTES on JPEG file headers:** The proper JPEG header is the
  two-byte sequence, 0xFF-D8, aka Start of Image (SOI) marker.  JPEG
  files end with the two-byte sequence, 0xFF-D9, aka End of Image
  (EOI) marker.

  Between the SOI and EOI, JPEG files are composed of
  segments. Segments start with a two-byte Segment Tag followed by a
  two-byte Segment Length field and then a zero-terminated string
  identifier (i.e., a character string followed by a 0x00), as shown
  below with the JFIF, Exif, and SPIFF segments.

  Segment Tags of the form 0x-FF-Ex (where x = 0..F) are referred to
  as APP0-APP15, and contain application-specific information. The
  most commonly seen APP segments at the beginning of a JPEG file are
  APP0 and APP1 although others are also seen. Some additional tags
  are shown below:

  - `0xFF-D8-FF-E0` — Standard JPEG/JFIF file, as shown below.
  - `0xFF-D8-FF-E1` — Standard JPEG file with Exif metadata, as shown below.
  - `0xFF-D8-FF-E2` — Canon Camera Image File Format (CIFF) JPEG file
    (formerly used by some EOS and Powershot cameras).
  - `0xFF-D8-FF-E8` — Still Picture Interchange File Format (SPIFF),
    as shown below.

  [Link-to-Reference](https://www.garykessler.net/library/file_sigs.html)

## Discussions ##

- [LeiaCam should use MPO](https://forums.leialoft.com/t/leiacam-should-use-mpo/1766)

<!-- EOF: references.md -->
