 # SanBox (iOS Framework)


## Introduction

SanBox framework is be used for finding nearby iBeacon hardware and performing sanitizing processes. Detailed workflow is described below.


## Workflow Design

![Workflow design](/Resource/designConcept.jpg)

## Installation

Download or clone the SanBox framework on your system. It contains a development and distribution framework file. Please note that as per apple guidelines the distribution bundle should contain only device supported files. 

Development framework contains architecture files for running both devices and simulators.

Drag and drop the SanBox.framework file into your Xcode project. Make sure that "Copy Items to Destination's Group Folder" is checked. 


![Import Framework](/Resource/importFramework.jpg)


Navigate to the project settings -> target -> General -> Framework, libraries and Embedded content and select Embed and Sign option.


![Change settings](/Resource/changeStatus.jpg)


## Configure your Project

In Info.plist, add a new fields, NSLocationAlwaysUsageDescription, NSLocationAlwaysAndWhenInUsageDescription, NSBluetoothPeripheralUsageDescription with relevant values that you want to show to the user. This is mandatory for iOS 10 and above.


## Set up 

Import the framework header in your class

```swift
import SanBox
```

Initialize SandBoaxManager

```swift
let beacon =  Beacon(uuid: UUID(uuidString: uuidString)!, major: major,, minor: minor, identifier: randomString(length: 5), beaconName:identifier)
let sanBoxManager = SanBoxManager(beacons:[beacon],proximity:SBProximity(rawValue:1))
sandBoxManager.delegate = self
sandBoxManager.isNotificationEnabled = true
sandBoxManager.startmonitoring()
```

Beacon defines added devices, creating a sanBoxManager instance requires valid beacon reference.

## Sanitization Status

```swift
NotSanitized (the device is not sanitized)

InProgress (started sanitization)

AwaitingForAcknowledgement (waiting for the beacon to turn on)

AcknowledgementInProgress (Beacon is turned on and receiving signals)

Sanitized (Device is sanitized)

Failed (sanitization process failed)

UnKnown (default initialization status, it will override after checking the last sanitization details)
```


## Protocol/Callback

```swift
didEnteredBeconRegion(beacon:Beacon?)
Get called when the device enters beacon region
- Parameters:
- beacon: beacon reference that enters the specific region
didExitedBeconRegion(beacon:Beacon?)
get called when the beacon exited from the specific region
- Parameters: - beacon: beacon reference that exited from  the specific region
didFailedRegionMonitoring(beacon:Beacon?)
get called when the beacon failed to monitor the changes in the specific region.
- Parameters:
- beacon: beacon reference that failed to complete the sanitizing process.
fireLocalNotification(message:String)
used for firing local notifications
- Parameter message: message for showing in the notification bar.
fireImageNotification(status:SanitisationStatus)
if the image notification is enabled, then fireImageNotification method will be triggered for showing the beacon status in the listening controller.
- Parameter status: return the status of the beacon.
didExpirePeriodReached()
This method will get called when the sanitized device’s expire period reaches.
didUpdatedSanitisationStatus(beacon:Beacon?,status:SanitisationStatus)
This method is used for notifying listening controllers that the hardware sanitizing status changed.
- Parameter status: current status of the device.
- Parameter beacon : represents the ibeacon device.
didFailedAuthorization(error:AuthorizationError)
Used for informing the listening controller, when any kinds of authorization related issue comes. Authorization errors are listed below.
 - LocationServicePermissionDenied ( location service permission is not granted)
- BluetoothIsUnSupported (Bluetooth hardware failure)
-BluetoothTurnedOff (Bluetooth power off)
```


