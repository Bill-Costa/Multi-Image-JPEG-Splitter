No formal tests yet.  From within this directory, try the following
commands:

## Preflight Check ##

````text
$ ../src/mij-splitter ../samples/*
? mij-splitter: cannot split composite 3D images (jps)
_ ../samples/02-super-mario.jps
? mij-splitter: expect '.jpg' or '.mpo' file type
_ ../samples/README.md
! mij-splitter: halting; no files processed
$
````
## Normal Run ##

````text
$ ../src/mij-splitter ../samples/*.jpg ../samples/*.mpo
? mij-splitter: does not look like JPEG format
_ ../samples/00-empty-file.jpg
_ skipped
? mij-splitter: only one image found in file
_ ../samples/01-cameraman.jpg
../samples/01-cameraman.jpg (0 files)
? mij-splitter: cannot assume input in LIF format
_ ../samples/03-cat-stack.jpg
_ created 5 sequentially numbered image files
../samples/03-cat-stack.jpg (5 files)
../samples/05-chrome-engine_RH1.jpg (2 files)
../samples/06-oregano_LumePad.jpg (2 files)
../samples/07-valentines_LumePad.jpg (2 files)
../samples/04-monticello.mpo (2 files)
DevLab:test wfc$ ls -1
03-cat-stack_1-of-5.jpg
03-cat-stack_2-of-5.jpg
03-cat-stack_3-of-5.jpg
03-cat-stack_4-of-5.jpg
03-cat-stack_5-of-5.jpg
04-monticello_L.jpg
04-monticello_R.jpg
05-chrome-engine_RH1_L.jpg
05-chrome-engine_RH1_R.jpg
06-oregano_LumePad_L.jpg
06-oregano_LumePad_R.jpg
07-valentines_LumePad_L.jpg
07-valentines_LumePad_R.jpg
README.md
$ rm *.jpg
$
````
## Force and Accept Anything ##

````text
$ ../src/mij-splitter -a -f ../samples/*
? mij-splitter: no JPEG image markers found
_ ../samples/00-empty-file.jpg
../samples/00-empty-file.jpg (0 files)
? mij-splitter: only one image found in file
_ ../samples/01-cameraman.jpg
../samples/01-cameraman.jpg (0 files)
? mij-splitter: only one image found in file
_ ../samples/02-super-mario.jps
../samples/02-super-mario.jps (0 files)
../samples/03-cat-stack.jpg (5 files)
../samples/04-monticello.mpo (2 files)
../samples/05-chrome-engine_RH1.jpg (4 files)
../samples/06-oregano_LumePad.jpg (4 files)
../samples/07-valentines_LumePad.jpg (4 files)
? mij-splitter: no JPEG image markers found
_ ../samples/README.md
../samples/README.md (0 files)
DevLab:test wfc$ ls -1
03-cat-stack_1-of-5.jpg
03-cat-stack_2-of-5.jpg
03-cat-stack_3-of-5.jpg
03-cat-stack_4-of-5.jpg
03-cat-stack_5-of-5.jpg
04-monticello_1-of-2.jpg
04-monticello_2-of-2.jpg
05-chrome-engine_RH1_1-of-4.jpg
05-chrome-engine_RH1_2-of-4.jpg
05-chrome-engine_RH1_3-of-4.jpg
05-chrome-engine_RH1_4-of-4.jpg
06-oregano_LumePad_1-of-4.jpg
06-oregano_LumePad_2-of-4.jpg
06-oregano_LumePad_3-of-4.jpg
06-oregano_LumePad_4-of-4.jpg
07-valentines_LumePad_1-of-4.jpg
07-valentines_LumePad_2-of-4.jpg
07-valentines_LumePad_3-of-4.jpg
07-valentines_LumePad_4-of-4.jpg
README.md
$ rm *.jpg
$
```
