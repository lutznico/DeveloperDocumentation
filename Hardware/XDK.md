# CONNECTING THE BOSCH XDK TO THE RELAYR CLOUD

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk1.png)

## Overview


The [Bosch XDK](http://xdk.bosch-connectivity.com/) is a prototyping platform from Bosch Connected Devices and Solutions for the Internet of Things - it is packed with state-of-the-art sensors and comes equipped with wireless connectivity via wifi and bluetooth, as well as a rechargeable Li-ion battery and microSD slot. Because it was developed to be essentially plug-and-play, it is easy to quickly create prototypes of systems to monitor environmental conditions such as temperature, humidity, ambient light, movement, magnetic fields, and sound. 

Currently these  devices are deployed in the Cisco Open Berlin Innovation Center to measure temperature, light, and noise levels, though other possibilities  include monitoring air quality in an office building, noise levels in a factory, or conditions leading to decay or corrosion in a warehouse such as humidity and temperature. When combined with the relayr dashboard for visualizing the sensor outputs, possibilities for connected solutions for your company are as great as your imagination!

## Creating your Bosch XDK on the relayr cloud

Start by downloading [GCC ARM Embedded](https://launchpad.net/gcc-arm-embedded/4.9/4.9-2015-q3-update), [Python 2](https://www.python.org/downloads/), the text editor of your choice (I used [Atom](https://atom.io/docs/v0.191.0/getting-started-installing-atom)) and the [XDK Workbench](http://xdk.bosch-connectivity.com/en/web/bcdscommunitygb/forum/-/category/using-xdk-downloads/10796-c), which we will need for the Makefiles included in it. If you are working on a Mac or Linux, you will also need the [Segger JLink debugger](https://www.segger.com/jlink-software.html) and debugger board hardware (these are not needed if you are on Windows). 

The Segger JLink utility and ARM GCC toolchain are needed to compile and flash XDK firmware on Mac and Linux (on Windows it is bundled with XDK Workbench and flashing is done via USB). Make sure that arm-none-eabi-gcc and JLinkExe binaries are accessible from command line. You might need to append paths to those tools to your shell PATH environment variable.

Now you can start setting up your device on the [relayr dashboard](https://developer.relayr.io/). Sign in or make an account, and go to “Your Devices”. Add a new device, and select “Create my Own Device”. 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk2.png)

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk3.png)

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk4.png)

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk5.png)

Give your device a name, and click  “Start Prototyping”.

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk6.png)

You will be taken to this screen - the important part here  is the “Prototype Credentials” box. Save the text here, because you will need these credentials later! 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk7.png)

You can now scroll down below the “Example Usage” code and click “Select a Model”. 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk8.png)

Select “Create new Model” and give it a name, then continue with “Start Modeling”.

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk9.png)

Click “Save”, then “Finish” in the relayr dashboard. 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk10.png)

##Configuring your XDK

Next, head over to the [XDK on Relayr repo](#) and clone it. Open it in your text editor, because we need to fill out a few specifics. 

First, navigate to xdk110/make/application.mk, to add some specifications for your system. The paths for Windows and Linux are already filled out, but you should double check to make sure they are correct and that it points to directory of your arm-gcc installation. For Mac you should add this directory yourself. 

Within this same file, change the variables “SHELLNAMES” and “SH” to your shell. Double check that the correct shell is named here, since yours might be different that the default option listed here. 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk11.png)

If you are not using OSX, change TOOLDIR to your OS. On Windows, uncomment the block of code below it , and comment out the second entry of SH, as well as the second entry for FLASH. 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk12.png)

Next, navigate to xdk110/app/relayrxdk/paho/MQTTClient/MQTTXDK.h and specify the name and password of your wifi network, inside double-quotes.

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk13.png)

Now, head to xdk110/app/relayrxdk/src/ and create a new file named “credentials.json”.  Paste in the “Prototype Credentials” you saved during device setup on the relayr dashboard. Add double-quotes around user, password, clientID, and topic, as shown above. 

Make sure all of your changes in the text editor are saved, then in the terminal, navigate to the /xdk/xdk110/app/relayrxdk/make directory if you aren’t there already. 

