OVERVIEW
===
This is a JSON printer definition file for the Original Prusa i3 Mk2, for use with the new Cura 2.3
beta configuration style. This definition needs to be placed in the `cura/resources/definitions/`
directory in your Cura 2.3 installation.

Mac OSX
---
On Mac OSX this location is found inside the Application Bundle itself, and the cura directory lives
in `/Applications/Cura.app/Contents/Resources/cura`. Cura must be restarted for the new printer
definition to be available.

Windows
---
On Windows this depends on where you installed Cura, but by default is probably in `C:\Program
Files\Cura\cura`. I'm sure you can find it, and most likely Cura needs to be restarted on Windows as
well.

Details
===
Since the current Cura 2.3 beta does not provide access to the *huge* number of settings available
in the new printer definitions, I decided to make my own for my Original Prusa i3 Mark 2. I had to
override quite a few of the default values which are inherited from the base `fdmprinter.def.json`
definition.

I set some of the parameters based on the Slic3r quality settings provided by Prusa Research, such
as the initial layer height of `0.15mm`. Most of the rest of the settings were pulled from my EEPROM
defaults, which I have not changed. They are current as of firmware 3.0.8.

Gcode
---
For the starting gcode, I came up with a mixture of steps to accomplish several things:

- Most importantly, CuraEngine only uses absolute positioning *and* extruding values. So I use M82
  to set absolute extrusion mode.
- I adopted the extruder and bed pre-heating steps from Slic3r because Cura's automatic ones make
  you wait for one, then the other, which takes unnecessary time. If you provide your own temp
  codes, it doesn't generate any for you.
- I added an absolute version of the off-print-area steps to draw two thick lines to "wipe" the nozzle
  and clear off any lingering filament before beginning the print.
- Lastly, surrounding the wiping steps are some explicit extrusion distance resets, since we're
  using absolute extrusion now and don't want to confuse the print code since Cura won't know about
  our extrusion for wiping.

The ending gcode is pretty stock, probably lifted from Slic3r or Simplify3D.

Gantry Dimensions
---
I decided to pretend the extruder dimensions and gantry calculations didn't exist, so I set the
relevant values to 0, and set the `machine_use_extruder_offset_to_offset_coords` value to True. In a
multi-extruder setup or maybe under particular circumstances (idling or wipe towers or whatnot) this
might be an issue, but until myself or someone else decides to measure the carriage, it seems to be
working fine.
