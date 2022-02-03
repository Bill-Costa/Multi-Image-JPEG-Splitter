# Leia-Image-Format-Splitter

## Purpose ##

Splits a Leia Image Format (LIF) 3D image file into four separate JPEG
files (Left, Right, and 2 depth maps).

## Usage and Example ##

To Be Determined; 3-FEB-2022

## Installation ##

To Be Determined; 3-FEB-2022

## Background ##

The [Leia Lume
Pad](https://www.cnet.com/tech/computing/lume-pad-brings-glasses-free-3d-back-again-on-an-android-tablet/)
is an Android tablet with an [autostereoscopic
display](https://en.wikipedia.org/wiki/Autostereoscopy).  The tablet
is equipped with a 3D camera.  Photos taken with this camera are saved
as a single JPEG file using a proprietary LIF format to store the
resulting left/right stereo image pair and corresponding depth maps.
Conventional JPEG viewing software will only 'see' the left view which
is the first file in the sequence.  But when viewed with [Leia
Inc's](https://www.leiainc.com/) own software, all of the images are
used and displayed using their 3D lightfield technology.

The fact that the LIF format contains the independant left and right
JPEG segements, the sterescopic pair can be extracted which would be
the first step in converting the image into an alternate file format
such as
[MPO](https://en.wikipedia.org/wiki/JPEG#JPEG_Multi-Picture_Format) or
[JPS](https://en.wikipedia.org/wiki/JPEG#JPEG_Stereoscopic), or as a
conventional side-by-side format in a JPG file.  For these formats the
corresponding depth map images are not needed.

## Features ##

To Do; 3-FEB-2022

## Programming Notes ##

This program takes a somewhat naive approach in that it doesn't try to
understand the proprietary LIF format beyond that fact that complete
JPEG images can be found bracked by unique two character tags that
indicated the start-of-image (SOI) and end-of-image (EOI).  Any data
found between these image segments is discarded or can be optionally
saved to an auxillary file for later examination and study.

To perform the parsing of the binary byte stream, the following Finite
State Machine (FSM) is used.

![LIF Finite State Machine Parser]()

Some more text here.
