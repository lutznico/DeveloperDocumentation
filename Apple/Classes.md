# Basic Classes

The `Relayr.framework` includes a small subset of useful classes, which allow you to communicate with the relayr cloud, receive sensor data and manage users, devices transmitters and other entities. At the moment The BLE Direct Connection Classes are not fully implemented but they should be available in upcoming releases. The classes indicated below are all related to App > Cloud > Device communication.  
All calls are asynchronous and the server response time is proportional to the quality of your connection and the size of the response requested.


### *RelayrCloud*

Used as a static class to receive various statuses from the relayr servers.


	[RelayrCloud isReachable:^(NSError* error, NSNumber* isReachable){
	    if (isReachable.boolValue) {
	        NSLog(@"The Relayr Cloud is reachable!");
	    }
	}];
	
	[RelayrCloud isUserWithEmail:@"marcos@relayr.de" registered:^(NSError* error, NSNumber* isUserRegistered) {
	    if (!error && isUserRegistered.boolValue) {
	        NSLog(@"The user is registered on the platform");
	    }
	}]


### *RelayrApp*

A representation of your iOS/OSX app on the relayr Cloud Platform. This object is required in order to interact with the Relayr services (it is worth noticing that this is not a singleton and theoretically, you can define as many relayr apps as you wish).

An instance of `RelayrApp` can be created by passing the credentials (*appID*, *OAuthClientSecret*, and *redirectURI*) obtained from the [Developer Dashboard](https://developer.relayr.io/dashboard/apps/myApps), when you define the application to use the relayr Cloud Platform.

	
	[RelayrApp appWithID:@"..." OAuthClientSecret:@"..." redirectURI:@"..." completion:^(NSError* error, RelayrApp* app){
	    if (app) {
	        NSLog(@"Application with name: %@ and description: %@" app.name, app.description);
	        self.app = app;
	    }
	}];


You can check your app's properties, query the server for information related to it, or sign users in and out of it. You can have as many logged in users as you want.


	RelayrApp* app = ...;
	[app signInUser:^(NSError* error, RelayrUser* user){
	    if (user) {
	        NSLog(@"User logged with name: %@ and email: %@", user.name, user.email);
	    }
	}];


Storing users isn't necessary, since they are stored within the `RelayrApp` instance. You can query the app for logged users by initiating this method:


	RelayrUser* user = app.loggedUsers.lastObject;
	// Or...
	RelayrUser* user = [app loggedUserWithRelayrID:@"..."];


### *RelayrUser*

Represents a logged-in user. Users can access device data, they can query transmitters/devices they own, bookmark favorite devices, and become app publishers.


	RelayrUser* user = ...;
	NSLog(@"User with name: %@ and email: %@", user.name, user.email);

	// Lets ask the platform for all the transmitters/devices own by this specific user.
	[user queryCloudForIoTs:^(NSError* error){
	    if (error) { return NSLog(@"%@", error.localizedDescription); }
	
	    for (RelayrTransmitter* transmitter in user.transmitters)
	    {
	        NSLog(@"Transmitter's name: %@", transmitter.name);
	    }
	
	    for (RelayrDevice* devices in user.devices)
	    {
	        NSLog(@"Device's name: %@", devices.name);
	    }
	}];


### *RelayrTransmitter*

An instance representing a *Transmitter*. A transmitter is one of the basic Relayr entities. A transmitter, contrary to a device, does not gather data but is only used to *relay* the data from the devices to the relayr cloud platform. The transmitter is also used to authenticate the different devices that transmit data via it.

In the case of the Relayr WunderBar, the transmitter is the Master Module in the Cloud Platform scenario (data being sent from the sensors by the Master Module to the Relayr cloud over MQTT/SSL). In the future case of direct connection an app running on your phone could serve as a transmitter.


	RelayrTransmitter* transmitter = user.transmitters.anyObject;
	NSLog(@"This transmitter relays information of %lu devices", transmitter.devices.count);
	for (RelayrDevice* device in transmitter.devices)
	{
	    NSLog(@"Device name: %@, capable of measuring %lu different values", device.name, device.readings.count);
	}


### *RelayrDevice*

An instance representing a *Device*. A device is another basic relayr entity. A device is any external entity capable of producing measurements and sending them to a transmitter to be further sent to the relayr platform, or one which is capable of receiving information from the relayr platform.
Since a single relayr device can produce more than one reading at the same time, you should always query device capabilities prior to executing any other commands.


	RelayrDevice* device = transmitter.devices.anyObject;
	NSLog(@"Device manufacturer: %@ and model name: %@", device.manufacturer, device.modelName);
	
	for (RelayrReading* reading in device.readings)
	{
	    NSLog(@"This device can measure %@ in %@ units", reading.meaning, reading.unit);
	    NSLog(@"Last value obtained by this device for this specific reading is %@ at %@", reading.value, reading.date);
	}


The most reliable way to obtain data is to subscribe to a reading of a relayr device:


	// You can choose to subscribe with a block (it will be executed every time a new value is received):
	[device.readings.anyObject subscribeWithBlock:^(RelayrDevice* device, RelayrReading* reading, BOOL* unsubscribe){
	    NSLog(@"Value received: %@ from device: %@", reading.value, device.name);
	    if (/* Many values have been read */) { *unsubscribe = YES; }
	} error:^(NSError* error){
	    NSLog(@"An error occured while subscribing, please try again or blame someone else for everything...");
	}];
	
	// Or you can choose to receive subscription values via the target-action mechanism:
	[device.readings.anyObject subscribeWithTarget:self action:@selector(dataReceivedFrom:) error:^(NSError* error){
	    NSLog(@"An error occurred while subscribing");
	}];
	
	- (void)dataReceivedFrom:(RelayrReading*)reading
	{
	    NSLog(@"Value received: %@", reading.value);
	}


### *RelayrReading*, *RelayrWriting*, and *RelayrConnection*

These are object abstractions of a device's readings and writings, and a connection either to a transmitter or a different element on the platform.

You can query their properties for the following information:

* `RelayrReading` ***for data received from the device***. The type of data received is listed as a `meaning` and is measured in `unit` units.

	
		  RelayrDevice* device = ...;
		  for (RelayrReading* reading in device.readings)
		  {
		      NSLog(@"The measurement for %@ is: %@", reading.meaning, reading.value);
		  }
	  

* `RelayrWriting` ***for signals that the device can receive***. It can be infrared, or Grove signals.

		  RelayrDevice* device = ...;
		  for (RelayrWriting* writing in device.writings)
		  {
		      NSLog(@"");
		  }
  

* `RelayrConnection` to query the connection state (connected, disconnected, resetting, etc.), and the connection type (BLE, Wifi, etc.). You can even subscribe to changes in the connection channel (for example, be informed when you are in close proximity to a device or when the WiFi connection of your device is interrupted).

The `RelayrConnection` and `RelayrWriting` have not yet been implemented. These are currently being implemented and will be available in an upcoming release.
