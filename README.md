# FCPD FreeCAD/PureData connection framework

FCPD is a set of [FreeCAD](https://github.com/FreeCAD/FreeCAD) python macros and [Pure-Data](https://github.com/pure-data/pure-data) (aka PD) patches to link the two softwares.

## What is it for ?

FreeCAD is more than a CAD and Pure-Data is more than *"a free real-time computer music system"*.
Using a real-time visual programming language to control FreeCAD can results on a orchestrated animation system or a procedural modelling one.
On the other side we can imagine a real-time performance with visual/musical effects controled by FreeCAD virtual objects.

As you see your imagination (and your programmation skill) is the only limit of this couple.

## How to install ?

* Copy (or link) the [FCPD/FreeCAD folder](FreeCAD) content in your FreeCAD macro path.
* Copy the [FCPD/Pure-Data folder](Pure-Data) content in a Pure-Data accessible location.

## How to use ?

### FreeCAD side

See [Sample/PdAccessObjectProperties.FCMacro](Sample/PdAccessObjectProperties.FCMacro) for an example

* Import the pdserver module in your macro
* instantiate a pdserver.PureDataServer object with address and port to listen to as constructor arguments
* set the default message handler i.e. function which take the incomming message as argument and return a stringable value to give back to Pure-Data
* with message handler registration system you can use different function depending of the first word of the incomming message. Simply call `PureDataServer.register_message_handler([first_word], function_to_call)`
* run the server with `pureDataServer.run()`

### Pure-Data side

See [Sample/test.pd](Sample/animate.pd) for an example

* Use a `[declare -path ]` object to let Pure-Data know where to find the FCPD objects
* Add a `[fc_client]` object with these arguments
    1. address of the (remote) computer on which the FreeCAD pdserver is listening
    2. port of the (remote) computer on which the FreeCAD pdserver is listening
    3. **local** port on which the remote FreeCAD can callback
    4. **1** for autoconnect (try to connect every second)
* Communicate with FreeCAD throw `[fc_process]`

PD objects are documented : right-click/help on them.

## More about messages

Pure-Data messages are text only so the FreeCAD objects have to be converted to and from string to be processed with Pure-Data.

The PureDataServer object automaticaly remove non-conform characters of the callback message i.e. **,** and **=** are converted to space and **';()\[\]{}"** are removed. In this way the Base.Vector python object whose string representation is "Vector(0.0,0.0,0.0)" can be a return value of a message handler and converted to "Vector 0.0 0.0 0.0" PD message.

