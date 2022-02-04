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

## Program Limitations ##

For parsing convenience, we slurp the entire file into memory, but
only if it is not larger than a preset maximum to prevent a runaway
process.

## Programming Notes ##

This program takes a somewhat naive approach in that it doesn't try to
understand the proprietary LIF format beyond that fact that complete
JPEG images can be found bracked by unique two character tags that
indicated the start-of-image (SOI) and end-of-image (EOI).  Any data
found between these image segments is discarded or can be optionally
saved to an auxillary file for later examination and study.

To perform the parsing of the binary byte stream, the following Finite
State Machine (FSM) is used.

![LIF Finite State Machine Parser](docs/parser-FSM.png?raw=true)

The above model can also be represented as a transition table.  Find
the current state at the top or the table; read down to find which
condition matches the current input character.  For example `IN: ! D8`
means if the current input character is not the `0xD8` character.  If
the input matches, perform the action described and read to the right
for the next state.

Q1 is the unique initial starting state.  Q1 and Q5 are the only
expected halting states.  In particular end of data while in Q3 or
Q4 is an error (truncated image).

`````text
+----------+------------+----------+----------+----------+
| Q1       | Q2         | Q3       | Q4       | Q5       |
| not      | start      | in       | leave    | end      |
| image    | image      | image    | image    | image    |
==========================================================
| IN: ! FF | IN: ! D8   |          |          | IN: ! FF |
| - echo   | - echo FF  |          |          | - echo   | Q1
|          | - echo     |          |          |          | not
|          |            |          |          |          | image
+----------+------------+----------+----------+----------+
| IN: FF   |            |          |          | IN: FF   |
|          |            |          |          |          | Q2
|          |            |          |          |          | start
|          |            |          |          |          | image
+----------+------------+----------+----------+----------+
|          | IN: DB     | IN: ! FF | IN: ! D9 |          |
|          | - new file | - save   | - save   |          | Q3
|          | - save FF  |          |          |          | in
|          | - save     |          |          |          | image
+----------+------------+----------+----------+----------+
|          |            | IN: FF   |          |          |
|          |            | - save   |          |          | Q4
|          |            |          |          |          | leave
|          |            |          |          |          | image
+----------+------------+----------+----------+----------+
|          |            |          | IN: D9   |          |
|          |            |          | - save   |          | Q5
|          |            |          | - close  |          | end
|          |            |          |   file   |          | image
+----------+------------+----------+----------+----------+
`````


