# Flashing a Sensor Module and replacing its Firmware - An Experimental Feature

Since some of our extra Sensor Module functionalities require that the current firmware on it be replaced, we've put together a step by step tutorial for flashing a sensor module. Please find below a version for Android and iOS users. 

### For Android Users

1. Copy the ".hex" file form its location onto your Phone
2. Start the Manager app, select the sensor you would like to flash and activate the BLE direct Connection mode(To do so simply swipe to the left and activate BLE mode)
3. Select 'Update Firmware' and choose the option to 'select from folder'
4. Place Module in <i>firmware update mode</i>. You can do this by pressing the button on the module and holding it for 3 seconds. The LED will be constantly on when <i>firmware update mode</i>is activated.
5. Follow the instructions in the app to flash the firmware 

You can always revert to the original sensor functionality by flashing the modules and re-installing the original sensor firmware. The original firmware for each of the sensor modules can be downloaded from <a href="https://s3-eu-west-1.amazonaws.com/relayr-firmware/BLEModulesFirmware-20140910.zip"> this link </a>. 

### For iOS Users 

1. Download the new firmware file from the place indicated.
2. In case you have not downloaded the firmware to your phone, copy it to your phone.
3. Install the Nordic DFU (Device Firmware Update) application. The application compatible with Android is the **nRF Toolbox**, which can be downloaded from the <a href="https://itunes.apple.com/us/app/nrf-toolbox/id820906058?mt=8">Apple Store</a>
4. Put the Sensor Module which you would like to flash in *DFU Mode*. You can do this by pressing the button on the module and holding for **3 seconds**. The LED will be constantly on when the DFU mode is activated.
5. Connect to the Module and open the DFU pane.
6. In the nRF Toolbox, open the DFU Utility. 

<img src="assets/fourth.png" class="center" width=300px>

7. Select "Application" in the Select File Type setting.
8. Click "Select Device" - the module should appear in the list of available BLE devices.
9. Select "Upload" to upload the new firmware onto the sensor module


Remember, you can always revert to the original sensor functionality by flashing the modules and re-installing the original sensor firmware. The original firmware for each of the sensor modules can be downloaded from <a href="https://s3-eu-west-1.amazonaws.com/relayr-firmware/BLEModulesFirmware-20140910.zip"> this link </a>. 