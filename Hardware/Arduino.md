Introduction
------------
Welcome to the **Bridge library for Arduino**.
The components in this repository allow you to connect the Wunderbar Bridge/Grove module to the [Arduino UART](http://arduino.cc/) and in just 10 minutes have an easy gateway to the **relayr Cloud Platform**

This repository contains two libraries: one is the [WunderBarBridgeUno](https://github.com/relayr/Arduino-Bridge-Library/tree/master/libraries/WunderbarBridgeUno) for Arduino boards with one serial port and the second is the [WunderBarBridgeMega](https://github.com/relayr/Arduino-Bridge-Library/tree/master/libraries/WunderbarBridgeMega) for Arduino boards with multiple serial ports.

Boards such as the **[Arduino Uno](http://arduino.cc/en/Main/arduinoBoardUno)** only have one hardware serial port. Therefore,  the same port is used for the a USB connection with a computer and for the connection with the bridge module. 
For debugging purposes, you would need to connect an external USB serial converter to one of the software serial ports and use an external serial monitoring application.

Boards such as the **[Arduino Mega](http://arduino.cc/en/Main/arduinoBoardMega)** have three extra hardware serial ports. four ports in total. You can therefore set your debug/bridge connection port to be any of the 4 ports. The same goes for the bridge module. You are able to see the data sent and received via the Arduino default serial monitor 
when the debug port is set to 0, without the need for an external application. 

![Picture of the example](assets/arduinoBridge.gif)

## Getting Started

Below is a step by step tutorial of how to install and use the repository as well as a sketch example to get you inspired before you start your own project.

### 1. Installing the Library

There are two methods of installing the library. In both method, the desired library should be selected according to the Arduino board you are planning on using.

#### Method 1: Importing the *zip* file

* Download the repository as a **.zip** file from [here](https://github.com/relayr/Arduino-Bridge-Library/archive/master.zip) and unzip it.
* In the *[Arduino IDE software](http://arduino.cc/en/main/software)*, navigate to *"Sketch > Import Library"*. At the top of the drop down list, select the option to *"Add Library''*.
* You will be prompted to select the library you would like to add. Navigate to the *.zip* file's location and open it.
* Restart the *Arduino IDE* and go to the *"Sketch > Import Library"* menu. You should now see the library at the bottom of the drop-down menu. It is ready to be used in your sketch. The *Wunderbar Bridge* example should also be available in the list.

#### Method 2: Using *git clone*

* Clone the *Arduino IDE*.
* If you have **git** installed just go to the Arduino folder and perform a 

	`git clone https://github.com/relayr/Arduino-Bridge-Library.git`.
	
	On *Windows* machines, it is likely to be cloned under *"My Documents\Arduino\"*. For *Mac* users, it is likely to be cloned under *"Documents/Arduino/"*. On *Linux*, the clone will create a folder in your sketchbook.

* Restart the *Arduino IDE* and go to the *"Sketch > Import Library"* menu. You should now see the library at the bottom of the drop-down menu. It is ready to be used in your sketch. The *Wunderbar Bridge* example should also be available in the list.


----------


### 2. Flashing the UART firmware on the Bridge Module

In order to allow the *Bridge Module* to communicate with the *Arduino* over [UART](http://en.wikipedia.org/wiki/Universal_asynchronous_receiver/transmitter), it is necessary to flash the Bridge UART Firmware.

Follow our [instructions for flashing a Sensor Module](https://developer.relayr.io/documents/HowTos/Flashing) in order to replace the existing firmware on the Grove/Bridge module with the compatible firmware for connecting to Arduino.

The new firmware file is the ["sensor\_bridge\_fw\_UART.hex"](https://github.com/relayr/Arduino-Bridge-Library/blob/master/libraries/WunderbarBridgeUno/sensor_bridge_fw_UART.hex) file, located inside the */libraries* folder.


----------

### 3. Connecting  the Bridge Module to Arduino

#### *ArduinoUno*

The easiest way of connecting the two is using the [*"Grove Servo cable"*](http://www.seeedstudio.com/depot/grove-branch-cable-for-servo5pcs-pack-p-753.html?cPath=178_179) from Seedstudio. Below is the list of connections defined by Pin number / Name:

 **Bridge:** --------- **Arduino:** 

*  **1 - Tx** ----------> **0 - Rx**
* **2 - Rx** ----------> **1 - Tx**
* **3 - Vcc** ---------> **5 Volts**
* **4  - Gnd** ---------> **GND**

#### *ArduinoMega*

Since the ArduinoMega has multiple serial ports, you can connect the Bridge module to any of them:

**Bridge:** --------- **Arduino:** 

*  **1 - Tx** ----------> **0/1/2/3 - Rx**
* **2 - Rx** ----------> **0/1/2/3 - Tx**
* **3 - Vcc** ---------> **5 Volts**
* **4  - Gnd** ---------> **GND**


----------


### 4. Debugging Connection to the PC Serial Port 

#### *ArduinoUno*

In order to debug the application, and see, for instance, what data is coming from the cloud, it could be very helpful to connect a second serial port to the *Arduino*. For this, the library uses the *Software Serial Library* (which comes with the *Arduino IDE*, no need to import it). 

Any USB-to-Serial converter such as [this](http://www.amazon.com/PL2303HX-RS232-Cable-Module-Converter/dp/B008AGDTA4) should work. Below is the list of connections defined by Pin number / Name:

**Arduino:** ------ **PC Serial port:**

*  **10 - Rx** --------> **Tx**
* **11 - Tx** --------> **Rx**
* **GND** -----------> **GND**

**Note:** The example sketch defines *pins* 10 & 11 as the connecting ones to the *Software Serial* port, however, this can be changed to any available digital pin.

#### *ArduinoMega*

With the ArduinoMega you could use any port for debugging. The default debugging port in 0 as to allow you to debug via your computer:


**Arduino:** ------ **PC Serial port:**

*  **0 - Rx** --------> **Tx**
* **1 - Tx** --------> **Rx**
* **GND** -----------> **GND**

----------

### 5. Viewing The URAT Output

#### *ArduinoUno*

The output sent over UART can be seen with any **Serial Monitor Software**. Below we've listed two examples of Serial monitors which could be used for this purpose:


* For *Linux/OSx* the *"screen"* application comes with the OS. It can be used from the terminal with `screen /dev/tty.USBSerialXXX 115200` where XXX is the virtual serial port created when the cable is connected to the machine and 115200 is the [*baud rate*](http://en.wikipedia.org/wiki/Symbol_rate) used in the example. Other converters may use a different name base. 
List the *tty* devices under */dev* to get the correct name base.

* For *Windows*, a common serial monitor is ["PUTTY"](http://www.putty.org/). [Realterm](http://realterm.sourceforge.net/) also provides useful funcionalities. You may refer to their respective documentation for configuration options. See [PUTTY documentation](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html ) and [Realterm documentation](http://realterm.sourceforge.net/).

#### *ArduinoMega*

In order to view the output for the ArduinoMega you could use the Serial Monitor from your Arduino without the need for an external software.


## Using the library


#### *ArduinoUno*

1. Include the *SoftwareSerial* and *WunderbarBridge* headers:

		#include <SoftwareSerial.h>
		#include <WunderbarBridge.h>

2. Instantiate the Bridge class, including the *DEBUX_RX*, *DEBUG_TX* pin numbers and *baurate* to use: `Bridge bridge = Bridge(DEBUG_RX, DEBUG_TX, 115200);`

3. The serial port library calls a handler upon receiving a new byte in the buffer. Therefore, it is necessary to call the *processSerial()* method of the bridge library when this occurs:

		void serialEvent(){
		  bridge.processSerial();
		}

4. The *bridge.newData* flag will be set by the library when a new packet is received from the cloud. 
The received payload can be retrieved with the *getData()* method which returns a *bridge_payload_t* struct: `bridge_payload_t rxPayload = bridge.getData();`.

	The *bridge_payload_t* struct contains 2 fields: *length* (the payload length) and a uint8_t array (*payload[]*) which contains the received data.

5. To send data to the cloud, use the method *sendData(uint8_t payload[], int size);*
    
	**For example**:

		uint8_t dataOut[] = {1, 2, 3};
		bridge.sendData(dataOut, sizeof(dataOut));

#### *ArduinoMega*

1. Include the *WunderbarBridge* header:

		#include <WunderbarBridge.h>

2. Instantiate the Bridge class to use: 

		Bridge bridge = Bridge(115200);

	if you wish to connect your Bridge module to ports other than RX1 and TX1, use 

		bridge.setBridgePort(portnumber);

	if you wish to debug from a port other than 0 (your PC) use 

		bridge.setDebugPort(portnumber);

3. The serial port library calls a handler upon receiving a new byte in the buffer. Therefore, it is necessary to call the *processSerial()* method of the bridge library when this occurs (in the example below port Serial1 is used as the one to which the Bridge module is connected to): 

		if(Serial1.available())	{
		bridge.processSerial()
		}

4. The *bridge.newData* flag will be set by the library when a new packet is received from the cloud. 
The received payload can be retrieved with the *getData()* method which returns a *bridge_payload_t* struct: `bridge_payload_t rxPayload = bridge.getData();`.

	The *bridge_payload_t* struct contains 2 fields: *length* (the payload length) and a uint8_t array (*payload[]*) which contains the received data.

5. To send data to the cloud, use the method *sendData(uint8_t payload[], int size);*
    
	**For example**:

		uint8_t dataOut[] = {1, 2, 3};
		bridge.sendData(dataOut, sizeof(dataOut));



**Please Note**: The default port for connecting to the Bridge Module is 1. If you wish to use a different port, use `setBridgePort(portnumber)`with the desired port number. If you wish to change the debugging port use `setDebugPort(portnumber)`with the desired port number.

To get a bit more insight into using the library, have a look at the [Cloud Connection Example ](https://github.com/relayr/Arduino-Bridge-Library/blob/master/examples/BridgeCloudConnection/BridgeCloudConnection.ino)


## Testing the connection between the Bridge Module and the Master Module:

1. The Bridge Module has to be onboarded again after a [firmware change](https://developer.relayr.io/documents/Hardware/Flashing).
2. To verify that the Bridge Module is connected to the Master Module, simply initiate the  "TURN ON LED" command in the module's 'Settings' panel on the Developer Dashboard. If the LED turns on for about 5 seconds, the module is listening for commands from the cloud and  will also be able to send data.
3. Go to Device activity and paste the following command:
` {"path":"", "command":"down_ch_payload","value":[1,2,3]} `
Where [1,2,3] is the raw data sent to the bridge module (the payload).

We hope that you'd utilize our Grove Bridge in your future projects. The realm of possibilities is as vast as your imagination. Arduino is just the beginning. Stay tuned for more Grove delights to come!