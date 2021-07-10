## FreeCAD/PureData connection framework

FCPD is a set of [FreeCAD](https://github.com/FreeCAD/FreeCAD) python macros and [Pure-Data](https://github.com/pure-data/pure-data) (aka PD) patches to link the two software.

This repository is no more maintained, please use [FCPDWorkbench](https://github.com/FlachyJoe/FCPDWorkbench).

## What is it for ?

FreeCAD is more than a CAD and Pure-Data is more than *"a free real-time computer music system"*.
Using a real-time visual programming language to control FreeCAD can results in an orchestrated animation system or a procedural modeling one.
Or the reverse, imagine a real-time performance with visual/musical effects controlled by FreeCAD virtual objects.

As you can start to see, imagination and  programming skill are one's only limitation when using FCPD.

<p align=center>
    <i>Sample</i></br>
    <img src="https://raw.githubusercontent.com/FlachyJoe/FCPD/main/doc/sample-animate.gif" width=800/>
</p>

## Requirements

External libraries are needed for Pure-Data :
* [list-abs](https://puredata.info/downloads/list-abs)
* [iemlib](https://puredata.info/downloads/iemlib)
See Pure-Data documentation to install them or use an already populated distribution as [Purr-Data](http://l2ork.music.vt.edu/main/make-your-own-l2ork/software/).

## Installation

* Copy (or link) the [FCPD/FreeCAD folder](FreeCAD) content in your FreeCAD macro path.
* Copy the [FCPD/Pure-Data folder](Pure-Data) content in a Pure-Data accessible location.

## Usage

### On the FreeCAD side

See [Sample/PdAccessObjectProperties.FCMacro](Sample/PdAccessObjectProperties.FCMacro) for an example

* Import the pdserver module in your macro
* instantiate a `pdserver.PureDataServer` object with address and port to listen to as constructor arguments
* set the default message handler i.e. function which take the incoming message as argument and return a stringable value to give back to Pure-Data
* with message handler registration system you can use different function depending of the first word of the incoming message. Simply call `PureDataServer.register_message_handler([first_word], function_to_call)`
* run the server with `pureDataServer.run()`

### On Pure-Data side

See [Sample/test.pd](Sample/animate.pd) for an example

* Use a `[declare -path ]` object to let Pure-Data know where to find the FCPD objects
* Add a `[fc_client]` object with these arguments
    1. Remote IP address of the (remote) computer on which the FreeCAD pdserver is listening
    2. Remote port of the (remote) computer on which the FreeCAD pdserver is listening
    3. Local port on which the remote FreeCAD can callback
    4. '`1`' for autoconnect (try to connect every second)
* Communicate with FreeCAD throw `[fc_process]`

PD objects are documented. Access help via Right mouse button(RMB) > Help

## More about messages

Pure-Data messages are text only so the FreeCAD objects have to be converted to and from string to be processed with Pure-Data.

The PureDataServer object automatically remove non-conform characters of the callback message i.e. `,` and `=` are converted to space and `';()\[\]{}"` are removed. In this way the `Base.Vector` python object whose string representation is `Vector(0.0,0.0,0.0)` can be a return value of a message handler and converted to `Vector 0.0 0.0 0.0` PD message.

## Feedback

Bugs, feature requests, and inquiries ? Please open tickets in the repo's issue queue.

## License

Copyright 2020 @flachyjoe and other contributors

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.
