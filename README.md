PLA Filament Recycler & Extruder
I got tired of throwing away failed prints and scraps of PLA, so I designed a machine that can recycle those scraps and turn them back into usable filament. You can also use raw PLA pellets as a material, which is much cheaper than buying a spool. Everything was designed from scratch, including the CAD, electronics, and code.
What it does
The machine takes in shredded 3D print scraps or raw PLA pellets, melts them down, and extrudes new 1.75mm filament. You can use this to recycle failed prints or support material and turn them back into usable filament. I designed this machine to save money and reduce waste from scraps of PLA I had already paid for.
How to use it

Shred old prints into small chips, or better yet, just use raw pellets. Put these into the top funnel.
Plug it in and it will automatically heat up to 210°C.
The plastic is pushed through a water-cooled steel barrel by an auger into the hot end. Melted plastic is pushed out through a 1.72mm nozzle.
Two 120mm fans cool the filament just after it exits the nozzle.
It is cooled again on its way to the spool via a stepper-driven puller, which regulates the filament diameter.
Spool it up by hand.

How it works
Feeding
Plastic goes into a hopper and gets pushed through a steel barrel by an auger. The barrel is water-cooled so that the plastic frame won't melt.
Heating
The hot end has two 12V cartridge heaters sitting in a custom-made heater block. Temperatures are monitored in real time by K-type thermocouples, which the firmware uses to maintain a constant 210°C.
Cooling & Diameter
The filament is rapidly cooled by two 120mm fans directly after exiting the nozzle. The stepper puller controls the filament diameter by varying the speed at which it pulls the filament away from the nozzle — the faster it pulls, the thinner the filament gets. You then spool it up by hand.

CAD File Notes
In the CAD file there are folders named parts, electronics, and non-printed parts. The parts folder contains everything I'll be 3D printing. The electronics folder has STEP files of all the electronics. The non-printed parts I'll either fabricate myself, have CNC'd, or purchase. Here is the link to the OnShape file https://cad.onshape.com/documents/712a637f79dd720bf38c4e9e/w/a795a65476a95b8c58273d13/e/a84f1c8939a0f9925b142090
the code was uploded using arduino ide to a arduino uno r3