Plug in your Bosch XDK hardware to your computer and make sure it is powered on -   green LED will be on. If yours isn’t, make sure the switch is on and both the cable for  flashing/programming/debugging (and if on a Linux or Mac, the JLink debugger) and the USB cable  are plugged in. 

Once everything is connected, execute ‘make’ and ‘make flash’  from the terminal, and you should see live readings from the sensors! 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk14.png "Pictute 1")

Congratulations! You successfully connected your XDK to the relayr cloud! 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk15.png)

You can access the Device Activity log from this icon in the top right corner of your dashboard, which will also provide you with a command line to control your device.

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk16.png)

Try sending some commands to your XDK - for example you can turn the LEDs on and off by typing into the command line on the Dashboard.

	{"path" : "led", "command" : "<ledcolor>", "value" : "<0, 1, or 2>"}

* the options for ledcolor are : "red", "yellow", or "orange"
* the option for values are: 0 for off, 1 for on, and 2 to toggle

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk17.png)

Have fun creating, and feel free to leave a comment here or in the forums with questions or ideas for projects!


# BoschXDK Datatable in HTML
we are setting up a simple html file which shows the raw Data the XDK is publishing. For that we create a new text file and name it for example boschxdk.html. For simplicity we are creating only one file so js and css go in there too. Start with some basic headers:
	
	<!DOCTYPE html>
	<html lang="en-US">
	<title>Bosch XDK Data Table</title>

	<head>



##CSS
add CSS for centering, some loaders and the table. Feel free to add more and style differently
	
	   <!-- basic css -->
    <style>
    .center {
        text-align: center;
    }

this shows some loading circles while waiting for incoming data

    /*loading*/
    
    $circle-size:10vmin;
    * {
        box-sizing: border-box;
        position: relative;
    }
    
    body {
        background: #222;
        overflow: hidden;
    }
    
    .table {
        text-align: center
    }
    
    .circle {
        animation: spin 3s linear infinite both;
        background: #bada55;
        border-radius: 100vmax;
        /* margin:calc(50vh - 5vmin) calc(50vw - 1vmin); */
        /* Half the longest viewport width minus half the width/height of the circle. */
        margin: 60vh calc(50vw - 1vmin);
        height: 10vmin;
        position: absolute;
        width: 10vmin;
    }
    
    .circle + .circle {
        animation: spin 3s linear 1s infinite both;
        background: #10aded;
    }
    
    .circle + .circle + .circle {
        animation: spin 3s linear 2s infinite both;
        background: #a991e5;
    }
    
    @keyframes spin {
        0% {
            transform: rotate(360deg) translate(0vmax, -10vmax);
        }
        50% {
            transform: rotate(180deg) translate(0vmax, 0vmax);
        }
        100% {
            transform: rotate(0deg) translate(0vmax, -10vmax);
        }
    }

and the following is some basic css for the table, colors and hovering included.

    /*table css*/
    
    .readings {
        padding: 20px;
    }
    
    table {
        color: #333;
        font-family: Helvetica, Arial, sans-serif;
        width: 80%;
        border-collapse: collapse;
        border-spacing: 0;
    }
    
    td,
    th {
        border: 1px solid transparent;
        /* No more visible border */
        height: 30px;
        transition: all 0.3s;
        /* Simple transition for hover effect */
    }
    
    th {
        background: #10aded;
        /* Darken header a bit */
        font-weight: bold;
    }
    
    td {
        background: #FAFAFA;
        text-align: center;
    }
    /* Cells in even rows (2,4,6...) are one color */
    
    tr:nth-child(even) td {
        background: #F1F1F1;
    }
    /* Cells in odd rows (1,3,5...) are another (excludes header cells)  */
    
    tr:nth-child(odd) td {
        background: #FEFEFE;
    }
    
    tr td:hover {
        background: #666;
        color: #FFF;
    }
    /* Hover cell effect! */
    </style>


