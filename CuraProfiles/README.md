## Maskin gcode

### Bambu

```gcode
G21 ;metric values
G90 ;absolute positioning
M82 ;set extruder to absolute mode
M107 ;start with the fan off
G28 X0 Y0 ;move X/Y to min endstops
G28 Z0 ;move Z to min endstops
G0 Z0.2 F1800 ; move nozzle to print position
G92 E0 ; specify current extruder position as zero
G1 Y10 X180 E50 F1200 ; extrude a line in front of the printer
G92 E0 ; specify current extruder position as zero
G0 Z20 F6000 ; move head up
G1 E-7 F2400 ; retract
G04 S2 ; wait 2s
G0 X0 F6000 ; wipe from oozed filament
G1 E-1 F2400 ; undo some of the retraction to avoid oozing
G1 F6000 ; set travel speed to move to start printing point
M117
```

```gcode
G4 ; wait
G92 E0
G1{if max_layer_z < printable_height} Z{z_offset+min(max_layer_z+30, printable_height)}{endif} ; move print head up
M104 S0 ; turn off temperature
M140 S0 ; turn off heatbed
M107 ; turn off fan
G1 X0 Y200 F3000 ; home X axis
M84 ; disable motors
```

### Cura

Start:

```gcode
G21 ;metric values
G90 ;absolute positioning
M82 ;set extruder to absolute mode
M107 ;start with the fan off
G28 X0 Y0 ;move X/Y to min endstops
M300 S1318 P266
G28 Z0 ;move Z to min endstops
G0 Z0.2
G92 E0 ;zero the extruded length
G1 X40 E25 F400 ; Extrude 25mm of filament in a 4cm line. Reduce speed (F) if you have a nozzle smaller than 0.4mm!
G92 E0 ;zero the extruded length again
G1 E-1 F500 ; Retract a little
G1 X80 F4000 ; Quickly wipe away from the filament line
M117 ; Printing…
G5
```

Stopp:

```gcode
M104 S0 ; turn off extruder
M140 S0 ; turn off bed
M84 ; disable motors
M107
G91 ;relative positioning
G1 E-1 F300 ;retract the filament a bit before lifting the nozzle, to release some of the pressure
G1 Z+0.5 E-5 ;X-20 Y-20 F{speed_travel} ;move Z up a bit and retract filament even more
G28 X0 ;Y0 ;move X/Y to min endstops, so the head is out of the way
G1 Y180 F2000
M84 ;steppers off
G90
M300 S1318 P266
```

### PETG

- Z offset fungerte på 0.1, hvis det klistrer seg kan den økes til 0.2 - Bruk z offset plugin, Cura har ikke direkte støtte for dette.
- Bruk kun "brim" hvis liten kontaktflate, større deler er det bare bortkastet.

PETG er ikke som PLA/ABS, det skal legge seg oppå forrige lag og smelte sammen, ikke presses inn i forrige lag.
