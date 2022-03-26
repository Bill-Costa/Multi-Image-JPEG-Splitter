# Multi-Image JPEG Splitter #

## Purpose ##

This utility extracts individual JPEG images that have been stacked
within a single file.  Supported input formats include
[3D Leia Image Format](https://docs.leialoft.com/developer/android-media-sdk/supported-media)
(LIF) and
[MPO](https://en.wikipedia.org/wiki/JPEG#JPEG_Multi-Picture_Format)
files.

## Usage ##

`mij-splitter` is a command line application.

`````shell
$ mij-splitter [options] my-image.jpg [...]
`````

The default operation extracts only the left/right stereoscopic image pair
as ordinary JPEG files.

![LIF to JPEG L/R Pairs](docs/mij-to-jpeg.png?raw=true)

To view a complete manual page, use the `-manpage` option.

`````shell
$ mij-splitter -manpage
`````

For a list of available options, use the `-help` option.

## Requirements and Installation ##

This script requires a Perl version 5 language interpreter which is
typically pre-installed on most Linux systems and macOS up to and
including Big Sur 11.  To confirm if Perl is already installed on your
system, and is in your
[PATH](https://en.wikipedia.org/wiki/PATH_\(variable\)), use the
following terminal command:

`````text
$ perl --version

This is perl 5...
`````

Perl can also be easily installed on Windows, macOS, and Linux by
following this [step-by-step
guide](https://www.perl.com/article/downloading-and-installing-perl-in-2021/).

To install this utility, just copy the single file `mij-splitter` from
the project's `src` directory to [wherever you keep your
scripts](https://shapeshed.com/using-custom-shell-scripts-on-osx-or-linux/).
Sorry there is no automated installer at this time.

## Background ##

[Stereoscopic photography](https://en.wikipedia.org/wiki/Stereoscopy)
uses two or more images to capture the representation of a single
three dimensional (3D) image.  There are a variety of organizational
schemes that can be used to store a stereoscopic left/right image
pair within a single image file.  Popular methods include
concatenating the images together in a
[side-by-side](https://en.wikipedia.org/wiki/Stereoscopy#Side-by-side)
format, or using a [color
anaglyph](https://en.wikipedia.org/wiki/Stereoscopy#Color_anaglyph_systems)
encoding.  When displayed, it is easy to identify such images as being
special in some way.  And while it may be clear to the observer that
there are multiple images being stored within the file, this composite
has actually been encoded as a single stand-alone digital image.

> _**Note that this program is *not* designed for processing those
> sorts of conventional 3D image files.**_

As an alternative, there are at least two digital file formats that
take a different approach.  Both the [3D Leia Image
Format](https://docs.leialoft.com/developer/android-media-sdk/supported-media)
(LIF) and [MPO
format](https://en.wikipedia.org/wiki/JPEG#JPEG_Multi-Picture_Format)
stack complete individual JPEG images, in sequence, within a single
file.  When displayed using conventional JPEG viewing software, only
the first (left) image is displayed.

This utility can be used to extract the embedded stereo image pair
into two separate left and right JPEG files.  Optionally, in the case
of LIF files, it can also extract the two corresponding depth map
images that are part of this proprietary format.

### Leia Image Format ###

The Leia Image Format (LIF) is a proprietary
[stereoscopic](https://en.wikipedia.org/wiki/Stereoscopy) file format
used for storing so called "4-View" images designed to be displayed on
a device using Leia's "3D Lightfield Technology" [autostereoscopic
display](https://en.wikipedia.org/wiki/Autostereoscopy).  When this
was written, this includes the [Red Hydrogen One
smartphone](https://en.wikipedia.org/wiki/Red_Hydrogen_One) and [Leia
Lume Pad tablet
computer](https://www.cnet.com/tech/computing/lume-pad-brings-glasses-free-3d-back-again-on-an-android-tablet/).
Both of these devices are equipped with a 3D camera.  Photos taken with
this camera are saved as a single JPEG file using the proprietary LIF
file format to store the resulting left/right stereo image pair and
corresponding depth maps.  Conventional JPEG viewing software will
only 'see' the left view which is the first image in the data stream.
But when viewed with [Leia Inc's](https://www.leiainc.com/) own
software, all of the images are used for displaying the 3D image using
their lightfield hardware technology.

The fact that the LIF format contains a pair of conventional JPEG
images for the left and right views makes it is possible to extract
these as two independent JPEG image files.  This would he first step
in converting such pairs into an alternate stereoscopic file format
such as
[MPO](https://en.wikipedia.org/wiki/JPEG#JPEG_Multi-Picture_Format) or
[JPS](https://en.wikipedia.org/wiki/JPEG#JPEG_Stereoscopic), or as a
conventional side-by-side format image with a `.jpg` extension.  For
these formats the LIF format depth map images are not needed so they
are not extracted by default.

Finally note that [LeiaPlayer 3.0](https://forums.leialoft.com/t/leiaplayer-3-0-now-available/1402)
app, which runs on the Lume Pad, has a "SBS Export" option for
extracting the left/right pair as a single side-by-side format JPEG
image.  From there, one of the stereoscopic editors, listed below,
could extract individual left/right images if needed.

### Multi-Picture Format (MPO) ###

This utility can also extract all of the individual images from the
JPEG-based Multi-Picture format (MPO) while preserving any embedded
thumbnail/preview images.  Unlike LIF files, MPO is a well established
3D digital image format and is widely recognized by stereoscopic
computer applications.  These apps, described below, are probably a
better choice for manipulating MPO than this program.

## Working with Digital Stereoscopic Images ###

So, once you have a pair of left/right image files, now what?

### Stereoscopic Photo Editors ###

Specialized stereoscopic software will allow you to assemble, crop,
and align the two images into a 3D image that is [comfortable to
view](https://stereosite.com/taking-stereo-photos/stereo-window-basics/).

- [StereoPhoto Maker](https://stereo.jpn.org/eng/stphmkr/)
  - [How to Use](https://stereoscopy.blog/2019/09/08/how-to-use-stereo-photo-maker-basic-tutorial/)
- [COSIMA](http://www.cosima-3d.de/)
- [StMani](https://sourceforge.net/projects/stmani3/)
- [Using Photoshop for Stereoviews](https://stereoscopy.blog/2020/06/05/how-to-make-stereoviews-only-using-photoshop/)

### Stereoscopic Photo Viewers ###

Stereoscopic photo viewers can be used to more easily watch 3D image
slide-shows, or explore a collection of 3D images.  Some viewers can
also be used to do simple conversions from one common 3D format to
another.

- [sView](https://en.wikipedia.org/wiki/SView)
- [S3D-Viewer](http://www.stereo-3d.net/)
- [LeiaPlayer 3.0 for the Lume Pad](https://forums.leialoft.com/t/leiaplayer-3-0-now-available/1402); also extracts side-by-side (SBS) format images from LIF files.

## Program Bugs and Limitations ##

- Method for detecting JPEG segments may be overly simplistic.  (See below.)
- We should also output composite 3D formats such as doing a lossless
  side-by-side joining of the JPEG left/right images.
- There is currently no option for recursively extracting all images
  found within a directory tree.
- Temp files may be left behind if program crashes.

## Programming Notes ##

This program takes a very naive approach in that it doesn't try to
understand the proprietary LIF format, nor even common JPEG formats
for that matter, beyond that fact that complete JPEG images
[are bracketed by unique two byte tags](https://www.media.mit.edu/pia/Research/deepview/exif.html).
These tags indicated the start-of-image (SOI) and end-of-image (EOI).
In a LIF file, any data found between, or after these concatenated
JPEGs is discarded or can be optionally saved to an auxiliary file for
further examination and study.

To perform the parsing of the binary byte stream, the following
[Finite-State Machine](https://en.wikipedia.org/wiki/Finite-state_machine)
(FSM) is used.  In this model each transition is made when the next
single byte is read from the input stream.

![LIF Parser](docs/parser-FSM.png?raw=true)

In the FSM illustrated above:

- "other" means a byte that is not an expected JPEG image delimiter
  tag character..
- "save" means write current image data byte(s) to the active image output file
- "echo" means optionally write any non-image data to an 'extra' file.

Note that Q1 is the unique initial starting state.  Q1 and Q5 are the
expected halting states.  In particular reaching the end of the input
stream while in Q3 or Q4 is an error (truncated image).

The above [directed graph](https://en.wikipedia.org/wiki/Directed_graph)
can also be represented as the following state transition table.

`````text
+----------+------------+----------+----------+----------+----------+----------+
| Q1       | Q2         | Q3       | Q4       | Q5       | Q6       | Q7       |
| not      | start      | in       | leave    | end      | in       | leave    |
| image    | image      | image    | image    | image    | thumb    | thumb    |
================================================================================
| IN: ! FF | IN: ! D8   |          |          | IN: ! FF |          |          |
| - echo   | - echo FF  |          |          | - echo   |          |          | Q1
|          | - echo     |          |          |          |          |          | not
|          |            |          |          |          |          |          | image
+----------+------------+----------+----------+----------+----------+----------+
| IN: FF   |            |          |          | IN: FF   |          |          |
|          |            |          |          |          |          |          | Q2
|          |            |          |          |          |          |          | start
|          |            |          |          |          |          |          | image
+----------+------------+----------+----------+----------+----------+----------+
|          | IN: DB     | IN: ! FF | IN: other|          |          | IN: D9   |
|          | - new file | - save   | - save   |          |          | - save   | Q3
|          | - save FF  |          |          |          |          |          | in
|          | - save     |          |          |          |          |          | image
+----------+------------+----------+----------+----------+----------+----------+
|          |            | IN: FF   |          |          |          |          |
|          |            | - save   |          |          |          |          | Q4
|          |            |          |          |          |          |          | leave
|          |            |          |          |          |          |          | image
+----------+------------+----------+----------+----------+----------+----------+
|          |            |          | IN: D9   |          |          |          |
|          |            |          | - save   |          |          |          | Q5
|          |            |          | - close  |          |          |          | end
|          |            |          |   file   |          |          |          | image
+----------+------------+----------+----------+----------+----------+----------+
|          |            |          | IN: D8   |          | IN: other| IN: other|
|          |            |          | - save   |          | - save   | - save   | Q6
|          |            |          |          |          |          |          | in
|          |            |          |          |          |          |          | thumb
+----------+------------+----------+----------+----------+----------+----------+
|          |            |          |          |          | IN: FF   |          |
|          |            |          |          |          | - save   |          | Q7
|          |            |          |          |          |          |          | leave
|          |            |          |          |          |          |          | thumb
+----------+------------+----------+----------+----------+----------+----------+
`````

(Note: There are 7 columns and a set of row labels on the right hand side in
the table above.)

### Reading the State Table ###

Find the current state at the top or the table; read down to find
which condition matches the current input byte.  For example `IN:
! D8` means the current input byte is not the `0xD8` character.
If the current input byte matches the condition, perform the
action described, and then read to the right to determine the next
state.  Return to the top of the table to find that state, continue
with the next input.

The beauty of using a FSM model is that the input loop can contain a
case or [switch
statement](https://en.wikipedia.org/wiki/Switch_statement) with a
branch defined for each state.  When a branch is taken, it can examine
the current input byte to determine the action to be performed, and
determine what needs to be the next state.  At any point in the input
stream, the only information needed is knowing the current state,
and what is the current input byte.  Using this approach, a FSM can be
almost mechanically transcribed into code.

### More About Finite State Machine Theory ###

- Fun fact, [regular expressions describe patterns which can be recognized by finite state machines](https://www.cs.drexel.edu/~kschmidt/CS360/Lectures/2.html).
- [Why you really can parse HTML \(and anything else\) with regular expressions](https://neilmadden.blog/2019/02/24/why-you-really-can-parse-html-and-anything-else-with-regular-expressions/).

## DISCLAIMER ##

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