and load some JS libraries, we are using [jquery](https://jquery.com) and [bootstrap](http://getbootstrap.com) for html handling aswell as column configurations. Also dont forget to import the [relayr Browser SDK](https://github.com/relayr/browser-sdk).

	 	    <!-- some libraries -->
	    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.0.0-alpha1/jquery.js"></script>
	    <script src="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.0.0-alpha/js/bootstrap.js"></script>
	    <script src="https://developer.relayr.io/relayr-browser-sdk.min.js"></script>
	    <link rel="stylesheet" type="text/css" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.0.0-alpha/css/bootstrap.css">
	</head>

##Body
the body simply some text and a table in which we will add the incoming Data.The first part is just the header and the loading circles

	<body>
	    <div class="row">
	        <div class="col-md-12 center">
	            <h1>Bosch XDK Data Table</h1>
	        </div>
	        <div class="col-md-12 center">
	            <p>this visualization is directly connected to the relayr cloud. It retreives all the data and shows it in the table</p>
	        </div>
	    </div>
	    <div class="row">
	        <div class="col-md-12 center">
	            <div class="loader">
	                <div class="circle"></div>
	                <div class="circle"></div>
	                <div class="circle"></div>
	            </div>
	        </div>
	    </div>

next we define a readings `<div>` containing a table with dummy cells

	    <div class="row">
	        <div class="col-md-12 center">
	            <div class="readings">
	                <table id="mytable" class="table table-striped table-bordered" cellspacing="0">
	                    <thead>
	                        <tr>
	                            <th>temperature</th>
	                            <th>pressure</th>
	                            <th>humidity</th>
	                            <th>acceleration</th>
	                            <th>gyro</th>
	                            <th>magnetometer</th>
	                        </tr>
	                    </thead>
	                    <tbody>
	                        <tr>
	                            <td>dummy1</td>
	                            <td>dummy2</td>
	                            <td>dummy3</td>
	                            <td>dummy4</td>
	                            <td>dummy5</td>
	                            <td>dummy6</td>
	                        </tr>
	                    </tbody>
	                    <tfoot>
	                        <tr>
	                            <th>temperature</th>
	                            <th>pressure</th>
	                            <th>humidity</th>
	                            <th>acceleration</th>
	                            <th>gyro</th>
	                            <th>magnetometer</th>
	                        </tr>
	                    </tfoot>
	                </table>
	            </div>
	        </div>
	    </div>


##JS
html is done and we start typing the Javascript. In the beginning we define the credentials, obtained from relayr.io. (for further explanation of the relayr Browser SDK go to the [documentation](https://developer.relayr.io/documents/Browser/WebDevelopers)).

    <script>
    
    // get your credentials
    var AppID = "your app id";
    var toke = "your token";
    var XDK_deviceId = "your device id";

    // connect to thr relayr cloud
    var relayr = RELAYR.init({
        appId: AppID
    });

    // make vars for the table 
    var table = document.getElementById("mytable");
    var tbody = table.getElementsByTagName("tbody")[0];
    var rowindex = 0;

    // hide the readings while loading
    $(".readings").hide();

this method from the relayr Browser SDK connect to cloud and waits for incoming Data which we then manipulate. In the example here we change the cells in the table and update the values whenever a new reading arrives

    // call the getDeviceData and get your incomingData
    relayr.devices().getDeviceData({
        token: toke,
        deviceId: XDK_deviceId,
        incomingData: function(data) {

            // if nothing is incoming hide everything
            if (data.deviceId != XDK_deviceId) {

                $(".table").hide();
                $(".circle").show();

            } else {
                // if it is the same device id - remove CSS loading
                // and show the table
                $(".circle").hide();
                $(".readings").show();

                // make more vars for the table 
                var rows = tbody.getElementsByTagName("tr");
                var cells = rows[rowindex].getElementsByTagName("td")

                // check which reading
                if (data.readings[0].meaning == "temperature") {
                	//and update cell
                    cells[0].innerHTML = data.readings[0].value;
                }
                if (data.readings[0].meaning == "pressure") {
                    cells[1].innerHTML = data.readings[0].value;
                }
                if (data.readings[0].meaning == "humidity") {
                    cells[2].innerHTML = data.readings[0].value;
                }
                if (data.readings[0].meaning == "acceleration") {
                    cells[3].innerHTML = JSON.stringify(data.readings[0].value);
                }
                if (data.readings[0].meaning == "gyro") {
                    cells[4].innerHTML = JSON.stringify(data.readings[0].value);
                }
                if (data.readings[0].meaning == "magnetometer") {
                    cells[5].innerHTML = JSON.stringify(data.readings[0].value);
                }





            }
        }
    });
    </script>

and finally close everything

	</body>

	</html>
