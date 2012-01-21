---
layout: page
title: BRE; Ben's ROM Editor
description: Ben Ogle builds things. Here are his projects.
keywords: Ben Ogle
redirects:
- /projects/bre
---

BRE is a closed-source, freeware Engine Management System for Old Hondas written by me in C++,
and C#, with ECU code written in assembly.

What is an Engine Management System? When a car's engine is modified by, say, adding a turbo,
changing cams, or even using a different exhaust system, the engine will have different needs
for fuel and ignition delivery. BRE allows a user to change the code inside the car's engine
computer (ECU) to modify the way it delivers fuel and spark. BRE also has extra features to make
tuning the ECU easier such as a Datalogger for real-time data gathering, and integration with an
EEPROM emulator for real-time ECU code updates.

The ECU decides how much fuel and ignition advance to dole out based mostly on static lookup
tables. The ECU code picks a cell in each of the main fuel and ignition tables based on how much
throttle given (intake manifold vacuum; x axis), and engine speed (RPM; y axis).

Here is a fuel table as displayed in BRE. Values are in milliseconds of injector duration:

![Fuel table](/images/screenshots/bre_fuel.png)

And an ignition table. Values are in degrees of advance before top dead center:

![Ignition table](/images/screenshots/bre_ign.png)

Datalogger
----------

The datalogger allows the user to gather data such as RPM, throttle position, air/fuel mixture,
engine temp, etc. from a running engine. BRE also has a digital dash to show this real-time
data. Epic screenshot of my car pre-turbo doing a 2nd gear pull:

![Datalogger](/images/screenshots/bre_logger.png)

Data analysis
-------------

BRE has a feature that will analyze a saved log's air/fuel mixture, then automatically adjust
the fuel tables to your target air/fuel mixture settings. Values without color are interpolated
based on fuel table values and surrounding air/fuel mixture results.

Additionally, this auto-tune functionality is available in real-time. So a user can just drive
around their neighborhood and have BRE tune on the fly!

![Analyzer](/images/screenshots/bre_analyzer.png)

Other features
--------------

There are many other lookup tables that affect fuel delivery and ignition advance. These include
corrections for coolant temp, air temp, battery voltage, and atmospheric pressure. BRE allows
users to modify these tables as well.

Other features include enabling and disabling of factory code, and new additions including fancy
use of extra outputs, code for turbo cars, and features for handling large injectors.

BRE also incorporates a C# library that compiles and loads C# scripts into BRE. Users can add
features and change out-of-the-box behavior. One such example is modifying the datalogger
definition script to capture new data such as the raw value of some sensor. Or maybe adding
support for a new wideband oxygen sensor.

ECU Reverse Engineering
-----------------------

The ECU code is stored on a 32k EEPROM. A binary dump of this EEPROM was disassembled using a
community-built disassembler. Then I got to work digging through sea of cryptic
machine-generated code.

Most of the thought work in this project was in reverse engineering this code and writing new
features for it. This work was done with very little hardware knowledge and a PDF of the
microcontroller manual. If you would like to dig through the ECU code, it is in a public github
repository.

Will BRE work in/for my car?
-----------------------------

It works with only a very specific family of engine computers. If you have an 88-91 Civic or a 90-91
Integra and you use an OBD0 VTEC ecu from Japan or Europe, then yes. BRE supports the PR3, and
both the Japanese and European PW0.

Links to BRE related stuff

* [ECU code github repository](http://github.com/benogle/obd0vtec_dev)
* [BRE support forum](http://forum.pgmfi.org/viewforum.php?f=36)
