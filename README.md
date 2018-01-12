# show-focuspoints
When photo editing, see where the camera's Autofocus points were. Marks present, enabled, and used points.


This is a little helper program that you run on your images that were
taken with a camera that has autofocus and leaves the autofocus points
in the picture's metadata.  You can generally see the AF points on the
camera's display, but they are usually not available in tools.

This tool has two modes (select with "-m"):

- mode 1: make a new photo containing a copy of the original photo and
  overlay focus points.  You can then watch it in any picture viewer.

- mode 2: make a new picture that has a transparent background and
  semi-transparent focus point markers (and does not contain the
  original picture).  You then import that as a layer into GIMP or
  whatever, and from then on you can turn it on and off, change
  transparency of my layer etc.  This layer will also survive cropping
  and rotating while moving the focus point markers along with the
  rest of the picture.  I plan to change my workflow to always put one of
  these in first thing before doing any cropping.

  You can also use mode 2 transparent layers to play more games with
  GMIC.

Requirements:
- GMIC 2.0+ http://gmic.eu/
- exiftool https://www.sno.phy.queensu.ca/~phil/exiftool/
- should work in Python2 and Python3, but really who can tell?


CURRENT LIMITATIONS:

This is just my working version for now.  Sophistication will be added
after I get a feeling for what people want.

Current limitations:
- Canon only for now.  Send me other Cameras' pictures.
- How the marking is done is not configurable right now, it's just one
  scheme I used.  Let me know what you want.
- You should be, but current are not, able to request that only focus
  points actually used are plotted.  It plots all the camera's focus
  points in semi-visible red.  I find that it helps in finding out
  what's going on.  Send me wishes.
- GMIC must be version 2.0+.  Debian-stable and some other OSes do not
  have it yet.
- exiftool is also not too common in distributions.
- When using mode 2 to make a mostly transparent layer you must use an
  output image format that supports transparency.  I don't check that.

PERMANENT LIMITATIONS:

When cropping or rotating pictures, editing tools generally do not
adjust the markings for the focus points.  In GIMP, for example, they
are left in, but not adjusted, so that the points show up in the wrong
place.  However, as I said earlier, if you import a transparent layer
from this script into your editor session before you do any cropping
or rotating then this layer will from then on move correctly.


SECURITY:

This is a Python script parsing the text output of a commandline
utility (exiftool), that is calls via popen.  It then (optionally)
calls GMIC on a commandline generated, including the original picture
name.  This is a generally a high-risk situation because an attacker
can try to send a picture with special characters in the filename,
which will then try to escape shell quoting, executing shell commands.

At the time of this writing this script does not invoke an shells.
Both the initial popen and the optional GMIC invokation are done with
pre-parsed arrays of commandline arguments.  Some testing has been
done to make sure that filenames, no matter how exotic, are
interpreted verbation as one commandline argument and are not being
passed through a shell.

This script has the option to either invoke GMIC for you, or to print
a commandline to stdout that you can run on your own later (flag "-n").
The second option is higher risk because now you are using a shell.

Both exiftool and GMIC might be vulnerable to exploits in picture
data, picture metadata or picture names, regarless of whether you use
my script or not.  There is a lot of parsing going on there.  In a
public setting privilege separation seems in order.
