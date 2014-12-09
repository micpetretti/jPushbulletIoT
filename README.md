jPushbulletIoT
==============
The code can be used for communication on an arbitrary Java enabled programmable platform. First an object of the PushbulletDevice Class has to be created, which will register the device to the service.

	PushbulletDevice myDevice = new PushbulletDevice();

This will either represent a physical device in the digital space or it can be a digital device only, for example to provide a service like a logger. Every device registered to the service can be represented with the DeviceEntry Class. Every Note is represented as a PushEntry Objects. They have the following fields:
```
public class PushEntry {
	public String title;		// String for the title.
	public String body;			// String for the message.
	public String pushIden; 	// ID generated by the service.
	public double created;		// Unix-time, moment of sending.
	public String sourceDevice;	// ID of the sending device.
	public String targetDevice;	// ID of the receiving device.
}
```

```
public class DeviceEntry {
	public String nickName;		// Unique nickname.
	public double created;		// Unix-time, moment of creation.
	public String iden;			// ID generated by the service.
}
```

The created device can now be used to carry out the communication tasks.

```java
// First there are some getter-methods for handling devices informations:
String myDevicesNickname = myDevice.getNickname();

Double creationTime = myDevice.getTimestamp();

String myDevicesID = myDevice.getIden();

// Get all information about a certain device, registered to the service.	
DeviceEntry certainDevice = myDevice.getDeviceEntry(devicesNickname);


// Reading all notes for myDevice.
List<PushEntry> listOfAllNotes = myDevice.read();

// Pushing a Note. Either create a new one or to answer a push modify one of the read ones.
PushEntry myNote = new PushEntry(title, body, targetDevice, sourceDevice);
myDevice.push(myNote);

// Delete one of the pushes from the server. 
PushEntry oldNote = listOfAllNotes.get(x);
myDevice.deletePush(oldNote);

// Send a file.
File myFile = new File(pathToTheFile);
String urlToMyFile = myDevice.pushFile(myFile);
PushEntry urlNote = new PushEntry(title, urlToMyFile, targetDevice, sourceDevice);
myDevice.push(urlNote);
```

Now a .jar file has to be created, to be deployed on the target machine. Located in the same folder as the archived Java program, there must be a "general.properties" named file containing information that will be read by the constructor. The file must contain three lines. URL is the url to version 2 of the used Pushbullet API. The ApiKey can be found at \url{https://www.pushbullet.com/} after log in, in the section "Account Settings". And the deviceName must be chosen unique for the project.

URL = https://api.pushbullet.com/v2
apiKey = xXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxX
deviceName = myDevice

