# Leia-Image-Format-Splitter #

## Purpose ##

Splits a Leia Image Format (LIF) 3D image file into two separate JPEG
files; a left and right stereoscopic pair.  Optionally it can also
extract the two corresponding depth map images, and other binary and
text data found within the LIF file.

## Usage ##

`lif-splitter` is a command line application.

`````shell
$ lif-splitter [options] my-image.jpg [...]
`````
The default operation extracts only the left/right stereoscopic image pair
as ordinary JPEG files.

<img src="docs/lif-to-jpeg.png?raw=true"
     alt="Extracting the left and right JPEG segments."
     style="display:block;
            float:none;
            margin-left:auto;
            margin-right:auto;
            ">

To view a complete man page, use the `-manpage` option.  For a list of
options, use the `-help` option.

`````shell
$ lif-splitter -manpage
`````

## Requirements and Installation ##

This script requires Perl which is typically pre-installed on most
Linux systems and macOS up to and including Big Sur 11.  To confirm
Perl is already installed on your system, and is in your
[PATH](https://en.wikipedia.org/wiki/PATH_\(variable\)), use this
terminal command:

`````text
$ perl --version

This is perl 5...
`````

Perl can also be easily installed on Windows, macOS, and Linux using this
[step-by-step guide](https://www.perl.com/article/downloading-and-installing-perl-in-2021/).

To install this utility, just copy the single file `lif-splitter` from
the project's `src` directory to [wherever you keep your
scripts](https://shapeshed.com/using-custom-shell-scripts-on-osx-or-linux/).
Sorry there is no automated installer at this time.

## Background ##

The Leia Image Format (LIF) is a proprietary
[stereoscopic](https://en.wikipedia.org/wiki/Stereoscopy) file format
used for storing so called "4-View" images designed to be displayed on
a device using Leia's "3D Lightfield Technology" [autostereoscopic
display](https://en.wikipedia.org/wiki/Autostereoscopy).  When this
was written, this includes the [Red Hydrogen One
smartphone](https://en.wikipedia.org/wiki/Red_Hydrogen_One) and [Leia
Lume Pad tablet
computer](https://www.cnet.com/tech/computing/lume-pad-brings-glasses-free-3d-back-again-on-an-android-tablet/).
Both of these devices is equipped with a 3D camera.  Photos taken with
this camera are saved as a single JPEG file using a proprietary LIF
format to store the resulting left/right stereo image pair and
corresponding depth maps.  Conventional JPEG viewing software will
only 'see' the left view which is the first image in the data stream.
But when viewed with [Leia Inc's](https://www.leiainc.com/) own
software, all of the images are used and displayed using their 3D
lightfield technology.

The fact that the LIF format contains an independant pair of
conventional JPEG images for the left and right views, it is possible
to extract the sterescopic pair as separate left and right JPEG image
files.  This would he first step in converting such pairs into an
alternate file format such as
[MPO](https://en.wikipedia.org/wiki/JPEG#JPEG_Multi-Picture_Format) or
[JPS](https://en.wikipedia.org/wiki/JPEG#JPEG_Stereoscopic), or as a
conventional side-by-side format with a `.jpg` extension.  For these
formats the LIF format depth map images are not needed.

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

<img src="docs/parser-FSM.png?raw=true"
     alt="LIF Finite State Machine Parser"
     style="display:block;
            float:none;
            margin-left:auto;
            margin-right:auto;
            ">

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


